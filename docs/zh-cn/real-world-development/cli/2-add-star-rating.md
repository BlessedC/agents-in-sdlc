---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个 pull request 合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次更改了。先从小处着手：游戏数据中已经包含星级评分，但主页上的游戏卡片还没有显示它。接下来让代理显示评分、审查更改，并将其作为第一个 pull request 合并。

本课将完成以下内容：

- 在功能分支上启动专注于某项任务的 Copilot 对话。
- 让代理对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 通过转发到浏览器的端口运行应用并确认更改。
- 创建并合并第一个 pull request。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示评分。不过，主页上的游戏卡片目前只显示标题、类别、发布方和说明。作为热身练习，你将让代理在每张卡片上显示现有评分。这是一个小而独立的更改，非常适合首次使用会话。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot 应用不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用 worktree。保存的对话可以让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，你会看到三类内容：你发送的提示和代理的回复、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

新建一个对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到显示游戏详情的文件，并创建所需代码。现在已经使用 Copilot CLI 添加了新功能！

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使更改很小也不例外。现在就在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片会在有评分时显示数字评分，并在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性，并非确定性工具，因此实际代码可能不同。应审查行为，而不是期待某一种完全一致的实现。

## 检查更改

当然，不能只阅读代码并假设它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发端口打开的浏览器中检查更新后的用户界面（UI）。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个 pull request

功能已创建完成！接下来创建 pull request（PR），将新代码合并到项目中。

1. 要求默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，你已使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上启动了专注于某项任务的 Copilot 对话。
- 引导代理对游戏卡片进行了小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发到浏览器的端口中确认星级评分。
- 创建并合并了第一个 pull request。

接下来，从筛选 issue 开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
