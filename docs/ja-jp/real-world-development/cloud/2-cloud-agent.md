---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織が技術的負債に悩んでいます。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要求などがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。エージェントは作業を終えると、開発者がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを十分に整備しておらず、アプリケーションにはほとんどドキュメントがありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc doc コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。ユーザー エクスペリエンス (UX) の受け入れテストに使える状態にすることが目的です。現在は作業がブロックされていますが、優先度の高い Issue がほかにあります。

これらは優先順位が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期に作業を進められます。その間、別の作業に取り組み、後で Copilot の変更をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者のようにバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を記録するドラフト プル リクエストを作成し、環境を準備して、タスクに取りかかります。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プル リクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後でその影響を確認できます。

リポジトリの GitHub Web UI、または codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc doc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、これらの指示により TSDoc doc コメント、TypeScript の規約、コメント ヘッダーが反映されることを確認します。後の演習で PR をレビューする際にも取り上げます。

## Copilot cloud agent の開発環境を設定する

コードの作成には、通常、必要な開発ツールや依存関係を使用できるようにする環境とセットアップが必要です。SWE のように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は GitHub Actions を使って環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に開発ツールや依存関係をインストールできます。演習の手順を円滑に進められるよう、このラボではワークフローを事前に設定しています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用ブラウザー、単一 Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが成功することを確認できます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが理解していますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成が得意なため、cloud agent に依頼するタスクとして適しています。バックグラウンドでドキュメントを作成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプル リクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境（前述のワークフローを含む）を作成し、プロジェクトを分析して、最適な方法を検討しています。

13. todo リストと、Copilot が実行するタスクを確認します。
14. プル リクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が現れるまで、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成していきます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的は、非同期に作業して、別の作業に取り組めるようにすることです。この利点を活かすため、もう 1 つのタスクを cloud agent に割り当てた後、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間をほかのタスクに使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに、Copilot が作業中であることを示す 👀 が表示されます。

![Copilot が Issue に対応していることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。次は、カスタム エージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、タスクを非同期に実行できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作が安全に実行されるようにしました。
- 範囲を明確にした Issue の重要性を確認しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、[カスタム エージェントの作成と使用][next-lesson]に進みます。[Copilot cloud agent では MCP サーバー][cloud-agent-mcp]と、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
