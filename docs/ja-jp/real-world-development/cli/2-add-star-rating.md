---
title: "レッスン 2 - 星評価を追加する: すぐに得られる成果"
description: "Copilot CLI でゲーム カードを変更し、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小さく保ちましょう。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで、目的を絞った Copilot の会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送されたブラウザーで確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードに表示されるのは、タイトル、カテゴリー、発行元、説明だけです。まずはウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。初めてのセッションに適した、小さく完結した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話から後で同じ話題に戻れます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始めて、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを特定して、必要なコードを作成します。Copilot CLI で新しい機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲーム カードに表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは毎回異なる場合があります。特定の実装と照らし合わせるのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. 次のように Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードをプロジェクトにマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI が PR を作成する際に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選び、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、最初の変更をリリースできました。次の操作を行いました。

- 機能ブランチで、目的を絞った Copilot の会話を始めました。
- ゲーム カードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[絞り込み機能の Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を実装します。

## 参考資料

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
