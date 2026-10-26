---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの向上と機能追加を目的に、GitHub Copilot と複数のタスクに取り組んできました。コードへのドキュメント追加、デザインチームが改善を重ねるための関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

これまで何度も説明してきたように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ず確認し、通常の DevOps プロセスに沿って作業します。そこで、チームのほかのメンバーにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で自律的にタスクを実行するため、安全を保つためのセキュリティ上の制約が設けられています。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセス権のみを持ち、書き込みアクセス権はコード用のブランチに限られます。
- Cloud agent は GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions のワークフローは、人間が承認するまで実行できません。
- MCP サーバーを含め、[外部リソースへのアクセスはデフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加に関するプルリクエスト（PR）を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスがデフォルトで制限されているため、この警告は**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** のようなタイトル、またはそれより詳しいタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだことを振り返ったりしてもよいでしょう。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメントコメントなどが追加されています。変更内容は環境によって異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム命令ファイルによるものです。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するとき、一方的に依頼するだけではありません。チームのメンバーに依頼するのと同じように、プルリクエストのコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はそのコメントを確認し、対応するための別のセッションを開始します。結果は確定的ではないため、具体的な依頼文は示せません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。上記の例や、コードベースのドキュメントに関する別の提案を使ってください。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更を確認します。

満足できるまで、反復して改善を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトル、またはそれより詳しいタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 一部のワークフローが承認待ちであることを示すインジケーターが表示されます。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（Copilot が作成したブランチ名を `<branch-name>` に指定します。たとえば **copilot/fix-8** です）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot cloud agent に更新を依頼して反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（この issue はカスタム命令を更新する**前**に割り当てたことを思い出してください）。これまでと同様、**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれています。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトル、またはそれより詳しいタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装を確認し、特に次の点に注目してください。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持するための local storage の使用
   - ハイコントラストモードとライトモード用の CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript／TypeScript コード

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティのカスタムエージェントが、アクセシビリティのベストプラクティスに従ってこれらの機能を実装するよう Copilot を導いたことを確認してください。改善点やアクセシビリティ上の懸念があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらにタスクを委任する

Cloud agent に実際のバックログ項目を任せ、その間にほかの作業を進めると効果的です。この習慣を身につけるため、リポジトリに issue をいくつか追加し、Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス用ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認し**、cloud agent の出力にどのように影響するかを確認しました。
- **issue を Copilot cloud agent に割り当て**、環境をセットアップし、計画を立てて、非同期にタスクを実行する様子を確認しました。
- **カスタムエージェントを作成して利用し**、アクセシビリティのためにハイコントラストとライトモードの切り替えを追加しました。
- **Copilot エージェントページをミッションコントロールとして使い**、アクセシビリティのセッションを監視し、途中で誘導しました。
- **cloud agent のプルリクエストをレビューして改善し**、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学習したい場合は、ほかのハーネスで今回学んだ内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ作成します。

自分のリポジトリでは、次の方法も試してみてください。

- リファクタリングやテストカバレッジに関する issue を cloud agent に割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` をセットアップする。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
