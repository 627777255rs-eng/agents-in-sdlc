---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを調べ、レポート形式をカスタマイズし、フィルタリングを検証するために使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを書くことだけではありません。これまでは、コードが動作することを手動で検証し、指示ファイルを使って標準に従うようにしてきました。しかし、テストはどうでしょうか。Lint はどうでしょうか。継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が最適です。スキルを使うと、これらの操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを調べます。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、Lint、型チェックに `quality-checks` スキルを使用しています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中の `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキル名と説明を定義する frontmatter、スキルの概要、呼び出すべき状況のガイダンスを記述します。フォルダーには、スクリプトやスキルが使用するその他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに配置すると、チームで共有・再利用できるリポジトリ アセットになります。また、ユーザーのスキル フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを調べる

Tailspin Toys チームが単体テスト、Lint、型チェックの実行に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、`.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。説明によって、Copilot がいつスキルを呼び出すか判断できることを確認します。
3. 指示を読み、テストと Lint の手順がどのように説明されているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。まず、3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードで、フィルタリングの会話に戻ります。
2. 次のプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したかどうか、ツールが実際に報告した内容が分かるレポートに改善しましょう。結果をこの形式で報告するよう、スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたら、実際に動作を確認しましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- 単体テスト、Lint、型チェック用の `quality-checks` スキルを調べました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR に含めます。次は、[Playwright MCP サーバーを使ってサイトを直接操作][next-lesson]します。

## スキルの例

以下のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
