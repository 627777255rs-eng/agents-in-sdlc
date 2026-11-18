---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode で絞り込み機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで絞り込み機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、絞り込み機能の作業に関するプルリクエストを開きます。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にアクセスできると、さらに便利になります。Copilot が外部のツールやサービスと通信するための標準的な方法が Model Context Protocol (MCP) です。

![コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) と agent mode の連携を示す図。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わり操作したりできます。

これらのツールやリソースには、MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部のツールやサービス (既存の API や NPM パッケージなどのローカル ツール) との通信を管理します。各 MCP サーバーによって、AI エージェントが利用できるツールやリソースが異なります。

広く使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントを使って、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動化機能を利用できます。AI エージェントを使って Web ページに移動し、フォームに入力して、ボタンを選択できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同じように扱ってください。利用する前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の項目はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。機能のテストに最適です。

1. `.vscode/mcp.json` の `github` と並べて `playwright` を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は **ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がなく、ブラウザーを表示できない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使うため、Playwright に必要なブラウザーはすでにインストールされていることが多いです。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は **オンデマンドで** MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 項目の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code でサーバーを信頼するか確認されたら、構成をレビューし、信頼して起動できるようにします。

手動で起動する必要はありません。スキップした場合は、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを開いて確認する代わりに、エージェントがページを開いて移動し、絞り込みを適用して、その結果を確認して要約できます。会話を続けながら機能が期待どおりに動作するか確認する、すばやい方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが判断します。支援技術と同じようにページを認識するため、簡単な機能確認をアクセシビリティの基本確認にも活用できます。

サーバーを接続し、アプリが起動している状態で、Copilot に絞り込み機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動して各手順を実行し、結果を報告します。レポートを Issue の受け入れ条件と照らし合わせます。問題がある場合は追加の質問をするか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードすることがあります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してからもう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更されたファイルを表示する Source Control パネルのスクリーンショット。](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) を使うなど、プルリクエストの作成方法はいくつかあります。GitHub Copilot を利用しているので、PR の作成も任せてみましょう。Copilot は関連する Issue を見つけ、それに関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように PR の作成を Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作に必要なタスクを Copilot に許可します。GitHub ツールを初めて使うときに、GitHub での認証を求められることがあります。案内に従って認証します。
5. Copilot が Issue を検索し、適切な Issue を見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択し、プルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動テストするために使いました。
- ブランチを公開し、絞り込み機能のプルリクエストを開きました。

MCP サーバーを使って機能を *テスト* しましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティのレビューなど、目的を絞ったタスクを効率化する[カスタム エージェントを作成][next-lesson]します。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
