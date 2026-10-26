---
title: "レッスン 2 - 星評価を追加する: 手軽な改善"
description: "GitHub Copilot app で最初の agent session を開始し、ゲーム カードに小さな変更を加えて、最初のプル リクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

workspace を確認し、quick chat を試したので、**agent session** を開始してプロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームのデータにはすでに星評価がありますが、ホーム ページのゲーム カードには表示されていません。agent に評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- agent session を開始し、その構成を学びます。
- agent に目的を絞った小さな変更を依頼します。
- workspace の差分表示で変更をレビューします。
- app をローカルで実行し、ブラウザーで変更を確認します。
- 最初のプル リクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードに表示されるのはタイトル、カテゴリ、パブリッシャー、説明だけです。ウォームアップとして、既存の評価を各カードに表示するよう agent に依頼します。小さく独立した変更なので、最初の session に最適です。

## Session の構成

**session** は、専用の分離された workspace で agent と会話する場所です。各 session には専用の **Git worktree と branch** が用意されます。複数の session を同時に実行しても、それぞれの変更は衝突しません。session は sidebar で repository ごとにまとめられ、選択すると切り替えられます。

session 内には、agent との **conversation**、ファイルを調査して編集する agent の **tool activity**、差分とともに表示される**変更ファイル一覧**があります。

## Session を開始して変更を依頼する

新しい session を開始してプロジェクトを調べ、機能を実装します。[app のセットアップ][prior-lesson]で GitHub repository からプロジェクトを追加しました。その repository 用に session を作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、app を開きます。
2. sidebar 上部の **+** を選択して session を開始します。
3. repository に `tailspin-toys` を選択します。
4. prompt box の下で **new working tree** と **Interactive** mode を選択し、次の prompt で変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して prompt を Copilot に送信します。

Copilot app は最初に分離されたプロジェクトのコピーである新しい worktree を作成します。続いて必要なファイルを探し、機能を追加するコードを作成します。Copilot app で新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot app で変更を確認しましょう。

1. app の右上にある **Toggle review panel** を選択します。開いた panel で **Changes** を選ぶと、変更ファイルと差分が表示されます。

    ![Create PR の右側にある Toggle review panel button を矢印で示す GitHub Copilot app の上部 toolbar](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使う中心的なファイル `GameCard.astro` に、次のようなコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は `No rating yet` と表示します。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI は確率的に動作するため、コードは上記と異なる場合があります。概ね同様の動作になっていることを確認してください。

## 変更を確認する

ブラウザーで確認する前に、agent が実行した自動チェックの結果をレビューします。数値の `starRating` と `null` の場合の表示をテストが確認していることを確かめます。前提条件の不足や省略されたチェックは成功ではありません。インストールを求められた場合は、承認する前に確認してください。

コードを読むだけで動作すると判断せず、Copilot にサイトを起動させて、browser canvas で更新後の UI を確認します。

> [!TIP]
> canvas は Copilot app 内で使える対話型 widget です。後で custom canvas を試し、自分でも作成します。ここでは組み込みの browser canvas を使います。

1. 次の prompt で、app を起動して browser canvas にページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくすると app が起動し、Copilot app 内にブラウザーが表示されます。
3. 評価のあるゲーム カードに 5 段階の評価が表示されることを確認します。
4. 次の prompt で session 用に起動した dev server を停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプル リクエストを作成してマージする

機能を作成できました。コードベースに変更を取り込むプル リクエスト (PR) を作成します。

1. 右上の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、案内に従って認証します。
3. PR 作成前に関連 Issue が必要だと Copilot から説明された場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. chat のすぐ上にある **PR** bubble を選択し、review pane で PR を開いて必要に応じてレビューします。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しい dialog window で **Merge pull request** を選択して PR をマージします。

## まとめと次のステップ

おめでとうございます。GitHub Copilot app を使って最初の変更をリリースしました。次のことを行いました。

- agent session を開始し、その構成を学びました。
- agent にゲーム カードの小さな変更を依頼しました。
- workspace の差分表示で変更をレビューしました。
- app をローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を開き、チェックを確認して明示的にマージしました。

次は、フィルタリング Issue から始め、Plan mode と Autopilot mode を使って、より大きな機能を作成します。[Issue から始める][next-lesson]に進んでください。

## リソース

- [GitHub Copilot app で agent session を使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプル リクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
