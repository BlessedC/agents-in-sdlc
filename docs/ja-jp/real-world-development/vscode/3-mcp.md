---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode で絞り込み機能を作成しました。プル リクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーでテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- 絞り込み機能をブラウザーで手動テストするよう Copilot に依頼します。
- ブランチを公開し、絞り込み機能のプル リクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部に接続するとさらに強力になります。Model Context Protocol (MCP) は Copilot が外部に接続する方法であり、エージェントが外部のツールやサービスと通信するための標準です。

![agent mode の内部動作と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。AI エージェントはリソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わって操作できます。

これらのツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。新しいリポジトリの作成、既存リポジトリの更新、Issue やプル リクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

そのほかにも多くの MCP サーバーがあります。GitHub は、発見やエコシステムへの貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティ上の影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` サーバーを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot はバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使う際に GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。これを使って機能をテストします。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、Codespace には表示用デスクトップがないため、画面を表示せずにブラウザーを実行するよう Playwright に指示します。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright を使うため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動します。設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して信頼することを選択します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを開いて確認する代わりに、エージェントにページを開かせ、移動し、絞り込みを適用させて、結果を報告させることができます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などのラベル付きの構造化要素を基に判断します。簡単な機能確認が、アクセシビリティの基本チェックにもなります。

サーバーを接続してアプリを実行した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ基準と照らし合わせます。問題がある場合は、PR を作成する前に質問したり、コードの修正を依頼したりします。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前にもう一度起動します。Copilot が初めて Playwright MCP サーバーを使う際に、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプル リクエストを作成する

機能が動作することを確認できたら、チームでレビューするプル リクエスト (PR) を作成します。まず、`filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プル リクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プル リクエストを作成する方法はいくつかあります。GitHub Copilot を使って、関連する Issue を見つけ、それに関連付けて PR を作成してみましょう。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. agent picker で **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使用できるようにします。
3. 次のように Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために許可を求めたら、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の指示に従ってください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプル リクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、ブラウザーで絞り込み機能を手動テストしました。
- ブランチを公開し、絞り込み機能のプル リクエストを作成しました。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
