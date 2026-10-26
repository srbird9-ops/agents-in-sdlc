---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能要望など、さまざまな例があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行します。作業が完了すると、人間の開発者が確認できるドラフト PR を作成します。これにより、手間のかかるタスクを任せて開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確に定めることの重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。サイトの初期バージョンを開発した請負業者は、ドキュメントを十分に整備していませんでした。実際、ドキュメントがまったくないことに気づくでしょう。まず、アプリケーション内のすべてのエクスポート関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルを関連ゲームとして表示し、ユーザーが引き続きゲームを探せるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現時点では作業がブロックされていますが、より優先度の高い issue がほかにあります。

このようなタスクは優先度が下がりやすいため、Copilot cloud agent に任せるのに適しています。Copilot cloud agent が非同期で作業している間、開発者はほかのタスクに集中できます。その後、作業内容を確認し、期待どおりであることを確かめられます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者と同様に、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクに着手します。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 適切な範囲の命令が重要な理由

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見単純な操作にも、詳しく見ると複雑な部分があります。そのため、[Copilot cloud agent に依頼するタスクの範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、それが Copilot の動作を導く仕組みを確認しました。カスタム命令の演習をすでに完了している場合は、`.github/copilot-instructions.md` がどのように使われるかを確認済みです。まだの場合は、ここでファイルを簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれている命令ファイルを読み取り専用で確認しておくと、後でその影響を見分けやすくなります。

次のファイルを、リポジトリの GitHub Web UI または起動済みの codespace で開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターに注目してください。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるときは、作成されるプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。これらは命令ファイルで指定されています。後の演習で PR をレビューするときにも、あらためて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が関わるかにかかわらず、コードの作成には通常、特定の環境とセットアップスクリプトが必要です。これにより、環境を整え、すべての準備が整っていることを確認します。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じように作業を進めます。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。作業開始前に実行する[専用のセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボでは、学習をスムーズに進められるよう、あらかじめ設定されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードが適用された SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一環としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド／アプリのセットアップ — Node.js（アプリ全体が Astro + Drizzle/libSQL になりました）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様の形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーに注目してください。これにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

誰もがドキュメントの重要性を理解していますが、情報が古いか、ドキュメント自体が不足しているプロジェクトは少なくありません。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者のチームへの参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成し、後の演習で作業内容を確認できます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールしながら、Copilot がどのように問題を解決しているかを確認します。コードの調査や状態の把握、計画を立てるための思考と判断、コードの作成などが行われます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を活用して、Copilot cloud agent にもう 1 つのタスクを割り当ててから、アプリケーションに機能を追加する作業に移ります。

## ゲームの詳細ページに関連ゲームのセクションを追加する

先ほど説明したように、GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は、それほど時間がかからないかもしれませんが、その時間をほかのタスクに使うこともできます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、進捗を常に監視する必要はありません。完了後に確認すればよいのです。それでは、カスタムエージェントの作成と利用について見ていきましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を使いました。cloud agent に issue を割り当て、非同期に作業を進めてもらえます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに Copilot を活用できます。

次の概念を確認しました。

- コード生成の環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確に定めることの重要性を理解する。
- issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みましょう。[Copilot cloud agent では MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
