---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザーエクスペリエンスの向上と機能追加を目的に、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視できることも重要です。

cloud agent に割り当てたタスクを管理するためのツールは複数あります。その 1 つが、GitHub.com の[エージェントページ][agents-page]です。このミッションコントロールでは、オープンなプルリクエスト（PR）があるエージェントのタスクをすべて確認できます。実行された操作を調べたり、進行中のセッションを誘導したりすることもできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションを誘導し、機能の追加を依頼します。

## シナリオ

チームは、ハイコントラストモードの作成をエージェントに依頼した後、ライトモードも追加するよい機会だと気づきました。サイトのスタイルを更新し、切り替え機能を追加する作業がすでに進んでいるため、ライトモードも実装するのが自然だと考えました。そこで、ハイコントラストに加えてライトモードも追加するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインにあるタスクの一覧を確認します。Copilot に割り当てたタスクが表示されます。たとえば、次のタスクがあります。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれます。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実行方法によって異なります。

4. 右側に表示されるプルリクエスト（PR）ペインも確認してください。ここから PR と変更されたファイルを表示して、さらに監視できます。

## Cloud agent を誘導する

実行中のタスクを確認したら、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼しましょう。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールに表示されたアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot のタスクと「Steer active session while Copilot is working」ダイアログが強調表示されたエージェントページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、その内容を作業計画に含めたことを確認します。

## Copilot に作業を任せる

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順を完了すると、新しい依頼が作業の流れに組み込まれます。

今回も完了まで数分かかるため、ここまでに学んだことを振り返る時間にしましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して誘導する中心的な場所である、Copilot エージェントページを確認しました。このミッションコントロールから、進行中と完了済みのタスクを追跡し、実行中の作業を確認できます。また、進行中のタスクに指示を追加して、範囲を調整することもできます。

次の概念を確認しました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 進行中のセッションに追加の機能を依頼する。

Copilot がアクセシビリティ機能の作業を進めている間に、Copilot が作成した[プルリクエストを改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
