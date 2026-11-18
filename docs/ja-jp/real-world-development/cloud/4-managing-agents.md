---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業するよう設計されていますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば、GitHub.com の[エージェント ページ][agents-page]では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

チームは、エージェントにハイ コントラスト モードの作成を依頼した後、ライト モードも追加できる良い機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、ハイ コントラストに加えてライト モードも実装するよう、エージェントに追加で指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスクの一覧を確認します。Copilot に割り当てたタスクには、次のようなものがあります。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を作成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。次のような手順が含まれることがあります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するために、さまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent に追加の指示を送る

現在実行中のタスクを確認したので、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードも追加するよう依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。表示されるタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション。](../../../_images/ex6-accessibility-session.png)

2. セッションがセットアップを完了し、作業を始めるまで数分待ちます。次のようなメッセージが表示されたら作業を開始しています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中にアクティブなセッションへ指示を送るダイアログが強調表示された、agents ページの cloud agent タスク。](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加する様子を確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を完了してから、新しい依頼をワークフローに加えます。

この作業にも数分かかります。待っている間に、これまでに学んだことを振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを確認しました。ここでは、進行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

次の操作を行いました。

- ミッション コントロールとエージェント ページで、cloud agent のタスクを監視しました。
- 進行中のセッションに、追加機能を依頼しました。

Copilot がアクセシビリティ機能を完成させる間に、Copilot が作成したプルリクエストを[反復的に改善][next-lesson]します。

## 参考資料

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
