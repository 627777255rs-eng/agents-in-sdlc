---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、[codespace][codespaces] を起動します。codespace では、命令ファイルを編集し、cloud agent が作成した内容をレビューできます。

## ラボ用リポジトリを設定する

この演習で作成するコードのために、[テンプレート][template-repository]からリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。各演習でこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。

    ![テンプレートを使用するボタン。](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する画面。](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモしておきます。ラボの後半で使います。

## Codespace を作成する

次に、codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを書き、実行し、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、機能豊富な IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面。](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面。](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりもはるかに短時間で済みます。作成を待つ間に GitHub Copilot の機能を確認してみましょう。次の演習から、その機能を使います。

> [!CAUTION]
> 今後の演習で codespace に戻ります。しばらくの間は、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] で動作するように構成されています。どちらの環境にも、スムーズに作業できるよう必要な前提条件がすべてインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成できました。また、Copilot cloud agent と一緒に作業するときに使う codespace の作成も始めました。

## 次のステップ

次は、cloud agent が従うカスタム命令を追加します。[演習 1 - カスタム命令][next-lesson]に進みます。

## 参考資料

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
