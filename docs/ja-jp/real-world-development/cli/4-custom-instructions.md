---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認してドキュメント標準を追加し、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用する際には、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構造や書き方も指定できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに対応した指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入する場合は、そのことも記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明します。関連するコードを変更するときは、コメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件について Copilot にコンテキストや好みを伝え、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送られる単一の指示ファイルです。ほとんどの要求に関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すぐに利用できる指示ファイルがあります。変更を加える前に、既存の内容と変更による影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、リポジトリ内で Copilot を使うすべての作業に適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これは指示を適用するファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot に例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として適切ですが、改善の余地があります。中心となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが付くようにします。

1. `.github/copilot-instructions.md` で、35 行目付近の **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新後の指示を適用し、機能に関するコンテキストを保つために、絞り込みの会話を終了して再開します。

1. 絞り込みの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、絞り込みの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを確認します。新たに追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを参照する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスに対応した指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [指示ファイルなどを集めた Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
