---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、Copilot に特定の作業を効果的に実行させるための命令、ベスト プラクティス、コーディング パターン、分野固有の知識を指定します。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー内、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、命令を定義する Markdown プロンプトが必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整しながら作業できます。この演習では、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローやテストの実行・管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、自分の環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを学びます。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダーク テーマが読みづらいという意見がありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントに機能を生成させます。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot を導く内容を理解できるよう、ファイルを確認します。

codespace に戻り、アクセシビリティのカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

ミッション コントロールは、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加のガイダンスを提供できます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して、割り当てのダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![アクセシビリティ エージェントが選択された、cloud agent の割り当て画面のスクリーンショット。](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの専門 AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や規約を再利用可能な形式にまとめ、Copilot の作業を導けます。

次の操作を行いました。

- カスタム エージェントの定義方法を学びました。
- タスクをカスタム エージェントに割り当てました。

Copilot がハイ コントラスト モードの実装に取り組んでいる間に、[ミッション コントロールからエージェント セッションを監視して指示する][next-lesson]方法を確認します。

## 参考資料

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備をする][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
