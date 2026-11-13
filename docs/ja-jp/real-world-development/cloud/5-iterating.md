---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善し、機能を追加するために、GitHub Copilot に複数のタスクを依頼しました。コードのドキュメント追加、デザイン チームが反復できる関連ゲーム機能の作成、ハイコントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成コードは常にレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者なしで作業するため、安全性を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードを作成するブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、担当者による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプル リクエスト (PR) を確認します。コードにドキュメントを追加するタスクの PR を、GitHub.com の標準的な PR インターフェイスでレビューします。

> [!NOTE]
> PR を確認する際、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択して、プル リクエストの一覧を開きます。
3. **Add missing documentation** など、類似したタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が終わるまで待ちます。数分かかることもあるため、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc コメントなど、更新されたコードを確認します。実際の変更内容は状況によって異なります。演習 2 の冒頭で確認したカスタム指示ファイルによる TSDoc doc コメント、TypeScript の規約、コメント ヘッダーを確認してください。
6. 内容を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プル リクエストで Copilot とやり取りする方法は一方向ではありません。チームのメンバーと同じように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は予測できないため、必ず特定の文面を入力する必要はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントに対する変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更を行うための新しいセッションを開始します。
3. **Back to pull request** を選択すると、プル リクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

満足するまで繰り返し改善できます。準備ができたら、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するために、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、類似したタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す通知を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前回と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば表示する関連ゲームの数を変更したり、コメント ヘッダーや TSDoc doc コメントを追加するよう依頼したりできます（このタスクはカスタム指示を更新する**前**に割り当てたことを思い出してください）。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、類似したタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。完了まで数分待つことがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 変更を確認したら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに従ってこれらの機能を実装するよう Copilot を導きました。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — さらにタスクを委任する

Cloud agent は、バックログの実作業を任せ、その間に別の作業を進めるのに適しています。リポジトリにいくつか Issue を登録し、Copilot に割り当ててみましょう。次のようなタスクが考えられます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリに用意されたカスタム指示ファイルを確認し**、後で cloud agent の出力にどう反映されるかを確認しました。
- **Copilot cloud agent に Issue を割り当て**、環境の設定、計画、非同期の実行を確認しました。
- **カスタム エージェントを作成して使用し**、アクセシビリティ向上のためハイコントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティ セッションを監視し、方向付けました。
- **cloud agent のプル リクエストをレビューして反復し**、変更を依頼するために `@copilot` をメンションし、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学習する場合は、ほかのハーネスで今回の内容を発展させてください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つ届けます。

ご自身のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
