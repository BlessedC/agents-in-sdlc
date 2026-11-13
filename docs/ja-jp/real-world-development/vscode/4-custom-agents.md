---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせて、特化型の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、Copilot の作業を導く指示、ベスト プラクティス、コーディング パターン、領域固有の知識を記述します。チームの専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。わかりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを連携させます。このラボではアクセシビリティのカスタム エージェントがサイトをガイドラインに照らしてレビュー、更新します。その作業の一部として、プル リクエストのワークフローやテストの実行、管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を活用するほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。現在のダーク テーマでは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、ユーザーが切り替えられるハイ コントラスト モードをデザイン チームが依頼しました。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot をどのように導くか理解しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティの作業用に `main` から新しいブランチを作成します。演習 3 の絞り込み PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを持つ YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主要な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタム エージェントが、Copilot Chat ビュー下部の agent picker に表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールを使って会話できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. Chat ビューの agent picker で、**Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agent picker を選択します。現在のエージェント名が表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルに適用します。変更はファイル パスと差分とともにチャット内に表示されます。
6. エージェントが変更したファイルに合わせて **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、進行中の作業を確認し、方向付けます。

## まとめと次のステップ

特定のタスクや領域に合わせた GitHub Copilot のカスタム エージェントを確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot の作業を導けます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタム エージェントを使いました。

次は[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加するよう依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
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
