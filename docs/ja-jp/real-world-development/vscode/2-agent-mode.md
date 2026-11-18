---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなど、さまざまな作業が必要です。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、更新し忘れたファイルを見つけて、さらに変更を加えます。作業はその後も続きます。

ここで役立つのが Copilot agent mode です。

Copilot agent mode は、IDE 内でより自律的に作業できるように設計されています。まず既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正する点で、開発者と同じように動作します。Agent mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる利用体験を提供します。Agent mode は IDE 内で動作し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の操作を学びます。

- Copilot agent mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot agent mode で UI とデータ レイヤーに新機能を実装する方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリーや発行元で絞り込めるようにします。前の演習では発行元のヘルパーを追加しました。残りのデータ レイヤー、UI、テストの作業を Copilot agent mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を確認できます。単一の Astro アプリで構成され、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、標準の DOM API を使う小さな範囲の Astro `<script>` でインタラクティブな機能を追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> ダイアログが表示され、`http://localhost:4321` を開くよう求められた場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバー アドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> codespace で Codespace ターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボード ゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリーなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの最初の実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されます。GitHub Copilot に、その内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は自律的に計画し、複数ファイルにまたがる変更を実装して、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの構築には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、よい学習体験にすることが目的です。ただし、LLM の動作は確定的ではないため、ワークショップに記載した例とは異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確定的ではないため、Copilot が異なる MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを送信し、バックログの Issue を Copilot に確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先していることを確認します。

## 命令ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを与える命令ファイルを確認します。Web サイトに機能を追加する方法を説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - 次のようなファイルの種類別の命令ファイルへのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 命令ファイルから、他の命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルを基準とした相対パスです。命令を再利用し、複雑な内容を管理しやすい単位に分け、例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 絞り込み機能を実装する

絞り込みを完了するには、少なくとも次の 3 つの更新が必要です。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整する。
- 絞り込みの動作を確認するテストを追加または更新する。
- ゲーム一覧ページを更新し、絞り込み UI を追加する。

また、コードベースにマージする前にテストを実行し、成功する必要があります。Copilot agent mode にこれらの作業を任せます。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例を示すスクリーンショット。](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 Issue を基に機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がまずプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている様子を示すスクリーンショット。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など複数のタスクを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。完了後はすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot の確認に応じて **Continue** を選び、テストを実行します。

   ![テストの実行に同意するか確認する、Copilot Chat ペインのダイアログ。](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了してエラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot agent mode の会話が完了した状態を示すスクリーンショット。](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot agent mode を使って Tailspin Toys の Web サイトに新機能を追加する方法を確認しました。次の操作を学びました。

- GitHub Copilot agent mode で UI とデータ レイヤーに新機能を実装する方法。
- Copilot agent mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを開きます。

### ボーナスの演習 — ページネーションを実装する

ゲーム一覧が増えてきたら、ページネーションが必要です。この演習で学んだことを使い、Copilot にサイトへのページネーションの追加を依頼します。次の点を検討してください。

- 命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページネーションの実装方法を決めます。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- GitHub Copilot に変更を依頼してコンテキストを与えるなど、反復が必要な場合があります。Copilot と作業する際には一般的な流れです。

## 参考資料

- [Copilot の ask、edit、agent mode: 機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
