---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で継続的なワークフローに GitHub Copilot CLI を活用しました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、用意されていたフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を定め、Autopilot モードで構築し、Interactive モードでレビューしました。
- custom instruction でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) custom agent を作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの証跡を確認しました。
- フィルタリング全体の変更をレビューし、その PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使えるスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップには、更新済みの `main` から作成した別々のブランチで、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加します。
2. **フィルタリングと品質改善のワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA profile を作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、新しい形式で結果を報告しました。QA agent は PR の前に、それらの結果と要件およびテスト範囲のレビューをまとめました。

追加するテストは、実際に不足している部分を補う必要があります。新しいテストが不要という QA の結果も適切な場合があります。マージを許可する前にコードと証跡を確認し、変更後は影響を受ける証跡を更新してください。

## ベスト プラクティス

Copilot に渡すコンテキストとツールが、作業の結果を左右します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instruction は標準を定め、skill は繰り返し実行するタスクを説明し、custom agent は専門的な役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更に取り組みながら確認するには **Interactive**、範囲が明確なタスクを進めるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同じくらいコンテキストも重要です。何を、なぜ、どのように作るのかを明確に伝えると、出力が大きく変わります。

## さらに試す

基本的なワークフローを学びました。次の CLI 機能も確認してみてください。

- `/review` で code review agent に変更を分析させます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどに活用してください。学んだことをチームで共有し、メンバーの知見からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
