---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に準備を整えます。Tailspin Toys リポジトリのコピーを作成して [codespace][codespaces] を起動します。次のレッスンでは、codespace の統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- template から Tailspin Toys プロジェクトのコピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ用リポジトリをセットアップする

自分用の Tailspin Toys プロジェクトのコピーを使います。[template repository][tailspin-template] からリポジトリを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] を開きます。
2. **Use this template** を選択し、続けて **Create a new repository** を選択してリポジトリのコピーを作成します。
3. GitHub または Microsoft 主催のイベントで受講している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization にリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。ワークショップの後半で参照します。

> [!NOTE]
> template からリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこれらの Issue に取り組むため、自分で登録する必要はありません。

新しいワークショップ template を使ってください。template にはリポジトリ instruction、アプリケーション コード、テスト、`quality-checks` skill、演習で使うバックログが含まれます。古いコピーを使う場合は、必要なファイルが揃っているか講師に確認してください。

## Codespace を作成する

次に、ワークショップで使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、tool に対応した高機能な editor を利用できます。

1. 作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。template がプロジェクトの依存関係、Playwright Chromium、ローカル database をインストールします。
5. **Do you trust the authors of the files in this folder?** と確認されたら、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートで terminal を開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択し、Tailspin Toys サイトが読み込まれることを確認します。
8. terminal に戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- template から Tailspin Toys プロジェクトのコピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は Codespace に [GitHub Copilot CLI][next-lesson] をインストールし、GitHub account で認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template からリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
