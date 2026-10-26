---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。コードが動作することを手動で確認し、instruction file を使って標準に沿わせました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうしたタスクには **agent skill** が適しています。skill を使うと、Copilot にこれらの操作を適切に実行する方法を伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 実行結果のレポート形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、unit test、lint、型チェックに `quality-checks` skill を使っています。チームは、読みやすいレポートにするために出力形式を改善したいと考えています。

## Instruction、script、resource

agent skill には、再利用可能なタスクの instruction、実行可能な script、agent が必要に応じて読み込む補助 resource が含まれます。基本的には skill 名のフォルダーと、その中の `SKILL.md` file で構成されます。Markdown には skill の名前と説明を定義する frontmatter、機能の概要、呼び出す条件のガイダンスを記述します。フォルダーに script やほかの resource 用のサブフォルダーを含めることもできます。

> [!NOTE]
> skill に追加の folder や file は必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトの既存 command を使うため `SKILL.md` のみで構成されています。

skill はプロジェクトの `.github/skills` folder に置くとチームで共有・再利用できます。また、ユーザー用の `~/.copilot/skills` folder に置くこともできます。

## skill を確認する

Tailspin Toys チームが unit test、lint、型チェックの実行用に作成した `quality-checks` skill を見てみましょう。

1. Codespace に戻り、Codespaces editor で `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出す条件を理解するために使います。
3. instruction を読み、テストと lint の実行を Copilot に案内する方法を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接、または自然言語で skill を呼び出せます。Copilot に 3 つのチェックを実行させます。

1. Interactive mode のフィルタリング会話に戻ります。
2. 次の prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、tool が報告した内容がわかるようにレポートを改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. file の末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. file は自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、実行結果を確認しましょう。Copilot CLI は会話を再起動せずに編集済み skill を再読み込みできます。

1. 次の command を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じ prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェックに使う `quality-checks` skill を確認しました。
- 実行結果の形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、フィルタリングの機能 PR に含まれます。次は、[Playwright MCP server を使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## その他の skill の例

コミュニティの次の例は参考資料です。追加のタスクではありません。

- [Agent Skills specification][skill-spec]
- [Contribution workflow: `make-repo-contribution`][contribution-example]
- [Requirements documents: `prd`][prd-example]
- [Diagram と script の例: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
