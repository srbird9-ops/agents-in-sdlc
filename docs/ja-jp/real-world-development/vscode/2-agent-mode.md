---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイル変更やテスト実行が必要になることがあります。通常は必要なファイルを探して変更し、テストを実行してデバッグします。変更漏れがあれば、該当ファイルを特定して再度更新します。作業は続きます。

こうした場面で Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同様に、最初に既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動で修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。agent mode は IDE 内で作業するため、すばやくフィードバックを得て対話できます。cloud agent はチームの一員のように非同期で作業し、Issue とプル リクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて複数の変更を連携させる方法。
- GitHub Copilot Agent Mode が UI と data layer にまたがる新機能を実装する方法。
- codebase にマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリとパブリッシャーで絞り込めるようにします。前の演習ですでに publishers helper を追加しました。次は Copilot Agent Mode を使って、data layer、UI、テストの残りの作業を完了します。

## Tailspin Toys サイトを実行する

変更を加える前に、現在の機能を理解するため Tailspin Toys サイトを確認します。

このサイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro app が build 時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite database に直接クエリし、別の backend API や client-side UI framework はありません。再利用可能な data-access helper は `src/lib/` にあり、対話機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### サイトを起動する

単一の Astro dev server を起動する `npm run dev` task を利用できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後は `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server が起動すると、次のような URL を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro server のアドレス `http://localhost:4321` を選択してサイトを開きます。

> [!NOTE]
> codespace では、Codespace terminal から localhost URL を選ぶと `https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web server をホストする codespace へのプライベート tunnel です。

### サイトを確認する

サイトが起動したら、次の主な機能を確認します。

- **ホーム ページ**: タイトル、画像、説明があるボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat panel を開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な手順で説明します。

サイトの基本機能は動作しています。まず、追加機能を検討するためバックログを確認します。template からプロジェクトをセットアップしたときに Issue のバックログが自動作成されています。GitHub Copilot に一覧を表示させます。

1. Chat view の agent dropdown で **Agent** を選択します。Agent は複数ファイルの変更を自律的に計画・実装し、terminal command を実行して tool を呼び出します。

   ![Chat view の agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用できるモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使ったため、指示に記載しています。比較的似たコード提案を得て、演習をスムーズに進めるためです。ただし、LLM は確率的に動作するため、提案がこの資料と異なる場合があります。これは想定どおりです。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP command を使う場合がありますが、タスクは実行できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示する command を実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに役立つと考えた順に、項目へ優先順位を付けたことを確認します。

## instruction file をレビューする

コード生成を始める前に、Copilot にコンテキストを渡す instruction file を確認します。[user interface (UI) instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) には、Web サイトへ機能を追加する方法が記載されています。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、component の設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの instruction へのリンクがあります。

> [!TIP]
> instruction file から、ほかの instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。大きな instruction セットを小さく再利用可能な単位に分け、例や template を示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの変更が必要です。

- data layer (`src/lib/`) に残りのフィルタリング ロジックを追加または改善します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、codebase にマージする前にテストを実行して成功を確認します。Copilot Agent Mode にこれらの作業を任せましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** を選択して instruction file を追加します。

   ![UI instruction file を選ぶ例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat view の agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat view の agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べて機能に関係するファイルを見つけます。data-layer helper、UI、テストを特定して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調査しているスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクや codebase の複雑さによっては数分かかる場合があります。作業中に code editor に **Keep** と **Undo** が表示されることがあります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat pane でテスト実行の確認を求める dialog のスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はタスクが完了してエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat session のスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. すべて動作し、コードのレビューも完了したら、Copilot Chat window で **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode で Tailspin Toys に機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI と data layer にまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて複数の変更を連携させる方法。
- codebase にマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP server で機能をテストし][next-lesson]、プル リクエストを作成します。

### 追加演習: ページネーションを実装する

ゲームの一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装させてみましょう。既存の instruction file を使うなど、ベスト プラクティスに従います。ユーザーがページ サイズを選択できるようにするか、固定にするかを検討します。希望する実装に必要なガイダンスをプロンプトに含めてください。必要に応じて追加情報や修正を Copilot に伝えます。Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: 機能と使いどころ][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
