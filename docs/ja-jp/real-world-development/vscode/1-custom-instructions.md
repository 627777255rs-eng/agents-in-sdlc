---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。特に重要なのが、コード生成のガイダンスを伝える[命令ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム命令と、パスを指定した命令ファイルによって、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメントの規約が Copilot に届く仕組みを確認します。
- 現在の命令が適用された状態で、絞り込み機能の最初のデータ部分 (発行元のヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の規約を追加します。
- 同じプロンプトを再実行し、生成されるコードに新しい規約が反映されることを確認します。
- 命令の更新と絞り込み機能の作業を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、設定した規約の一部に従わない場合があります。Copilot の動作は確定的ではありません。この演習では、命令を更新した後の動作の *傾向* を確認します。生成結果の文字列を完全に一致させることが目的ではありません。

## 命令ファイル

### シナリオ

開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のような内容です。

- データ レイヤーには、必ず単体テストを作成します。
- UI はダーク モードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot が適切な情報を得て、示された慣行に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令は、コーディング スタイルや要件を Copilot が理解するためのコンテキストや設定です。希望するコーディング規約、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する命令と、特定のファイルの種類やタスクだけに適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する **すべて** のリクエストで Copilot に送信される単一の命令ファイルです。Copilot へのほとんどのチャットまたは CLI リクエストに関係する、プロジェクト レベルの情報を記載します。技術スタック、開発中の内容の概要、ベスト プラクティス、一般的なガイダンスなどが該当します。
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
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイル (たとえば `unit-tests.instructions.md`) や既存ファイルへのリンクを確認します。これにより、命令セットを再利用可能な小さなファイルに分け、コード生成の参考となる例を Copilot に示せます。ここにあるパスはリポジトリのルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング規約を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順では、TSDoc のドキュメント コメントとファイル コメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従って、変更用ブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する *前* に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルが適用された状態でプロンプトを送信します。その後、命令を更新して同じプロンプトを再度送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. Copilot にヘルパーの場所を知らせるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。Agent はファイルに変更を適用できます。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを更新します。多くの場合、ヘルパー ファイルとテストが変更されます。
6. 提案されたヘルパーが、型付けされた関数であることを確認します。この関数は最初の引数として `db` クライアントを受け取り、型付きの発行元配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが **ない** ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。命令を更新した後の *一貫性* の向上を確認することが、この演習の目的です。

## リポジトリに新しい規約を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コードの提案を改善するために、リポジトリのコーディング規約を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目あたりにあります。このセクションではプロジェクトのコーディング規約を説明していますが、コード内のドキュメントに関する規則はまだありません。これが、生成されたヘルパーにドキュメント コメントがなかった理由です。
3. 既存の規約のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、命令ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` ファイルとして作成することもできます。今追加したドキュメント コメントの規則のような、プロジェクト全体の規約にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行し、変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. Copilot に適切なファイルのコンテキストを与えるため、`src/lib/publishers.ts` を再度選択します。
3. 先ほどと **同じプロンプト** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの冒頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト規約に従わせ、次の演習で使う実際のコードに適用できました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. 次のコマンドを実行し、命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで命令ファイルと、ヘルパーの基盤作成で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加更新が表示される場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けてプルリクエストを作成します。手動で行う代わりに、すでに接続されている GitHub ツールを Copilot に使わせます。Issue を見つけ、Issue をクローズする PR を作成して、マージを依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートには GitHub のツールがあらかじめワークスペースに接続されています。Copilot が初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。案内に従って許可してください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 命令ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR では命令の更新と発行元ヘルパーの基盤作成の両方を説明します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を Copilot に許可するよう求められたら、コマンドを確認し、必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェル コマンドを Copilot に許可するよう求められたら、コマンドを確認し、必要に応じて **Allow** を選択します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを読み込む仕組みを確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- プロンプトを送信し、*既存の* 命令で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` に新しい全体規約を追加しました。
- 同じプロンプトを再実行し、提案されたコードに規約が反映されることを確認しました。
- 命令の更新とヘルパーの基盤を `main` にコミットして、次の演習で使えるようにしました。

次は、agent mode で命令を[機能の実装に適用][next-lesson]します。

## 参考資料

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
