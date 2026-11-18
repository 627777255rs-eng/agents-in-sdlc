---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にも操作に使えるスラッシュ コマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローが完了しました。次は、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルが処理できるコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使うと、状況を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き容量にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウがいっぱいになると自動的に圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関係のない別タスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示して、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown ファイルまたは HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown へのエクスポートに使う `/share file` など、目的に合った共有先を選びます。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を並行して進める、または委任する

ワークショップの主要な内容は完了しました。大きなタスク向けに、Copilot CLI には次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プルリクエストを開くことがあります。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、新しい範囲の明確なタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する仕組みを確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュ コマンドがあり、さらに詳しく確認できます。これまでに学んだことを[振り返り][next-lesson]、次の学習に進みましょう。

## 参考資料

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
