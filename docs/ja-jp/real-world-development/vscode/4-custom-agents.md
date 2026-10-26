---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

GitHub Copilot の [custom agent][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI assistant を作成できます。リポジトリの `.github/agents` folder に Markdown file を置いて agent を定義し、対象業務に合わせた instruction、ベスト プラクティス、コード パターン、専門知識を Copilot に提供します。チームの知識を再利用可能な agent としてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ agent、安全なコーディング プラクティスに従う security agent、一貫したテスト パターンを維持する testing agent などです。

custom agent はプロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` 内の Markdown file で定義します。各 file には少なくとも `name` と `description` を含む YAML frontmatter があり、その後に agent の動作、専門知識、instruction を定義する Markdown prompt が続きます。

### Custom agent と agent skill の違い

custom agent と [agent skill][agent-skills-concept]には一部共通点があります。どちらも主に Markdown file で定義し、AI に作業方法を伝えます。端的に言えば、**custom agent は作業者、skill はツール**です。

custom agent には独自のコンテキスト ウィンドウがあり、作業中に skill やほかの agent を調整できます。この演習のアクセシビリティ custom agent は、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業でプル リクエストのワークフロー skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。AI を使うほかの場面と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングのプラットフォームを利用できるようにしたいと考えています。最近、一部のユーザーから、文字と背景のコントラストが不十分で現在のダーク テーマが読みづらいという意見が寄せられました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。custom agent を使って機能を生成します。この演習では次のことを行います。

- 既存のアクセシビリティ custom agent を確認します。
- Copilot Chat でアクセシビリティ agent を使い、ハイ コントラスト モードを実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用 custom agent がすでに作成されています。Copilot をどのようにガイドするかを理解するため、その内容を確認します。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルタリング PR は別にしておきます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は custom agent に必須です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコードを生成する際の主要な責任、アクセシビリティのベスト プラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat で custom agent を使う

VS Code では、`.github/agents` で定義したすべての custom agent が Copilot Chat view 下部の agent dropdown に表示されます。custom agent を選ぶと、その agent の instruction と tool に合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat panel を開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な手順で説明します。

1. agent dropdown で **Agent** がまだ選択されていない場合は選択します。

   ![Chat view の agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat view 下部の agent dropdown を選択します。現在の agent が表示されます。既定値は **default** です。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ agent に送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat は codebase を調べて変更を提案し、プロジェクト ファイルを編集します。各編集はファイル パスとレビュー可能な差分とともにチャット内に表示されます。
6. agent が変更したファイルの一覧は **Files changed** indicator に反映されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は Astro component、CSS、関連テストなど、実際のリポジトリ ファイルを変更しています。

次の演習では、作業中の変更をレビューして進め方を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた GitHub Copilot の [custom agent][custom-agents]について学びました。custom agent を使うと、チームの知識と標準を再利用可能な agent にまとめられます。次の内容を確認しました。

- custom agent の定義方法。
- Copilot Chat の agent mode で custom agent を使う方法。

次は、[agent の作業を監視して進め方を調整します][next-lesson]。変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: Agent を監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
