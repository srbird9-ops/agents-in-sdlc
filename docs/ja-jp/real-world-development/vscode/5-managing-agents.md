---
title: "演習 5 - Agent を監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、codebase の調査、変更の提案、ファイル編集、コマンド実行を自律的に行います。VS Code の Copilot Chat では、ツール呼び出しとファイル編集をリアルタイムに確認できます。差分を個別にレビューし、変更を承認または取り消し、エディターを離れずに追加のプロンプトで会話を進められます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして承認します。
- セッション中に agent へ追加指示を出し、ハイ コントラストに加えてライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ custom agent がハイ コントラスト モードを実装しています。作業中にデザイン チームから追加の依頼がありました。既定のテーマが暗すぎると感じるユーザー向けに、ライト モードの切り替えも必要です。最初から新しい会話を始めるのではなく、既存セッションの作業を広げます。

## Copilot の進捗を監視する

Copilot Chat view には、agent が調べたファイル、実行したコマンド、提案した編集が表示されます。ファイル編集は差分と **Keep** / **Undo** 操作とともにチャット内に表示されます。

1. 演習 4 で使った Copilot Chat view に戻ります。
2. agent が作業している間、チャットをスクロールして各ツール呼び出しを確認します。ファイルの読み取りや編集、実行を求められたターミナル コマンドが表示されます。
3. agent がファイル編集を提案すると、差分がインラインで表示されます。
   - **Keep** を選択すると変更を承認します。
   - **Undo** を選択すると変更を元に戻し、agent に再試行させます。
4. チャット上部の **Files changed** indicator を開くと、セッション中に agent が変更したファイル全体を確認できます。

> [!NOTE]
> tool の実行 (たとえばターミナル コマンド) に許可が必要な場合、agent は一時停止して **Allow** を表示します。内容を確認し、許可できるコマンドのみ承認してください。

5. agent がハイ コントラスト モードの実装を終えるまで待ちます。変更をまとめた最終メッセージが表示されます。

## Agent にライト モードを追加させる

ハイ コントラスト モードができたので、同じ会話を続けてライト モードの切り替えを追加します。会話を継続するため、agent はこれまでに変更した component、スタイル、local storage の方法などのコンテキストを保持しています。

1. Copilot Chat の入力欄に次の追加プロンプトを送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent は既存の変更を確認し、ライト モード向けの追加を提案します。
3. 変更が表示されたら、ハイ コントラストのときと同様に **Keep** または **Undo** を選択してレビューします。
4. agent の方針が希望と異なる場合 (たとえば、切り替え UI を意図しない方法でリファクタリングする場合) は、入力欄の横にある **Stop** button で停止し、説明を追加して再度依頼します。

> [!TIP]
> 作業中のセッションに追加指示を出すと、最初からやり直すより効率的です。追加プロンプトで調整 (「CSS を簡潔にし、selector の重複ではなく CSS variable を使う」)、軌道修正 (「3 つ目の切り替えを追加せず、既存の設定 panel にライト モードを追加する」)、範囲の拡張 (「両方の切り替えに keyboard shortcut も追加する」) ができます。

## 変更全体をレビューする

commit する前に、agent が変更した内容をひととおり確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更されたファイルの一覧を確認します。Astro component、スタイル、関連テストなどが含まれます。
3. いくつかのファイルを開いて差分を確認します。custom agent のアクセシビリティ対応 (ARIA attribute、keyboard navigation、semantic HTML、local storage による設定保持) が反映されていることを確認します。
4. 問題がなければ、Source Control panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode を使い、作業中のセッションを監視して進め方を調整しました。各ファイルの編集をレビューし、変更を承認または取り消し、コンテキストを失わずに追加プロンプトで作業を広げました。

このレッスンでは、次のことを学びました。

- agent の作業をリアルタイムで監視する方法。
- 個別のファイル編集を承認または取り消す方法。
- 作業中のセッションに追加プロンプトを送り、進め方を調整する方法。

次のレッスンでは、[アクセシビリティの作業をプル リクエストとして公開します][next-lesson]。このラボでローカル custom agent が作成した変更も確認します。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
