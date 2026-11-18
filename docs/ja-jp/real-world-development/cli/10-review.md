---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連の作業を進めながら、GitHub Copilot CLI を使いました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されている絞り込み機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge に絞り込み機能の PR を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップでは、更新された `main` からそれぞれブランチを作り、2 つの PR を作成しました。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価の場合の表示を追加します。
2. **絞り込み機能と品質ワークフロー:** 絞り込み機能を実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

絞り込み機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップの手順を簡潔にするため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルでは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト範囲のレビューを加え、PR を作成する前の確認をまとめました。

追加するテストは、実際に不足しているケースを補うものである必要があります。QA の実行で追加テストが不要と判断されることもあります。マージを許可する前にコードと検証結果をレビューし、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。命令は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。**Plan** は実装前に方針を考える場合に、**Interactive** は特定の変更を一緒に進める場合に、**Autopilot** は範囲の明確なタスクを任せる場合に使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に伝えることで、結果は大きく変わります。

## さらに確認する

基本的なワークフローを終えました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` で、コード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について相談し、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを分離します。
- `/delegate` で、タスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールでも、上達するには使い続けることが大切です。本番コード、趣味のコード、以前から作ろうと思っていた小さなアプリなどで活用してみてください。学んだことをチームと共有し、チームの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の作業環境を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## 参考資料

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
