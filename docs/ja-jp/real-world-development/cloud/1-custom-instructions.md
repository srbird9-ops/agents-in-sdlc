---
title: "演習 1 - カスタム命令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に背景情報を伝えておきたい場合は、その情報を参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」進めるかも伝えられます。

この演習では、次のことを行います。

- プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が、リポジトリのカスタム命令やパスを指定した命令ファイルを通じて Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「変更前／変更後」を実行しません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、リアルタイムで並べて比較するのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストを確認するときに、命令ファイルの影響を確認できます。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには必ずユニットテストが必要です。
- UI はダークモードにし、現代的な見た目にする必要があります。
- コードには TSDoc のドキュメントコメントを追加する必要があります。
- 各ファイルの先頭には、そのファイルの役割を説明するコメントブロックを追加する必要があります。

命令ファイルを使い、Copilot がこれらの慣行に沿ってタスクを進めるための情報を提供します。

### カスタム命令

カスタム命令を使うと、Copilot にコンテキストや好みを伝え、コーディングスタイルや要件をより正確に理解させることができます。コーディング規約、ライブラリ、コメントの形式などを指定できる便利な機能です。リポジトリ全体に適用する命令も、特定のファイル種別やタスクに適用する命令も作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリ内のすべてのリクエストで Copilot に送信される単一の命令ファイルです。Copilot へのチャットや CLI リクエストの多くに関係する、プロジェクト全体の情報を記述します。技術スタック、開発対象の概要、ベストプラクティス、全体に適用するガイダンスなどを含めます。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に使う命令ファイルです。TypeScript などの言語や、UI コンポーネントやユニットテストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使用されます。コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの命令ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準形式の命令ファイル][custom-instructions-support]にも対応しています。

### 命令ファイルを管理するためのベストプラクティス

命令ファイルの作成方法をすべて扱うには、この演習の範囲を超えてしまいます。サンプルプロジェクトには、代表的な方法が示されています。基本的には、次の点に留意してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関するガイダンスを記述します。
- 自然な言葉で明確に記述し、コードの例と避けるべき例を示します。

命令ファイルの作り方は 1 つではありません。AI の使い方と同じように、試行錯誤を通じてプロジェクトやシナリオに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意するとよいでしょう。このプロジェクトのファイルを確認すると、[UI の変更][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の命令ファイルが見つかります。
>
> Copilot を使って命令ファイルを作成することもできます。各環境での操作は異なります（たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init`）。必要な場合は、その環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、命令ファイル、カスタムエージェント、その他のリソースが集められた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれている命令ファイルを確認してください。共通の `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する命令ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令が適用されるファイルを指定する glob です。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成するときの命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな命令セットを再利用可能な小さなファイルに分け、コード生成の参考になる例を Copilot に示せます。ここに記載されたパスは、リポジトリのルートではなく命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されています。ただし、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーをコードに追加するという、プロジェクト全体のルール 1 つです。このハーネスの後半で issue を割り当てると、cloud agent はこのルールを参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチにある命令を参照しますが、ここでは変更を整理してコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、コーディング標準を記録しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントに関するルールはまだありません。そのため、生成されたヘルパー関数にドキュメントコメントがありませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、リポジトリレベルの `.github/copilot-instructions.md` を使って全体に適用するガイダンスを記述できます。また、`*.instructions.md` ファイルを使って特定の言語、ファイル種別、タスクに関するガイダンスも記述できます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記載するのが適切です。

## 命令の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチにある命令ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が命令を参照できるように、変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てる issue に取り組むとき、cloud agent はこの命令を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業を再現するために含まれています。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを得る仕組みを確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストを確認するときに、この標準が反映されているかを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
