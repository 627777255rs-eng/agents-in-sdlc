---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、作成するコードの内容だけでなく、コードの構成方法も指定できます。絞り込み機能を実装したので、Copilot が使った命令を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令と、パスを指定した命令ファイルがエージェントに届く仕組みを確認します。
- コーディング規約を守るために命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys のような開発チームには、開発手法に関するガイドラインや要件があります。たとえば、次のような内容です。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その説明も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を説明します。関連するコードを変更した場合は、コメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令は、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定です。希望するコーディング規約、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する命令と、特定の種類のファイルだけに適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する **すべて** のリクエストで Copilot に送信される単一の命令ファイルです。Copilot へのほとんどのチャットまたは CLI リクエストに関係する、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> その他の命令形式とサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

作業を始めやすいように、スターター プロジェクトには命令ファイルがいくつか含まれています。変更を加える前に、すでにあるファイルを確認して、その影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関するガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、命令を適用するファイルを指定する glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、長い命令セットを再利用可能な小さなファイルに分割し、参考にするファイルを示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは出発点として適切ですが、まだ追加が必要です。中心となる `copilot-instructions.md` を編集し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目あたりにあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。命令を保存した後、機能のコンテキストを失わずに新しいガイダンスを読み込めるよう、絞り込み機能の会話を終了して再開します。

1. 絞り込み機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、絞り込み機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のように Copilot にガイダンスの適用を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルのコンテキストを読み込む仕組みを確認し、新しい規約を機能に適用しました。次の操作を行いました。

- リポジトリの命令と、パスを指定した命令ファイルがエージェントに届く仕組みを確認しました。
- コーディング規約を守るために命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な品質チェックを確実に実行するため、[quality-checks スキルをカスタマイズして実行][next-lesson]します。

## 参考資料

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
