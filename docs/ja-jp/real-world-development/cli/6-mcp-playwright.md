---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加することも必要です。MCP server を使うと、Copilot は CLI に組み込まれた機能を超えて、追加のツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びます。
- まだ利用できない場合は Playwright MCP server を追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

unit test と end-to-end test は重要ですが、UI の更新を検証するには実際に操作する必要があります。Copilot が開発中のサイトをユーザーのように操作できるようにすると、変更が期待どおりに動作するか確認する作業を自動化し、信頼性を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部のツールやサービスと通信する仕組みです。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムに通信し、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

これらのツールやリソースには MCP server を介してアクセスします。MCP server は、AI agent と外部のツールやサービスをつなぐ橋渡しをします。それぞれの MCP server は、AI agent が利用できるツールとリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP server があります。GitHub はエコシステム内のサーバーを見つけやすくし、貢献を促すために[MCP registry][mcp-registry]をホストしています。

> [!CAUTION]
> MCP server も、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot がユーザーのようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに構成されている場合は、セットアップを省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright を使って Copilot に機能を確認させる

これまでは、期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP server を使って、同じ確認を行います。

1. 次のプロンプトで、Playwright MCP server を使って機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、MCP server を自動で見つけるため、明示的に伝える必要はありません。ただし、利用するツールがわかっている場合は指示すると、意図した方向に進みやすくなり、より安定した結果につながります。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作しているか確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、サーバーを停止してからレポートを作成します。

## まとめと次のステップ

おめでとうございます。Playwright MCP server を使って、Copilot CLI から実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びました。
- 未構成の場合は Playwright MCP server を追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、skill とブラウザー ツールを専門的な役割にまとめる[QA custom agent][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
