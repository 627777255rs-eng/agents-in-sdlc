---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を誰でもアクセスできるようにしましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages デプロイの設定を依頼します。
- ワークフローとプルリクエストをレビューしてからマージします。
- デプロイを確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試せる場所がまだありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイム依存関係を追加せずに、公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストの作成によって、コードがコミットおよびプッシュされることがあります。プロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**すべて承認**に設定されている場合、個別の確認は表示されません。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. リポジトリに README がまだない場合に限り、追加されていることを確認します。
2. ワークフローのビルドとデプロイ手順、トリガー、権限を含むプルリクエストの変更をレビューします。
3. ワークフローが、未マージのプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新された差分をレビューします。マージを許可する前に、必須のプルリクエスト チェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送ると、レビュー済みプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作に別途確認が求められない場合があります。

`<PR_NUMBER>` をレビューしたプルリクエスト番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージされた space-quiz のプルリクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで完了とみなさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告に頼らずデプロイを確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
