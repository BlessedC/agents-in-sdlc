---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で GitHub Copilot CLI を使い、一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みの絞り込み Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- 絞り込みの Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで作成し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証内容を評価しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、更新した `main` からそれぞれ作成した 2 つの PR があります。

1. **星評価:** 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **絞り込みと品質ワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加します。

絞り込みを計画してから PR を作成するまで、同じ会話とブランチを使いました。手順を簡潔にするため、これらの変更を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式でレポートしました。QA は要件とテスト カバレッジのレビューにその結果を加え、PR に向けて確認しました。

追加するテストは実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果をレビューし、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストやツールが、作業結果を左右します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの設定を行いました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選択します。作成前にアプローチを検討するときは **Plan**、範囲が明確な変更を対話しながら進めるときは **Interactive**、明確に定義したタスクを任せるときは **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確にすると、出力が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコード レビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題について話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールの使い方を上達させる最善の方法は、使い続けることです。本番コード、趣味のコード、長年作ってみたいと思っていた小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
