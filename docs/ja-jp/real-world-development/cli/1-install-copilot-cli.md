---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、フィルタリングの Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をすべてコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップは、もちろんツールのインストールです。幸い、すでに使い慣れているツールを使って実行できます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに AI エージェントを活用し始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルにその機能をもたらします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示された場合は、デバイス コードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されていることがあります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェル プロンプトでは、コマンドは Codespace 上で直接実行されます。Copilot CLI の起動後は、自然言語がエージェントに送信され、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、すべてが動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答する前にリポジトリをどのように探索するか確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルタリングの Issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の簡単な変更を終えた後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチ上で動作し、自動的には worktree を作成しません。各変更の前に feature branch を作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は、設定済みの権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、`--yolo` を付けて再起動します。これはユーザーが許可したショートカットで、Codespace 内で承認プロンプトを省き、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は、すべての権限を自動的に許可します（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳細は[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を使って、後続のレッスンで Issue やプルリクエストを操作するための GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開またはマージする前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をし、会話が動作していることを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って、同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの Issue を見つけました。

次は[最初の焦点を絞った変更][next-lesson]に取り組み、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
