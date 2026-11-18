---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成し、動作をテストし、プルリクエストをレビューし、エージェントと作業を管理する反復的な流れを体験します。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として作業します。同社はボードゲームのクラウドファンディングを運営しています。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。ゲームの絞り込みやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発スタイルに合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを活用します。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
