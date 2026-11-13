---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作处理了多个任务，目标是改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建相关游戏功能供设计团队迭代，并实现包括高对比度和浅色模式切换在内的无障碍功能。现在查看代码更改，并在需要时向 Copilot 提供反馈，改进其工作成果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用审查之前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改。

## 安全和 GitHub Copilot 云代理

由于 Copilot 云代理会在没有监督的情况下异步执行任务，因此系统设置了一些安全限制，以确保操作安全：

- Copilot 只有存储库的读取权限，并且仅对用于编写代码的分支具有写入权限。
- 云代理在 GitHub Actions 中运行，并创建独立、临时的工作环境。
- 运行任何 GitHub Actions 工作流前，都需要人工审批。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云代理创建的第一个 pull request（PR），其中包含为代码添加的文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器调用。如果需要，可以[自定义或禁用 Copilot 云代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 pull request 列表。
3. 打开标题类似于 **Add missing documentation** 的 pull request。

> [!NOTE]
> 如果 Copilot 仍在处理任务，pull request 会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下，也可以回顾到目前为止的学习内容。

4. pull request 准备就绪后，选择 **Files changed** 选项卡，审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改可能不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 规范和文件头注释。这些要求来自你在练习 2 开始时审查的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待审批的提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 pull request 的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在 pull request 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果并非确定性，因此无法给出固定的提示词。可以让 Copilot 完成以下更新：

- 在每个代码文件顶部添加简要说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加一条评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 完成工作。注意 Copilot 会启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回 pull request。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，pull request 中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏”issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到有工作流正在等待审批的提示。
7. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在 pull request 的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，可以通过迭代让 Copilot 云代理进行更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。此 PR 应该同时包含练习 3 中要求的高对比度模式，以及练习 4 中在任务控制台要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，pull request 会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有工作流正在等待审批的提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 pull request 的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，确认功能正常，并且刷新页面后设置仍然有效。

注意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进点，可以像前面处理其他 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云代理最适合处理可以交给它、让你继续做其他工作的真实待办事项。可以在自己的存储库中再创建几个 issue 并分配给 Copilot，培养这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理学习路径。在这些课程中，你：

- **审查了存储库自带的自定义指令文件**，稍后可以观察这些指令对云代理输出的影响。
- **将 issues 分配给 Copilot 云代理**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**来改善无障碍体验，添加高对比度和浅色模式开关。
- **使用 Copilot 代理页面作为任务控制台**，监控并引导无障碍会话。
- **审查并迭代优化云代理创建的 pull request**，通过提及 `@copilot` 请求修改并审批工作流。

## 回顾和后续步骤

云代理学习路径已完成。如果想继续探索，其他学习路径可以补充本次练习内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

还可以在自己的存储库中尝试以下后续操作：

- 将重构或测试覆盖率 issue 分配给云代理。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issues 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
