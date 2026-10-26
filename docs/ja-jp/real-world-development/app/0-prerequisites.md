---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに備え、Tailspin Toys プロジェクト用に Node.js をインストールし、template からリポジトリのコピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の中心となるデスクトップ アプリです。Issue とプル リクエストにすばやくアクセスし、GitHub Copilot を使って開発できます。このワークショップでは、GitHub Copilot app を使って、Astro で作られた Tailspin Toys app をローカルで更新します。始める前に、ローカルに Node.js がインストールされていることを確認してから Copilot app をインストールします。

このレッスンでは、次のことを行います。

- プロジェクトのテストを実行するために Node.js をインストールします。
- template から Tailspin Toys プロジェクトのコピーを作成します。

## Node.js をインストールする

複数のレッスンで、agent に機能の構築と Tailspin Toys の test suite 実行を依頼します。これには **[Node.js][nodejs]** が必要です。プロジェクトで必要な runtime は Node.js のみです。現在の **LTS** リリースをインストールします。

すべてのプラットフォームで最も簡単な方法は、公式 installer を使うことです。

1. Windows Terminal、macOS Terminal、または普段使うターミナルを開きます。
2. 次の command で Node.js のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. 両方の command が成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たす場合は、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合や、更新が必要な場合にのみ、以下の手順を実行します。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用している OS 用の **LTS** build をダウンロードします。
6. installer を実行し、既定値を受け入れます。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナル ウィンドウを開きます。
8. 新しいターミナルで次の command を実行して、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app の agent も `node --version` と `npm --version` を実行できる必要があります。通常のターミナルで成功しても、app から利用できるとは限りません。Node.js をインストールしたり PATH を変更したりした後は、app を完全に終了して再起動します。session で引き続きどちらかの command が失敗する場合は、続行する前に[session 環境のトラブルシューティング][session-environment]に従ってください。

> [!IMPORTANT]
> 各 worktree にも、プロジェクトの依存関係と E2E test 用の Playwright Chromium が必要です。worktree を準備する際は Tailspin Toys リポジトリの README に従い、インストールの要求を承認する前に内容を確認してください。

## ラボ用リポジトリをセットアップする

自分用の Tailspin Toys プロジェクトのコピーを使います。[template repository][template-repository] から新しいリポジトリを作成してください。ラボに必要なファイルがすべて含まれています。app のインストール時に接続します。

1. 新しいブラウザー ウィンドウで、ラボ用の GitHub repository `https://github.com/github-samples/tailspin-toys` を開きます。
2. **Use this template** を選択し、続けて **Create a new repository** を選択してコピーを作成します。

    ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft 主催のイベントで受講している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる organization にリポジトリを作成できます。

    ![template が github-samples/tailspin-toys に設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。後でラボ内で参照します。

> [!NOTE]
> template からリポジトリを作成すると、GitHub Issue のバックログも自動で作成されます。ワークショップではこれらの Issue に取り組むため、自分で登録する必要はありません。

新しいワークショップ template を使ってください。template にはリポジトリ instruction、アプリケーション コード、テスト、`quality-checks` skill、既存の canvas 拡張機能が含まれています。ワークショップ中に skill をカスタマイズし、QA agent を作成します。古いコピーを使う場合は、必要なファイルが含まれているか講師に確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- コンピューター上でプロジェクトを build してテストするために Node.js をインストールしました。
- template から Tailspin Toys リポジトリのコピーを作成しました。

次は [GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して workspace の基本を確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [template からリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-から-session-を開始する
