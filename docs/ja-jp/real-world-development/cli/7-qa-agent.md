---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーから絞り込み機能を確認しました。次は、この機能をカスタム エージェントにまとめ、明確な QA プロセスを定義します。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込みの Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントで QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化型エージェントです。目的、指示、利用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを要件の確認と報告にどう使うかを Copilot に伝えます。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成します。プロファイルでは、QA で行う確認事項と、実行できる操作の境界を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約をプロファイルに反映したことを確認します。確認だけを依頼しているのに、エージェントが機能を変更するような事態を防げます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を確認します。QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. 根拠を示した報告を行うこと、実装コードの変更前に確認を求めること、コミットやプル リクエストを作成しないことを確認します。
5. これらの責務や制約が含まれていない場合は、既定のエージェントに修正を依頼してから続けます。

## Issue を基に QA を実行する

Copilot CLI は会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後、同じ絞り込みの会話を再開し、QA を選択します。これにより、Issue と計画時の決定を引き続き参照できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドを実行すると有効になります。

2. 次のプロンプトで、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 完了したら、作成されたレポートを読みます。

## まとめと次のステップ

ワークフローに特化した役割を再利用可能な形で追加し、作業結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、絞り込みの Issue に照らして結果をレビューしました。

レビュー可能な実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、[それらを機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
