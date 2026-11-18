---
title: "レッスン 9 - Canvas を確認して作成する"
description: "既存の Database Explorer Canvas を使い、その後リポジトリに保存するトリアージ Canvas を作成してレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

ここまでは、チャットを通じてエージェントに指示してきました。しかし、多くの作業は会話の中ではなく、ボード、ドキュメント、チェックリスト上で行われます。**Canvas** は、まさにそのような作業のために、アプリ内でユーザーとエージェントが共有できる領域です。このレッスンでは、まず Tailspin Toys に含まれる Canvas を使用し、その後、これまで取り組んできたバックログ用の Canvas を作成します。

このレッスンでは、次の操作を行います。

- Canvas とは何か、どのような場面で使うかを理解します。
- 既存の Database Explorer Canvas でプロジェクト データを調べます。
- バックログをトリアージする共有 Kanban ボード Canvas を作成します。
- 別の機能を実装せずに、新しい Canvas を確認して操作します。

## シナリオ

Tailspin Toys には、データベースを調べる Canvas がすでに含まれています。Canvas がプロジェクト データを対話型の領域に変換する仕組みを確認した後、次に取り組む作業を選ぶための再利用可能なボードを作成します。新しい機能の実装は始めません。

## Canvas とは

[Canvas][canvas-docs] は、計画、トリアージ ボード、リリース チェックリスト、ダッシュボード、ドキュメントなどの作業成果物を扱う、共有の対話型領域です。チャットは意図の説明や曖昧さの検討に適していますが、多くの作業は具体的な *surface* 上で行われます。Canvas を使うと、その surface でエージェントと直接共同作業できます。

Canvas は **双方向** です。エージェントが作業中に Canvas を更新できる一方で、ユーザーも同じ surface を編集できます。Canvas を作成すると、エージェントはプロンプトとワークフローに基づいて内容を構築します。その後も機能の追加、削除、修正を依頼できます。作成した Canvas はアプリの右側のパネルに開きます。

一般的な例は次のとおりです。

- 1日の計画を立て、Issue やプルリクエストに優先順位を付ける **Markdown Canvas**。
- ユーザーとエージェントがカードを追加し、作業を列間で移動する **agentic Kanban ボード**。
- リポジトリで優先度の高い Issue や繰り返し現れるテーマをまとめる **Issue triage ボード**。

## Canvas を使用する理由

タスクに構造、反復、検証が必要で、チャットだけでは不十分な場合は Canvas を使います。Canvas では次のことができます。

- ワークフローに合った実際の成果物に、エージェントの作業を結び付けます。
- 共有 surface で作業を直接調整または修正し、その変更を基にエージェントに作業を続けさせます。
- チャットの応答だけでなく、成果物への目に見える変更として進捗を確認します。

## Database Explorer Canvas を使用する

まず、プロジェクトに含まれる既存の Database Explorer Canvas を使用します。実際に動作する例を使うと、自分で作成する前に、リポジトリにスコープされた Canvas の動作を確認できます。

1. 絞り込み機能のプルリクエスト (PR) がマージされたことを確認し、ローカルの `main` を更新します。
2. GitHub Copilot app に戻り、**Home screen** を選択します。
3. `tailspin-toys` が選択中のリポジトリであることを確認します。
4. 更新済みの `main` を基に **new working tree** でセッションを作成し、**Interactive** モードを選択します。
5. 必要に応じてローカル データベースを準備し、ファイルを変更せずに既存の Canvas を開くよう Copilot に依頼します。

    ```plaintext
    Set up the local database if needed, then open the repository's Database Explorer canvas. Do not change any files.
    ```

6. Database Explorer で利用可能なテーブルを確認し、`games` を選択します。
7. 高評価のゲームを 5 件表示する読み取り専用クエリを実行します。

    ```sql
    SELECT title, star_rating
    FROM games
    ORDER BY star_rating DESC
    LIMIT 5;
    ```

8. 結果に、評価の降順で 5 件以内のゲームが含まれることを確認します。
9. **Files** を開いて `.github/extensions/database-explorer/extension.mjs` を確認します。Canvas がプロジェクトとともに保存され、クエリが読み取り専用の `SELECT` 文と `WITH` 文に制限されていることを確認します。
10. セッションにファイルの変更がないことを確認します。

## Issue をトリアージする Canvas を作成する

次に、別の種類の共有 surface を作成します。トリアージ Canvas をプロジェクト スコープで保存すると、チームがレビューして再利用できるリポジトリのアセットになります。

1. 同じセッションで `/create-canvas` を入力し、作成する Canvas を次のように説明します。

   ```plaintext
   Create a Kanban triage canvas for this repo's open issues and save it under .github/extensions/. Highlight the three issues you'd prioritize and explain why, with the rest below. Include summaries and links.

   Give each card an "Add to current context" action that adds the issue details without starting work or changing the issue. Make it keyboard-accessible and open it so I can try it.
   ```

Copilot は `.github/extensions` の下に Canvas 拡張機能を作成し、アプリの右側のパネルに共有 surface を開きます。生成された拡張機能は視覚的な成果物にとどまらず、実行可能なリポジトリ コンテンツです。次にファイルと動作を確認します。

## Canvas を確認して操作する

Canvas を共有する前に、リポジトリの実際の Issue と比較し、コントロールを操作します。内容が正確で、操作がアクセシブルであり、Issue のアクションが作業を開始せずにコンテキストを追加することを確認します。

1. **Changes** を開き、Canvas 定義がユーザーやセッション専用ではなく、リポジトリの `.github/extensions/` の下に保存されていることを確認します。既存の拡張機能やアプリケーション ファイルが変更されていないことも確認します。
2. ボードを実際のオープンな Issue と比較し、優先順位の説明を評価します。
3. カードとコントロールが読みやすく、キーボードで利用できることを確認します。
4. Issue の **Add to current context** を選択し、詳細だけが会話に追加されることを確認します。実装や Issue の状態変更が始まってはいけません。
5. 修正内容をレビューし、変更したファイルに適用できる既存の検証を実行するよう Copilot に依頼します。対話型の surface が開いたという理由だけで正しいと判断せず、結果と阻害要因を記録します。
6. Canvas に変更が必要な場合は、トリアージの範囲に絞った改善を依頼し、影響するチェックを繰り返します。この Canvas 作業の一部として、バックログの Issue を実装しないでください。

このワークショップでは、すでに手動マージと Agent Merge を両方試したため、新しい PR を作成せずに終了します。実際の開発では、他のメンバーが Canvas を利用する前に、チームの通常のプロセスでレビューしてマージしてください。

## まとめと次のステップ

ユーザーとエージェントが共同作業できる共有 surface を作成し、再利用しました。このレッスンでは、次の操作を行いました。

- Canvas とは何か、どのような場面で使うかを理解しました。
- 既存の Database Explorer Canvas でプロジェクト データを調べました。
- バックログをトリアージする共有 Kanban ボード Canvas を作成しました。
- 別の機能を実装せずに、新しい Canvas を確認して操作しました。

バックログを追跡できるようになったので、ここまでに構築した内容と次の進め方を[振り返ります][next-lesson]。

## 参考資料

- [GitHub Copilot app で Canvas 拡張機能を使う][canvas-docs]
- [Awesome Copilot の Canvases][awesome-copilot-canvases]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../10-review/
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[awesome-copilot-canvases]: https://awesome-copilot.github.com/extensions/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
