---
title: "レッスン 7 - QA agent を作成して使う"
description: "Issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングの動作を確認しました。今度はこれらの機能を、明確な QA プロセスを定義した custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool とどう連携するかを学びます。
- 再利用できる品質保証 profile を作成して確認します。
- QA agent を選択し、フィルタリング Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。custom agent を使えば、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## custom agent とは

custom agent は、Markdown profile で定義する専門化された Copilot です。profile には agent の目的、instruction、利用できる tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使った各カスタマイズには異なる役割があります。リポジトリの instruction はチームの標準を記述します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー用の tool を提供します。QA profile は、これらを使って要件を確認し、結果を報告する方法を Copilot に指示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA profile を作成する

機能の PR を作成する前に、再利用可能な QA profile を作成するよう Copilot に依頼します。profile には、QA が行うチェックと守るべき範囲を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. default agent に、次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

新しい agent を使う前に、Copilot が意図した QA ワークフローと制約を profile に反映したか確認します。これにより、検証だけを求めている場面で不完全な agent や範囲の広すぎる agent が機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、含めると agent の表示名が明確になります。
3. profile の instruction を読み、QA が要件を確認し、リポジトリの instruction に従い、`quality-checks` skill を実行し、Playwright MCP を使うことを確認します。
4. QA が証跡を示して報告し、実装コードを変更する前に確認を求め、変更を commit したりプル リクエストを開いたりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に default agent に修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトの agent を読み込みます。profile の作成後、同じフィルタリングの会話を再開して QA を選択すると、既存の Issue と計画の決定事項を活用できます。

1. 次のプロンプトを入力して agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な agent の一覧に表示されないことがあります。その場合も agent は利用できるので、上記のコマンドで有効にできます。

2. 次のプロンプトで、QA agent に機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent の作業を確認します。
4. 完了したら、作成されたレポートを読みます。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに追加し、その結果をレビューしました。このレッスンでは、次のことを行いました。

- custom agent が instruction、skill、MCP tool とどう連携するかを学びました。
- 品質保証 profile を作成して確認しました。
- QA agent を選択し、フィルタリング Issue に照らして結果をレビューしました。

これで実装、skill の更新、QA profile、テスト、検証レポートの準備が整いました。次は[機能の PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用の custom agent を作成する][create-agents]
- [custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
