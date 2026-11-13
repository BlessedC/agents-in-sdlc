---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際には、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」行うかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに対応した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのが難しいためです。後続の演習で cloud agent が作成したプル リクエストをレビューするときに、指示ファイルの効果を確認できます。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送られる単一の指示ファイルです。Copilot に対するほとんどのチャットや CLI 要求に関係する、プロジェクト全体の情報を記載します。技術スタック、開発対象の概要、ベスト プラクティス、全体的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に対する指示を記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニット テストの追加といったタスク向けに使用できます。

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
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。各環境のレッスンで該当する方法を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスがあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob です。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。複数の指示を小さく再利用可能なファイルに分け、コード生成の例を Copilot に示せます（ここにあるパスはリポジトリ ルートではなく、指示ファイルを基準とした相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で、TSDoc doc コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を追加します。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更をきれいにコミットするためにもブランチを使います。

1. codespace のターミナルで次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。
4. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体向けの `.github/copilot-instructions.md` としても、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` としても作成できます。今回追加したドキュメント コメントのルールのように、プロジェクト全体の標準を定める場合はリポジトリ レベルのファイルが適しています。

## 変更をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチから分岐し、そのブランチにある指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` から分岐します。cloud agent が指示を参照できるよう、変更を `main` に取り込みます。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプル リクエストを作成し、マージします。次の演習で Issue に割り当てたとき、cloud agent はこれらの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成の手順を省略して、`main` に直接コミットします。ブランチを使うことで、実際のプロジェクトでの手順を体験できます。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成するプル リクエストをレビューする際に、この標準が適用されていることを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [指示ファイルなどを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
