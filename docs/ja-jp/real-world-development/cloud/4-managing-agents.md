---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプル リクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに方向付けを行ったりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼して、方向付けます。

## シナリオ

エージェントにハイコントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能を更新する作業をすでに行っているため、ライト モードも含めるのが合理的です。エージェントの作業に追加の指示を行い、ハイコントラストに加えてライト モードも実装します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方にタスク一覧があります。Copilot に割り当てた次のタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲーム変更用の API を生成する。
    - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。コードをチェックアウトする、Copilot の環境を作成する、MCP サーバーを設定する、割り当てられたタスクを完了するための各種手順を実行するといった操作が含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプル リクエスト (PR) ペインも確認します。ここから PR と変更ファイルを確認し、作業を監視できます。

## Cloud agent に方向付けを行う

実行中のタスクを確認できたので、ハイコントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラスト モードを追加しているセッションを選択します。作業の名称や進捗状況によって、表示されるタイトルは異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中の cloud agent タスクで、セッションの方向付けダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot に作業を任せる

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を完了した後、新しい依頼をワークフローに反映します。

今回も数分かかります。その間に、これまでに学習した内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、方向付ける中心的な場所である Copilot agents ページを確認しました。mission control から、進行中と完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクの範囲を変更したり追加の指示を与えたりできます。

このレッスンでは、次のことを行いました。

- mission control と agents ページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼して、方向付けました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプル リクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
