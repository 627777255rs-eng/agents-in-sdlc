---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を利用するデスクトップ アプリです。Issue やプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリをローカル環境で更新します。始める前に、Node.js がローカルにインストールされていることを確認し、Copilot app をインストールしましょう。

このレッスンでは、次のことを行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには **[Node.js][nodejs]** が必要です。これはプロジェクトに必要な唯一のランタイムです。現在の **LTS** リリースをインストールします。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナルなど、普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、インストール済みの Node.js のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. 両方のコマンドが成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、これらの手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナル ウィンドウを開きます。
8. 新しいターミナル ウィンドウで次を実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールした Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後、または PATH の変更後は、アプリを完全に終了して再起動してください。セッションでどちらかのコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を確認します。

> [!IMPORTANT]
> E2E チェックには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備するときは Tailspin Toys リポジトリの README に従い、インストール要求は内容を確認してから承認してください。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレート リポジトリ][template-repository]から作成しましょう。新しいリポジトリにはラボに必要なすべてのファイルが含まれており、アプリをインストールするときに接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` に移動します。
2. ラボ用リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択して、自分用のリポジトリを作成します。

    ![「Use this template」ボタンのドロップダウンで「Create a new repository」が選択された画面](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターから案内された手順に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![テンプレートに github-samples/tailspin-toys が設定され、リポジトリ名が入力された「Create a new repository」フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。ラボの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップではこの Issue を使って作業するため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使用してください。リポジトリには、指示、アプリケーション コード、テスト、`quality-checks` スキル、既存の Canvas 拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使用する場合は、必要なファイルが含まれていることをファシリテーターに確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- プロジェクトをコンピューターでビルドおよびテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は[GitHub Copilot app をインストール][next-lesson]し、作成したリポジトリを接続して、ワークスペースの基本を確認します。

## リソース

- [Node.js のダウンロード][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
