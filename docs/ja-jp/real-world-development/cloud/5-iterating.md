---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チームが改善できる関連ゲーム機能の作成、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業する必要があります。チーム全体のコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能を実装するために GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全を確保する目的で、いくつかのセキュリティ制約が適用されています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコードの作成に使うブランチ **のみ** に限られます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーなどの[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加するプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは **想定どおりの動作です**。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、より詳しいタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![変更されたファイルの一覧を表示するタブ。](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメント コメントやほかのドキュメントが追加されています。変更内容は状況によって異なります。

   TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム命令ファイルによるものです。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン。](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエスト上での Copilot との作業は、一方通行ではありません。チーム メンバーに依頼するように、プルリクエストまたはコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、指示文をひとつに決めることはできません。Copilot にドキュメントを追加・修正してもらう例を紹介します。

- 各コード ファイルの先頭に、内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、コードベースで追加したいドキュメントの変更を **@copilot** にメンションしてコメントで依頼します。
2. **View Session** を選択し、Copilot が作業を行う様子を確認します。Copilot が新しいセッションを開始して変更を加えます。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻るボタン。](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![ドラフト PR をレビュー可能な状態に変更するボタン。](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルのプルリクエストを選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターが表示されます。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン。](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、この作業は命令を更新する前に割り当てたため、コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます。前と同じように **Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを始めます。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルのプルリクエストを選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** が表示されます。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン。](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

   その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保存されることを確認します。

アクセシビリティのカスタム エージェントが、Copilot にアクセシビリティのベスト プラクティスに沿った機能を実装するよう導いたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せて、その間に別の作業に取り組めます。この習慣を身につけるために、リポジトリに Issue を追加し、Copilot に割り当ててみてください。次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンでは、次の操作を行いました。

- cloud agent の出力に後から反映されるよう、このリポジトリに含まれるカスタム命令ファイルを確認しました。
- Copilot cloud agent に Issue を割り当て、環境をセットアップして、非同期に計画・実行する様子を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使い、進行中のアクセシビリティ セッションを監視して指示しました。
- Copilot のプルリクエストをレビューして改善し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。引き続き学びたい場合は、ほかのハーネスで今回の内容をさらに実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent につなぐ任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のことを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## 参考資料

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

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
