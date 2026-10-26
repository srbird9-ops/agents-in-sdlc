---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、template から作成したリポジトリを接続して、workspace の基本を確認し、短い chat を試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、agent 主導の開発向けのデスクトップ アプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI pipeline をすぐに利用できます。複数の agent を並行して指示し、それぞれを分離された workspace で実行しながら、繰り返し作業を自動化する workflow に適しています。Node.js をインストールし、プロジェクトのコピーを用意できたので、次は app をインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub repository からプロジェクトを app に追加します。
- template によって用意されたバックログを含め、workspace の基本を確認します。
- app 自体について短い chat で質問します。

## シナリオ

チームでは、増え続けるバックログに AI agent を活用し始めています。Copilot app では、Issue の選択、agent の実行、変更のレビュー、プル リクエストのマージなどを一か所で管理できます。このレッスンで app をインストールして接続し、プロジェクトについて会話を始める方法を確認します。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用できます。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** policy を有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずインストールします。Windows、macOS、Linux 用のバージョンがあります。app をインストールして認証し、Tailspin Toys repository を追加します。

1. ブラウザーで [GitHub Copilot app の landing page][download-app]を開きます。
2. 使用する platform 用の app をダウンロードし、ページの手順に従ってインストールします。
3. インストール後に app を開きます。
4. **Sign in to GitHub** を選択し、案内に従って認証します。data residency を使用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選び、求められたら `*.ghe.com` hostname を入力します。GitHub Enterprise Server はサポートされていません。
5. onboarding で repository を選ぶ場合は、template から作成した Tailspin Toys repository を選択します。repository の選択を省略し、onboarding 後に追加することもできます。
6. theme を選び、onboarding を完了します。
7. sidebar の **Sessions** で repository を探します。すでに表示されている場合、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しい session を作り、**Add GitHub repository** を選択します。前提条件のレッスンで控えた owner と repository 名を使います。
9. repository が sidebar に表示されることを確認します。このレイアウトでは、`tailspin-toys` などの repository folder が **Sessions** の下にまとめられ、各 repository の下に session が表示されます。

## workspace の基本を確認する

プロジェクトを接続したら、sidebar にある app の主な領域を確認します。

- **New**: Copilot の新しい chat session を開始します。
- **Pull requests**: GitHub と統合されたプル リクエストを表示します。一覧の確認、filter、CI 状態の確認、レビューを app 内で行えます。
- **Issues**: GitHub Issue を表示します。一覧を確認して filter し、Issue から session を始められます。
- **Automations**: スケジュールまたはオンデマンドで実行する保存済み agent task です。todo の管理、定期メンテナンス、その他の繰り返し作業に役立ちます。まとめでは次のステップとして紹介しますが、別の演習ではありません。
- **Customize**: MCP server、plugin、skill、その他の component を使って Copilot app の機能を追加します。後で Playwright MCP を構成します。
- **Chats**: branch や専用 workspace が不要な質問やブレインストーミングに使う簡単な会話です。初回の chat を開始すると表示されます。見えない場合は **Sessions** の横の **+** を選択し、最初の chat を始めます。
- **Sessions**: repository folder と agent session をまとめて表示します。`tailspin-toys` などの repository 名を選ぶと repository view が開き、その下の session を選ぶと会話に戻れます。session ごとに分離された workspace が使われるため、変更を衝突させずに複数の作業を並行できます。星評価を追加するときに最初の session を作成します。

ワークショップを進めながら workspace の各機能を確認します。

> [!TIP]
> 操作方法や実現できることがわからないときは、Copilot に質問してください。手順を案内してくれます。

### 用意されたバックログを確認する

多くのプロジェクトにはバックログがあります。Tailspin Toys も例外ではありません。template を作成したときに用意されたバックログを確認しましょう。

1. sidebar の **Sessions** で個別の session ではなく Tailspin Toys repository folder を選び、repository の上部ナビゲーションで **Issues** を選択します。
2. Issue 番号ではなく、次のタイトルで探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して内容を読みます。各 Issue から agent session を開始できます。簡単な変更を終えた後、フィルタリングの Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加した repository の項目だけが表示されます。別の repository の作業項目を表示するには、その repository を app に追加してください。

## 簡単な chat を試す

app に慣れるには、*app 自体*について質問するのが簡単です。**quick chat** を使うと、branch や worktree を作らずに質問やブレインストーミングを行えます。session が不要な短い質問に最適です。

1. sidebar の **Chats** の横にある **+** を選択して新しい chat を開きます。**Chats** が表示されていない場合は、**Sessions** の横にある **+** を選択して最初の chat を始めます。その後 **Chats** が表示されます。
2. app の session の仕組みについて、次の質問を送ります。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. conversation view に表示された回答を読みます。各 session が専用の Git worktree で実行されるため、変更を衝突させずに複数の agent を並行して実行できます。会話は続けても、新しい chat を始めてもかまいません。

## まとめと次のステップ

おめでとうございます。GitHub Copilot app をインストールしてプロジェクトを接続し、workspace の基本を確認しました。次のことを学びました。

- app をインストールして GitHub にサインインする方法。
- GitHub repository からプロジェクトを追加する方法。
- workspace の基本を確認し、**Issues** で用意されたバックログを見つける方法。
- quick chat で手軽な質問をする方法。

次は[最初の agent session を開始し][next-lesson]、agent を使ってゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app で agent session を使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
