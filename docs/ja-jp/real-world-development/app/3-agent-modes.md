---
title: "レッスン 3 - Agent mode: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue からフィルタリングを実装し、Interactive で結果をレビューして検証します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

最初にプロジェクトへ小さな機能を追加しました。しかし、大きな変更には、より確実なプロセスが必要です。GitHub Copilot app は組織の既存のワークフローに沿って動作し、適切な方法で必要なものを構築できます。このレッスンから数回にわたり、一般的な agent 主導の開発プロセスを実践します。Issue から新機能を実装し、コードと機能の動作を確認して、プロジェクトにマージします。

> [!NOTE]
> 機能のワークフローを進める間は、同じ session を使います。通常はファイルの種類ごとに異なる session や PR を使いますが、ここでは主要な概念に集中できるよう手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい agent session を始めます。
- **Plan** mode で要件を定義します。
- **Autopilot** mode で新機能を実装します。
- コードをレビューします。
- browser canvas で機能を手動検証します。

この機能の作業を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA agent を作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリやパブリッシャーでゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意が必要な詳細もあります。Plan mode で方針を決めてから、範囲を限定した実装を Autopilot に許可します。

## 背景

AI coding agent を開発ワークフローに導入しても、基本は変わりません。むしろ重要性が増します。多くの開発者は次のような流れで作業します。

1. 必要な作業を説明する Issue を開きます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合、上記の流れを基にしています。

この標準的な手順に従うと、AI が生成したコードも要件を満たし、手作業で書いたコードと同じ審査を受けることを確保できます。

## Session mode

**session mode** は agent の自律性を制御します。prompt field の下にある dropdown で設定し、いつでも変更できます。

- **Interactive**: agent と共同で作業します。agent は変更を提案し、続行前に入力を待ちます。
- **Plan**: agent は最初に計画を作成します。実行前に計画をレビューして承認します。
- **Autopilot**: agent がコードを書き、テストを実行し、入力を待たずに反復して自律的に作業します。

最初に Plan mode で計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から session を開始する

星評価の PR がマージされ、ローカルの `main` が最新であることを確認します。

1. sidebar の **Sessions** で Tailspin Toys repository folder を選択し、repository の上部ナビゲーションで **Issues** を選びます。**Allow users to filter games by category and publisher** を開きます。これはすでに用意された Issue です。新たに作成する必要はありません。
2. **New session** を選択し、更新済みの `main` を基にした **new working tree** を選びます。

    ![GitHub Copilot app の Issue view で New session button を矢印で示す図](../../../_images/app-new-session-from-issue.png)

3. Issue が session に追加されていることを確認し、mode selector から **Plan** を選択します。

> [!NOTE]
> Node.js または npm が見つからない場合は、agent に `node --version` と `npm --version` の実行を依頼します。通常の terminal では実行できるのに session で失敗する場合は、app の PATH または version manager の設定を確認して、app を完全に終了して再起動します。失敗した操作とチェックを再試行してください。agent が npm を見つけられないという理由だけで npm を再インストールしないでください。

## フィルタリング機能を計画する

計画を立てると、Copilot がコードを書く前に方針をレビューできます。Issue から開始したため、Copilot は機能要求をコンテキストとして把握しています。次の prompt を送信します。

```plaintext
Build this feature.
```

Copilot の質問に答え、Issue の受け入れ条件と計画を比較します。カテゴリとパブリッシャーの filter、アクセシブルな control、data-access の変更、テストが含まれていることを確認します。複数カテゴリの組み合わせや、一致するゲームがない場合の動作など、不明な点を話し合います。

計画には、プロジェクト既存の tool を使う lint、unit test、E2E test、型チェックが含まれている必要があります。計画はフィルタリングの実装とテストに集中させます。品質改善のワークフローを終えてから PR を作成します。承認前に必要な変更を依頼し、後で検証できるよう Issue の URL と合意した内容を控えておきます。

## Autopilot を明示的に承認する

計画に問題がなければ、**Approve and implement this plan** または使用中のバージョンで同等の項目を選択します。承認すると mode が切り替わると決めつけず、現在の mode を確認してください。indicator が **Autopilot** であることを確認します。

Copilot が計画に沿って実装し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があるため、先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックの完了を判断する前にセットアップの問題を解決します。自分で起動した server のみ停止してください。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同様にマージ前にレビューします。

1. **Changes** を開き、フィルタリングの実装とテストを確認します。
2. 結果を Issue と承認済みの確認事項 (複数カテゴリやパブリッシャーの組み合わせなど) に照らして確認します。既存のリポジトリ instruction に従っていることも確認します。
3. lint、unit test、E2E test、型チェックの出力を調べます。省略されたチェックは成功ではありません。
4. 実装を承認する前に失敗を解決し、影響するチェックを再実行します。Playwright の E2E 設定は preview を build して提供し、ローカル server を再利用できます。別の worktree でなく、この作業の server がテストされたことを確認してください。

## 新機能を試す

コードを確認しました。次は、browser canvas でサイトを開き、実際に動作することを確認します。

1. 次の prompt で Copilot に app を起動し、browser canvas にページを開くよう依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくすると app が起動し、Copilot app 内にブラウザーが表示されます。
3. カテゴリを選択して結果を確認します。filter を解除し、パブリッシャーを選択して結果を確認します。
4. カテゴリとパブリッシャーの filter を組み合わせ、Issue と承認済みの計画に照らして結果を確認します。複数カテゴリが使える場合は、合意した組み合わせの動作も確認します。
5. 一致するゲームがない組み合わせを試し、空の状態を確認します。filter を解除またはリセットし、全件が再表示されることを確認します。
6. 次の prompt で session 用に起動した dev server を停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 次のレッスン用に session を保持する

フィルタリングの PR はまだ作成またはマージしないでください。レッスン 4 から 8 まで同じ session、worktree、branch を使います。次はフィルタリングのコードに custom instruction を適用し、その後 skill と QA を改善してからレッスン 8 で PR を作成・マージします。dev server を停止したら session はそのままにし、次のレッスンで再開してください。

## まとめと次のステップ

異なる agent mode を使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい agent session を開始しました。
- **Plan** mode で要件を定義しました。
- **Autopilot** mode で新機能を実装しました。
- コードをレビューしました。
- browser canvas で機能を手動検証しました。

次は、文書化されたプラクティスに沿ってコードを生成するため、[custom instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot app で agent session を使う][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
