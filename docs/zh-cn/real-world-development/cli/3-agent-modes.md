---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据 issue 构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

之前已在项目中添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，确保我们以正确的方式构建正确的功能。接下来的几节将遵循典型的代理式开发流程：从 issue 开始创建新功能，确保代码有效且功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 继续完成此功能时，将使用同一对话和分支。通常会针对不同文件类型使用不同分支或 PR，但为专注于核心概念，本工作坊会简化流程。

本课将完成以下内容：

- 根据 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发到浏览器的端口中手动验证功能。

继续完成此功能时，还会更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，用户需要按类别和发布方筛选游戏。待办事项中的 issue 已描述此功能，但组合类别筛选等细节需要在编写代码前确定。你将使用 Plan 模式明确这些决定，然后授权 Autopilot 在限定范围内完成实现。

## 背景

将 AI 编码代理引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者的工作流程大致如下：

1. 从已提交并描述需求的 issue 开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建 pull request（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准方法，可以确保 AI 生成的代码满足既定要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试并迭代，直到任务完成。

先使用 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issues。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

留意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或 pull request（PR）加入上下文。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方案，再交给 Copilot 处理。处理复杂任务时，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan** 模式指示器。
2. 在上一步添加的 issue 引用之后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳方案。

3. 过程中，Copilot 可能会询问筛选功能的具体行为。根据自己的偏好回答即可，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在开始实现！使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中名称相似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前也需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 对照 issue 和规划阶段确定的决策检查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发布方筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课中，你：

- 根据 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发到浏览器的端口中手动验证了功能。

接下来，通过[自定义指令][next-lesson]深入了解如何让代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
