---
title: "レッスン 4 - custom instruction で Copilot をガイドする"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[instruction file][instruction-files]には、作成するコードだけでなく、その構成方法も記述できます。フィルタリング機能を作成したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file が agent に渡る仕組みを確認します。
- コーディング標準を守らせるために instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、優れた開発チームと同様に、開発プラクティスのガイドラインと要件があります。たとえば次のとおりです。

- コメントにはコードを言い換えるのではなく、目的や自明でない判断の理由を記述します。
- `db/` と `src/lib/` の公開関数には、`db` 引数を注入できる場合はその引数も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro component には `Props` の契約を記述し、関連するコードを変更したときはコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使うと、上記のプラクティスに沿ってタスクを実行するために必要な情報を Copilot に渡せます。

## instruction file

custom instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルと要件を理解させることができます。希望するコーディング規約、ライブラリ、含めたいコメントの種類を指定でき、提案やコード スニペットを調整できます。リポジトリ全体に適用する instruction も、特定のファイル種別に適用する instruction も作成できます。

instruction file には次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式とサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instruction のサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

サンプル プロジェクトには、変更の影響を確認するための instruction file が含まれています。まず、既存の内容を確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle data layer、テストなどの instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、instruction を適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。大きな instruction セットを小さく再利用可能なファイルに分け、Copilot がコード生成時に参照できる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存ファイルはよい出発点ですが、改善できる点があります。新しい TypeScript に TSDoc comment を追加するため、中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、27 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。機能のコンテキストを失わずに新しいガイダンスを反映するため、instruction を保存してからフィルタリングの会話を終了し、再開します。

1. フィルタリングの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルタリングの会話を選び、`game-filters-cli` ブランチが checkout されていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc comment が追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がプロジェクトの instruction file からコンテキストを取得する方法を確認し、instruction を適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file が agent に渡る仕組みを確認しました。
- instruction file を更新してコーディング標準を守らせました。
- instruction file がコードに与える影響を確認しました。

次は、lint とテストを一貫して実行するため、再利用可能な `quality-checks` skill を[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI 用の custom instruction を追加する][instruction-files]
- [custom instruction のサポート情報][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
