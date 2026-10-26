---
title: "レッスン 3 - agent mode: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue のフィルタリングを実装し、Interactive mode で結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、プロジェクトへ小さな機能を追加しました。しかし、大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作するため、適切な方法で適切なものを構築できます。このレッスンから数回にわたり、一般的な agent 主導の開発プロセスを実践します。Issue から新機能を実装し、コードと機能の動作を確認してから、プロジェクトにマージします。

> [!NOTE]
> 機能のワークフローを進める間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中できるよう手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を開始します。
- Plan mode で要件を定義します。
- Autopilot mode で新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリやパブリッシャーでゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な詳細もあります。Plan mode で方針を決めてから、範囲を限定した実装を Autopilot に許可します。

## 背景

AI coding agent を開発ワークフローに導入しても、基本は変わりません。むしろ重要性が増します。多くの開発者が次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合、上記の流れを基にしています。

この標準的な手順に従うと、AI が生成したコードも要件を満たし、手作業で書いたコードと同じ審査を受けることを確保できます。

## 会話モード

**会話モード**は、agent がどの程度自律的に動作するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: agent と共同で作業します。agent は変更を提案し、続行前に入力を待ちます。
- **Plan**: agent は最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: agent がコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

最初に Plan mode で計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止中の場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub の Issue やプル リクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能やタスクの実装を Copilot に任せる前に、方針を決められます。複雑な作業では、計画に時間をかけることをお勧めします。Plan mode に切り替え、Copilot に計画を作成させます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan mode に切り替えます。プロンプトの下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue reference の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を見つけて計画を作成します。

3. フィルタリングの動作について Copilot から質問されたら、希望に基づいて回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. スクロールして計画を確認します。
6. 決定内容に合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を進めさせます。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックの完了を判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive mode に切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画で決めた内容に照らして確認します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認しました。次は、Codespaces の転送ポートからサイトを開き、実際に動作することを確かめます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、パブリッシャー、および計画で合意した組み合わせでフィルタリングを試します。
4. リセットと結果が 0 件の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot 会話を開始しました。
- Plan mode で要件を定義しました。
- Autopilot mode で新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、文書化されたプラクティスに沿ってコードを生成する方法を学ぶため、[custom instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
