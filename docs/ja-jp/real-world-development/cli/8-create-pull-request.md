---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを 1 つのブランチに保存しました。すべてをまとめてレビューし、プル リクエストを作成しましょう。最初のプル リクエスト (PR) は手動でマージしました。今回は **Agent Merge** にマージ プロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、絞り込みと品質ワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認します。
- 機能全体の変更と検証の根拠を調べます。
- 絞り込み機能の PR を作成します。
- レビューの後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の開発を通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys は残りの PR 作業を自動化したいと考えていますが、マージの許可は開発者が管理します。

## Agent Merge とは

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビュー コメントに対応し、必要に応じてリベースするなど、PR を妨げている問題に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せられますが、有効にする前に変更をレビューし、マージできる状態であることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したら、すべてをレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功したらマージを実行します。
7. Copilot が「PR #14 was squash-merged successfully.」などのメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、アクセス許可を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストのプロセスなど、開発の複数の段階を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認しました。
- 機能全体の変更と検証の根拠を調べました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
