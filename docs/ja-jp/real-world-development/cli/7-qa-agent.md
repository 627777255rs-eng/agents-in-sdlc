---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーから絞り込み機能を確認しました。次は、これらの機能を、明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な quality assurance プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込み機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門家向けバージョンです。プロファイルには、エージェントの目的、命令、利用できるツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令ではチームの規約を説明します。`quality-checks` スキルには繰り返し実行できるチェックをまとめます。Playwright MCP ではブラウザー ツールを利用できます。QA プロファイルでは、これらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。QA が実行するチェックと、その作業で守る範囲をプロファイルに定義します。

1. Codespace に戻り、絞り込み機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のようにカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと作業範囲を正しく設定したか確認します。確認を依頼しただけなのに、プロファイルが不完全だったり範囲を超えたりして機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、追加するとエージェントの表示名が明確になります。
3. プロファイルの命令を読み、QA が要件を起点とし、リポジトリの命令に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制約が含まれていない場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue を QA にレビューさせる

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じ絞り込み機能の会話を再開し、QA を選択すると、すでにコンテキストに含まれている Issue と計画の内容を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 作業が終わったら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに加え、結果をレビューしました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、絞り込み機能の Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらを[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## 参考資料

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
