---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまなものがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発プロセスを効率化し、開発者がより大きな課題に集中できるようになります。

この演習では、Copilot cloud agent を使って次のことを学びます。

- コード生成に使う環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。サイトの最初のバージョンを開発した委託先は、ドキュメントを十分に作成していませんでした。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc のドキュメント コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、受け入れテストで UX を試せる程度で十分です。現時点では優先度の高い別の Issue があるため、この作業はブロックされています。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業に集中している間に非同期で処理できます。完了後に戻って、期待どおりの結果になっているか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。進行中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして、段階的に作業し、学び、試し、結果に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot の動作を導く方法を学びました。すでにカスタム命令の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、この機会に簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認しておくと、後で影響を確かめられます。

リポジトリの GitHub Web UI、または起動中の codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストで TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらの内容は命令ファイルによるものです。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、適切な状態を保つための環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を構成します。`.github/workflows/copilot-setup-steps.yml` ファイルで設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるようになります。ラボの流れを円滑にして、学習に集中できるよう、このワークフローはあらかじめ設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は操作用の独自トークンを受け取ります。ここでは、手順に必要な最小限の権限を設定します。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中にリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースにマイグレーションを適用し、ビルドとテスト用のデータをシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、主なポイントは次のとおりです。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。このトリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待つ代わりに、ワークフローが成功することをテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いままになっていたり、まったくなかったりします。こうした技術的負債を放置すると、生産性が低下し、コードベースの保守や新しいメンバーの受け入れが難しくなります。Copilot はドキュメントの作成が得意なため、Copilot cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で作業内容を確認します。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

   ![Issue に Copilot を割り当てる画面。](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当ての詳細。](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を構築し、前述のワークフローを実行してからプロジェクトを分析し、問題への最適な取り組み方を判断します。

13. todo リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示。](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状況を把握し、計画を考えて判断を下し、コードを作成する様子を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で進め、開発者が別のタスクに集中できるようにすることです。この機能を使い、別のタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能を追加するコードの作成に取りかかります。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チームから依頼された関連ゲームの追加に時間がかからないとしても、その時間を別の作業に使えます。このタスクも Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

   ![Issue に Copilot を割り当てる画面。](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue に取り組んでいることを示す目の絵文字。](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了してからレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent を使うと、非同期に処理する Issue を Copilot に割り当てられます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

次の操作を行いました。

- コード生成に使う環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を理解しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム命令も使えます。

## 参考資料

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
