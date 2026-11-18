---
title: "演習 4 - カスタム エージェント"
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

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダーク テーマが読みづらいという意見がありました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot を導く内容を理解できるよう、ファイルを確認します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します。絞り込み機能の PR は演習 3 のブランチに残します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの命令とツールに合わせたチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント ドロップダウンから、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部にあるエージェント ドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集ごとに、ファイル パスとレビューできる差分がチャットに表示されます。
6. エージェントが変更した作業セットに応じて、**Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を行います。Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集する場合があります。

次の演習では、進行中の作業を確認し、エージェントに追加の指示を送ります。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの専門 AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や規約を再利用可能な形式にまとめ、Copilot の作業を導けます。

次の操作を行いました。

- カスタム エージェントの定義方法を学びました。
- Copilot Chat agent mode でカスタム エージェントを使いました。

次は、エージェントの作業を[監視して指示][next-lesson]します。変更内容を確認し、同じセッションにライト モードの切り替えを追加します。

## 参考資料

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
