---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agents →][next-lesson]

agent mode でフィルタリング機能を作成しました。プル リクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを自分で操作する代わりに、**Playwright MCP server** を接続して、Copilot に実際のブラウザーを操作させます。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP server が Copilot にツールを追加する仕組みを学びます。
- Playwright MCP server をワークスペースに追加します。
- Copilot に Playwright MCP server を使わせて、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリングの PR を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外にアクセスできると、より多くの作業をこなせます。Model Context Protocol (MCP) は Copilot が外部ツールやサービスとやり取りするための標準的な仕組みです。

![agent mode の内部と、コンテキスト、LLM、MCP server や VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI agent は外部ツールやサービスと通信できます。MCP により、agent は resource から最新情報にアクセスし、tool を使ってユーザーに代わって操作できます。

MCP server は AI agent と外部ツールやサービスとの橋渡しをし、通信を管理します。既存 API や npm package などのローカル ツールに接続できます。各 MCP server は、agent が利用できるツールと resource のセットを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プル リクエストを管理する API にアクセスできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を提供します。Web ページの移動、フォーム入力、ボタン操作などを agent が実行できます。

ほかにもさまざまなツールと resource を利用できる MCP server があります。GitHub はエコシステムの発見と貢献を促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティ面では、MCP server をプロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードと発行元を確認し、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP 構成を確認する

`.vscode/mcp.json` は、この VS Code workspace で利用できる MCP server を構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` server がすでに構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` entry はプロジェクト template に含まれています。前の演習で Copilot がバックログの Issue を読めたのはこのためです。[リモート GitHub MCP server][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めて tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は Copilot が操作できるブラウザーを提供するため、機能のテストに使えます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` entry を追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` server と異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して server を起動します。`--headless` を指定すると、デスクトップ画面がない codespace でも Playwright が表示ウィンドウなしでブラウザーを実行できます。

> [!NOTE]
> Tailspin Toys は end-to-end test に Playwright を使うため、必要なブラウザーは通常インストール済みです。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。Copilot が初めて server の tool を必要とすると、VS Code が server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上に **Start** 操作が表示されます。選択して server を起動し、構成を確認します。
2. VS Code で server の信頼を求められたら、構成を確認して信頼を選択します。

手動での起動は省略できます。その場合、次の手順で Copilot が tool を必要としたときに VS Code が server を起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] に接続すると、Copilot は実際のブラウザーを操作できます。agent はアプリを開き、ページを移動し、フィルターを適用して結果を読み取り、確認した内容をまとめます。会話を離れずに、機能が期待どおり動作するか確認できます。

Playwright MCP server はスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。agent はボタン、リンク、リスト項目など、支援技術と同様に構造化されたラベル付き要素を読み取ります。機能の簡易テストが、アクセシビリティの簡易確認にもなります。

server を接続し、アプリを起動した状態で、次のように Copilot にフィルタリング機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server でブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ条件と照らして確認します。問題があれば、質問を続けるか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> このテストではアプリが `http://localhost:4321` で実行されている必要があります。dev server を停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP server を初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら `npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能の動作を確認できたので、チームがレビューできるようにプル リクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** panel を開き、Copilot が作成した変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを選択して commit message を生成します。

   ![変更内容を表示する Source Control panel のスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プル リクエストを作成する

github.com や GitHub command-line interface (CLI) などから PR を作成できます。ここでは GitHub Copilot に任せます。関連する Issue を見つけ、関連付けて PR を作成できます。

1. Copilot Chat panel を開き、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub tool を使えるように、agent の一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めたり操作したりするときは、必要に応じて **Continue** を選択します。GitHub tool の初回利用時に認証を求められた場合は、案内に従ってください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP server が Copilot にツールを追加する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- Playwright MCP server を使い、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングのプル リクエストを開きました。

MCP server を機能の**テスト**に使いました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなど、対象を絞ったタスクを効率化する[custom agent][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
