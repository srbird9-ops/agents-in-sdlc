---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、あらかじめ用意されたフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースの探索、コードの生成、コマンドの実行、外部 tool とのやり取りをすべてコマンド ラインから行えます。タスクを任せたり変更を依頼したりしながら、作業に集中できます。最初に tool をインストールしましょう。使い慣れた tool でインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub account で認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP server を使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログへの対応に AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を提供します。このレッスンでインストールと認証を済ませ、ワークショップの残りで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js がプリインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub account での認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められたら、device code の手順に従って認証し、Copilot CLI の利用を許可します。
3. Copilot CLI に次の prompt が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub session を通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本を確認する

通常の shell prompt で入力した command は Codespace で直接実行されます。Copilot CLI を起動した後は、自然言語で agent に依頼し、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して利用中のバージョンで使える command を確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問で正常に動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が使えることを確認します。
6. 次の prompt でフィルタリングの Issue を見つけるよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。簡単な変更を終えた後に、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI session は、ターミナルで現在 checkout されているブランチを使います。自動的に worktree を作成することはありません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は設定済みの許可範囲外で tool を使う前に確認を求めます。このワークショップでは Codespace 内の承認 prompt を省略し、演習に集中できるよう `--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は全自動の許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離環境でのみ使用し、日常の開発で既定の alias にしないでください。詳しくは [tool の許可と拒否][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、後のレッスンで Issue とプル リクエストを扱うために使う GitHub MCP の読み書き tool を有効にします。Codespace によりローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub resource は実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリ ルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 簡単な質問をもう一度して会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動保存します。後のレッスンで instruction を変更したり agent を追加したりした後、`copilot --resume` を使って同じ会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub account で認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP server を使ってフィルタリングの Issue を見つけました。

次は、[最初の小さな変更][next-lesson]を行い、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
