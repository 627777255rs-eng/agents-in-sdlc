---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令と、パスを指定した命令ファイルによって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの規約が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更の前後でプロンプトの結果を比較しません。Copilot cloud agent は GitHub Issue で非同期に動作するため、影響をリアルタイムで並べて示すのは困難です。cloud agent が作成したプルリクエストを後の演習でレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のような内容です。

- データ レイヤーには、必ず単体テストを作成します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使うと、Copilot が適切な情報を得て、示された慣行に沿ってタスクを実行できるようになります。

### カスタム命令

カスタム命令は、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定です。希望するコーディング規約、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する命令と、特定のファイルの種類やタスクだけに適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する **すべて** のリクエストで Copilot に送信される単一の命令ファイルです。Copilot へのチャットまたは CLI リクエストのほとんどに関係する、プロジェクト レベルの情報を記載します。技術スタック、開発中の内容の概要、ベスト プラクティス、一般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に命令を適用します。TypeScript などの言語や、UI コンポーネントまたは新しい単体テストの作成などのタスク向けにガイドラインを設定できます。

> [!NOTE]
> IDE では、命令ファイルが使われるのは Copilot Chat でのコード生成だけです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> そのほか、AGENTS.md や CLAUDE.md など、別の標準に基づく命令ファイルも Copilot で利用できます。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成方法全体を説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには、代表的な方法を示すファイルが含まれています。基本的には、次のようにします。

- `copilot-instructions.md` では、作成中のアプリケーション、プロジェクトの構造、全体的なコーディング規約など、プロジェクト レベルのガイダンスを扱います。
- `*.instructions.md` ファイルでは、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル種類や特定のタスクに関する命令を扱います。
- 自然な言葉を使い、明確なガイダンスを記載します。コードをどのように書くべきか、または書くべきでないか、例を示します。

AI の使い方に唯一の正解がないように、命令ファイルの作成方法にも決まりきった方法はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、適切な命令ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルが見つかります。
>
> Copilot に命令ファイルを作成してもらうこともできます。手順は環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。現在使用している環境のレッスンで、関連する場合に説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] リポジトリには、命令ファイル、カスタム エージェント、その他のリソースが多数あります。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがいくつか含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** では、**GitHub Actions Workflows** の関連ガイダンスも確認します。これらの命令は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、命令を適用するファイルを決める glob を設定します。この例では、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. 単体テストを作成する際の、このプロジェクト固有の命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の命令ファイル (たとえば `unit-tests.instructions.md`) や既存ファイルへのリンクを確認します。これにより、命令セットを再利用可能な小さなファイルに分け、コード生成の参考となる例を Copilot に示せます。ここにあるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング規約を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## リポジトリの新しい規約を追加する

この演習で編集するのは、プロジェクト全体に適用される TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーに関する規則です。cloud agent は、このハーネスの後半で割り当てる Issue に取り組むときに、この規則を参照します。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象となるブランチから命令を読み込みますが、変更を整理してコミットするためにもブランチを作成します。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を与えるためのファイルです。コードの提案を改善するために、コーディング規約を文書化しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目あたりにあります。このセクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する規則はまだありません。これが、生成されたヘルパーにドキュメント コメントがなかった理由です。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、命令ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` ファイルとして作成することもできます。今追加したドキュメント コメントの規則のような、プロジェクト全体の規約にはリポジトリ レベルのファイルが適しています。

## 命令の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチから命令ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されます。cloud agent が命令を使えるように、変更を `main` に取り込みます。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue に割り当てると、cloud agent はこの命令を参照します。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは実際のプロジェクトに近い手順を体験できるよう、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの命令ファイルを確認し、`.github/copilot-instructions.md` に新しい全体規約を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この規則が使われる様子を確認できます。

次は、Copilot cloud agent に[最初の Issue を割り当てます][next-lesson]。

## 参考資料

- [GitHub Copilot の命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
