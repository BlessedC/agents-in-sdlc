---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせて、特化型の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、Copilot の動作を導く指示、ベスト プラクティス、コーディング パターン、領域固有の知識を記述します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントと agent skills の違い

カスタム エージェントと [agent skills][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。わかりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを連携させます。このラボではアクセシビリティのカスタム エージェントがサイトをガイドラインに照らしてレビュー、更新します。その作業の一部として、プル リクエストのワークフロー スキルやテストを実行、管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を活用するほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを行います。

- カスタム エージェントの定義方法を確認します。
- カスタム エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマでは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot をどのように導くか理解しましょう。

codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを持つ YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主要な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、監視し、追加の指示を与えられます。まず、ハイコントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントと Accessibility が強調表示された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

特定のタスクや領域に合わせた GitHub Copilot のカスタム エージェントを確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot の作業を導けます。

この演習では、次のことを確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

Copilot がハイコントラスト モードを実装している間に、[mission control からエージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
