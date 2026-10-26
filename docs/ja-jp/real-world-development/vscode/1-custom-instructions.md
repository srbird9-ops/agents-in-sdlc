---
title: "演習 1 - Custom instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの実行方法や Copilot が知っておくべき背景情報がある場合は、Copilot が利用できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。重要な方法の 1 つが [instruction file][instruction-files] です。コード生成のガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に渡る仕組みを確認します。
- 現在の instruction を使って、フィルタリング用の最初の data slice (publishers helper) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- instruction の更新とフィルタリング用の data slice を `main` に commit し、次の演習で更新済みガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的です。この演習の目的は、instruction 更新後の動作の傾向を確認することであり、出力を文字単位で一致させることではありません。

## Instruction file

### シナリオ

Tailspin Toys には、優れた開発チームと同様に、開発プラクティスのガイドラインと要件があります。

- data layer には常に unit test が必要です。
- UI はダーク モードで、モダンな印象にします。
- コード内のドキュメントには TSDoc doc comment を使います。
- 各 file の先頭に、その file の役割を説明するコメント ブロックを追加します。

instruction file を使うと、これらのプラクティスに沿って作業するために必要な情報を Copilot に渡せます。

### Custom instructions

custom instruction では、コンテキストや設定を Copilot に提供し、コーディング スタイルや要件を理解させます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定し、提案やコード スニペットを調整できます。リポジトリ全体に適用する instruction と、特定の file 種別やタスクに適用する instruction を作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。技術スタック、プロジェクトの概要、ベスト プラクティス、その他の全体的なガイダンスなど、ほとんどの chat や CLI request に関係するプロジェクト情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクや file 種別向けに作成し、TypeScript や Astro などの言語、UI component の作成や unit test の追加などのタスクに関するガイドラインを提供します。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われます。コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` frontmatter がある `*.instructions.md` file の両方を使います。
>
> Copilot は [AGENTS.md や CLAUDE.md など、ほかの規格による instruction file][custom-instructions-support]もサポートします。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法を詳しく扱うのはこのラボの範囲外ですが、サンプル プロジェクトの例は代表的な方法を示しています。

- `copilot-instructions.md` は、作成中のもの、プロジェクトの構造、全体のコーディング標準などのプロジェクト レベルのガイダンスに集中させます。
- `*.instructions.md` は、unit test、Astro component、data layer などの file 種別や特定のタスク向けに使います。
- 自然言語で明確に説明します。コードの望ましい例と避ける例を示します。

instruction file や AI の使い方に唯一の正解はありません。試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意します。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの file があります。
>
> Copilot に instruction file を生成させることもできます。各 surface で方法は異なります。たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` などです。関連するレッスンで説明します。
>
> template や出発点を探す場合は、instruction file、custom agent、その他のリソースがある [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` file があります。エディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` folder を開き、Astro、Drizzle data layer、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、リポジトリ ルートからの相対 glob で、適用先の file を決めます。この例では `**/*.test.ts` などに一致する TypeScript test file が対象です。
5. このプロジェクトの unit test 作成用 instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで確認します。`unit-tests.instructions.md` などのほかの instruction file や、既存のプロジェクト file へのリンクがあります。大きな instruction セットを再利用可能な小さな単位に分け、コード生成時に Copilot が参照する例を示せます。記載されたパスは repo root ではなく instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment と file comment header の規則を追加します。

## 変更用のブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しい terminal を開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction 更新**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、該当する手順で説明します。

custom instruction の効果を確認するため、まず現在の instruction のままプロンプトを送信します。その後 instruction を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いた editor tab を閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. `src/lib/publishers.ts` を開き、helper の配置先を Copilot に伝えます。
3. Chat view の agent dropdown で **Agent** を選択し、Copilot が file を変更できるようにします。

   ![Chat view の agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを適用します。helper とテストなど、複数 file が変更される場合があります。
6. 提案された helper が型付き関数であり、最初の引数に `db` client を受け取り、型付き publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の data-layer 規約によるものです。
7. 生成されたコードに TSDoc doc comment と file-level comment header が**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction 更新後に一貫性が向上することが重要です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コーディング標準を記録してコード提案を改善します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コード内ドキュメントの規則がまだないため、生成された helper に doc comment がありません。
3. 既存の標準の直後に次の Markdown 行を追加し、file comment header と TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、`.github/copilot-instructions.md` のようなリポジトリ レベルの file に全体的なガイダンスを記述することも、特定の言語、file 種別、タスク向けの `*.instructions.md` に記述することもできます。追加した doc comment のようなプロジェクト全体の標準は、リポジトリ レベルの file に置くのが適切です。

## 同じ prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しい file に注目するようにします。
3. 先ほどと**同じ prompt**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次のような comment block が追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment が含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に適用させ、次の演習で利用する実際のコードに反映しました。

## instruction と branch を commit / push する

instruction file もほかの asset と同様に source control で管理します。変更を commit してブランチをリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しい terminal を開きます。
2. 次の command で instruction と helper の変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper の基盤、および Copilot が変更した関連 file を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチを push したら、バックログのドキュメント標準に関する Issue に関連付けてプル リクエストを作成します。GitHub tool が接続されているため、Copilot に Issue を見つけて PR を作成・マージさせます。

> [!NOTE]
> Copilot が GitHub で操作する最初の演習です。プロジェクト template では GitHub tool が構成済みのため、追加セットアップは不要です。初めて tool を使うときに VS Code から GitHub へのサインインを求められたら、案内に従って許可します。後の演習では、この接続に使う Model Context Protocol を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat view を開き、agent picker で **Agent** を選択します。
3. instruction file の更新に関する Issue を探し、instruction の更新と publishers helper の基盤を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作を許可するよう求めたら、command を確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次の prompt で PR をマージしてローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作と shell command の実行を Copilot に許可するよう求められたら、command を確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存 instruction のままコード生成 prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じ prompt を再実行し、提案コードに新しい標準が反映されることを確認しました。
- 次の演習に備えて instruction の更新と helper の基盤を `main` に commit しました。

次は、Copilot が codebase 全体に新しい機能を追加する[agent mode][next-lesson]で instruction を使います。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot の custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人向け custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
