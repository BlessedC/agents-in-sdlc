---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づく絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから、典型的なエージェント主導の開発プロセスを進めます。Issue を使って新機能を作成し、コードが正しいこと、機能が期待どおりに動作することを確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能開発を進めながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のゲーム カタログが増えてきたため、訪問者がカテゴリや発行者でゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディングの前に合意する詳細があります。Plan モードで決定し、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本的な原則は変わりません。むしろ、より重要になります。多くの開発者は次のような手順で作業します。

1. 実装内容を記載した Issue から開始します。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、通常はここに示した手順を応用した流れになります。

標準的な手順を守ることで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビュー プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントがどの程度自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントがコードの作成、テストの実行、反復を自律的に行い、タスクを完了します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

絞り込み機能に着手する前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認します。`#` を使うと、GitHub 上の Issue やプル リクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てることで、Copilot に依頼する前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、計画に時間をかけることをお勧めします。Plan モードに切り替えて計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に表示されるモードが **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を決めて計画を作成します。

3. 作業中に Copilot から絞り込みの動作について質問されることがあります。希望に合わせて回答してください。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 決めた内容と異なる箇所があれば、修正を依頼します。

## Autopilot を承認する

計画の作成とレビューが完了したら、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)**（またはインストール済みのバージョンで同様の名前の項目）を選択します。
2. プロンプトの下に表示されるモードが **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画をレビューしてください。依存関係が見つからない、ポートが競合しているなどの問題が報告された場合、セットアップの問題を解決してからチェック結果を確認してください。

## 実装をレビューして検証する

コードをマージする前にレビューする必要があります。生成されたコードを確認し、サイトを起動して問題がないか確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を実行し、絞り込みの実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容と照らし合わせます。
4. コードのレビューが完了したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を試す

コードに問題がないことを確認したら、アプリを起動し、Codespaces のポート転送を通じてサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を使用可能と通知したら、**Open in Browser** を選択します。
3. 計画で決めたとおりに、カテゴリと発行者による絞り込み、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

さまざまな会話モードを使って、機能を作成しレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム指示を使って][next-lesson]コードが記載された慣行に従うようにし、コード生成の仕組みをさらに確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
