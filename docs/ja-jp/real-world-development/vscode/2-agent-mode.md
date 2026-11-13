---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は、関連ファイルを探し、変更を行い、テストを実行し、問題をデバッグし、変更漏れがあればさらに修正するという一連の作業を行います。Copilot Agent Mode は、こうした作業をより自律的に進めるための機能です。

Copilot Agent Mode は、IDE 内で開発者に近い方法で動作します。まず既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。Agent mode は IDE 内で動作し、すばやいフィードバックとやり取りが可能です。Cloud agent はチームの一員のように非同期で作業し、Issue やプル リクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけ、複数のファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行者で絞り込めるようにします。前の演習で発行者ヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認し、現在の機能を理解します。

この Web サイトは、開発者向けテーマのボードゲーム用クラウドファンディング プラットフォームです。ゲームの一覧や詳細を表示できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` に置き、インタラクティブな動作には標準の DOM API を使う小さなスコープ付き Astro `<script>` を使用します。

### Web サイトを起動する

Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されます。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューの agent picker で **Agent** を選択します。Agent は複数のファイルにまたがる変更を計画、実装し、ターミナル コマンドやツールを実行します。

   ![Chat ビューの agent picker のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。提案の一貫性を高めることが目的ですが、LLM は確率的に動作するため、説明と異なる提案が表示されることもあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを送信して、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つと思われる項目に基づいて、優先順位も付けています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。この演習では、Web サイトへの機能追加方法に関するコンテキストを持つ [ユーザー インターフェイス (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加方法に関する全体的なガイダンスを確認します。内容には、アーキテクチャの概要、コンポーネントの設計・テスト可能性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの特定ファイル向け指示へのリンクが含まれます。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きなファイルを小さく分け、Copilot に例やテンプレートを示すことができます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 絞り込み機能を実装する

絞り込み機能を完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データ層 (`src/lib/`) に絞り込みロジックを追加または調整します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、コードベースにマージする前にテストが成功する必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent picker で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを特定して変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。Copilot の作業中に選択する必要はありません。完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot が促したら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認ダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまで、コード生成とテストの間を行き来して修正します。

   ![Copilot Agent Mode の完了した Chat セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys Web サイトに機能を追加しました。この演習では、次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- プロジェクトを調べ、関連ファイルを見つけ、複数のファイルにまたがる変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲームの一覧が増えるにつれて、ページ分割が必要になります。この演習で学んだ方法を使って、Copilot にページ分割の実装を依頼してみましょう。

- 指示ファイルなど、既存のベスト プラクティスに従います。
- ユーザーがページ サイズを選択できるようにするか、値を固定するかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを伝えながら何度かやり取りすることがあります。Copilot を使う場合の通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの違いと使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
