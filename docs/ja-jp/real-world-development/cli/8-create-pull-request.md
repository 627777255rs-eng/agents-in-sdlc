---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、命令の更新、スキルの更新、quality assurance (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、絞り込み機能と品質管理の作業を 1 つの会話とブランチで進め、この PR にまとめます。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認します。
- 機能の変更全体と検証結果を調べます。
- 絞り込み機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業では、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージを許可する権限は開発者が保持したまま、PR に必要な残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストのマージに必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビュー コメントなど、PR を妨げている問題を解決し、必要に応じてリベースします。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができますが、有効にする前に変更をレビューし、マージできる状態であることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したら、変更をまとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. オプションの一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認してください。リポジトリで許可されている場合は、手動でレビュー済みのマージを完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの作成など、開発プロセスの一部を自動化しました。次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認しました。
- 機能の変更全体と検証結果を調べました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## 参考資料

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
