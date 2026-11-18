---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が使える自動化機能の追加などが必要です。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot の CLI に組み込まれている機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを確認します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を試します。

## シナリオ

単体テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように作業中の Web サイトを Copilot が使えるようにすると、変更の動作を確認しながら作業を自動化でき、期待どおりに動くという確信が高まります。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムで通信し、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部のツールやサービスをつなぐ役割を担います。各 MCP サーバーによって、AI エージェントが利用できるツールやリソースが異なります。

広く使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザーの自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献できるように[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、設定手順を飛ばして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を調べてもらう

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを使うよう Copilot に伝えなくても、自動的に判断されます。ただし、使用するツールが分かっている場合は、明示的に伝えても問題ありません。より一貫した結果が得られ、トークンも節約できます。

2. Copilot がブラウザー上で機能を確認するために実行する手順を確認します。
3. レポートを読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認できました。次の操作を行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を試しました。

次は、スキルとブラウザー ツールをまとめた[QA カスタム エージェントを作成][next-lesson]します。

## 参考資料

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
