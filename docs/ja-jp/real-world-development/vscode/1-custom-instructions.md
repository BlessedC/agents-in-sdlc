---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用する際には、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、コンテキストを利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。主な方法の 1 つは[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成のガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに対応した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、絞り込み用の最初のデータ処理（発行者ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用される様子を確認します。
- 次の演習で更新されたガイダンスを利用できるよう、指示の変更と絞り込みの処理を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合もあります。Copilot の動作は確率的です。この演習の目的は、指示の更新後に動作がどのように変化するかを見ることであり、出力を一字一句同じにすることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードで、現代的な印象にします。
- コードには TSDoc doc コメントでドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件について Copilot にコンテキストや好みを伝え、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送られる単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、プロジェクト全体のコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けの指示を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは、プロジェクトに含まれるファイルを例として、指示ファイルの代表的な使い方を紹介します。基本的な方針は次のとおりです。

- `copilot-instructions.md` は、開発対象やプロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関わるガイダンスに絞ります。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスク向けの指示に使います。
- 自然な言葉で、明確なガイダンスと例を記載します。

指示ファイルの使い方に唯一の正解はありません。AI の使い方と同様に、試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、環境によって方法が異なります。各環境のレッスンで該当する方法を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** のガイダンスがあります。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob です。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分け、Copilot に例を示せます（ここにあるパスは指示ファイルを基準とした相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で、TSDoc doc コメントとファイル先頭のコメント ヘッダーを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習のエディター タブを閉じ、Copilot に必要なコンテキストだけを残します。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューの agent picker で **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューの agent picker のスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを適用します。ヘルパーとテストを含む複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、発行者の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約によるものです。
7. 提案されたコードに TSDoc doc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の向上が、この演習のポイントです。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体向けの `.github/copilot-instructions.md` としても、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` としても作成できます。プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を再度選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習の基礎となるコードに適用できました。

## 指示と変更をコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同様に、ソース管理で管理します。指示の更新とヘルパーの変更をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プル リクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue に関連付けたプル リクエストを作成します。GitHub のツールがプロジェクトに接続されているので、Copilot に Issue を探し、Issue を解決する PR を作成して、マージを依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で作業するのは初めてです。プロジェクトのテンプレートには GitHub ツールがすでに接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** を選択します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新と発行者ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を実行する許可を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
5. PR の作成後、PR を `main` にマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作とシェル コマンドの実行許可を求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプル リクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用されることを確認しました。
- 次の演習で使えるよう、指示とヘルパーの基礎をコミットして `main` にマージしました。

次は [agent mode][next-lesson] を使って、Copilot にコードベース全体にまたがる機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [指示ファイルなどを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
