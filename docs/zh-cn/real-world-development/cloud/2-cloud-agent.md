---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理旨在自主执行代码更新、添加功能等任务。代理完成工作后，会创建一个草稿 PR，供开发者审查。这样可以把繁琐的任务交给代理处理，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云代理了解以下内容：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有妥善编写文档——事实上，代码完全缺少文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队也准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，即同一类别中的其他游戏，以便用户继续浏览。目前不要求实现得很精致，只需要足以用于 UX 验收测试。这是当前的一个阻碍，但优先级更高的其他 issue 仍需先处理。

这类任务很容易被降级优先级，因此非常适合交给 Copilot 云代理。云代理可以在后台异步处理，开发者则能继续完成其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式让它开始工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿 pull request 来跟踪进度、设置环境并开始处理任务。无论任务是否完成，你都可以查看 Copilot 会话。提出的解决方案准备好供你审查时，Copilot 会在 pull request 中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 没有魔法，不能替你省去思考。看似简单的操作也可能包含复杂细节，因此[应仔细界定分配给 Copilot 云代理的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程助手：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你完成了自定义指令练习，就已经看到 `.github/copilot-instructions.md` 的效果；如果还没看过，现在正好快速浏览一下。分配任务给 Copilot 云代理之前，先只读查看此存储库中已有的指令文件，稍后就能发现它们的作用。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` - 留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件中。

下一节将 `Code lacks documentation` issue 分配给云代理时，留意生成的 pull request 中是否包含 TSDoc 文档注释、TypeScript 规范以及文件头注释。这些内容来自上述指令文件。后续练习中审查 PR 时还会再次提到。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境准备就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云代理使用 [GitHub Actions][github-actions] 配置工作环境。你可以创建一个[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在代理开始工作前运行，以便安装所需的开发工具和依赖项。为了让实验流程更顺畅并帮助你了解这一机制，我们已预先配置好工作流。它会确保 Copilot 能使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用准备好的 SQLite 数据库（已迁移并填充数据）：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

人人都知道文档很重要，但大多数项目的文档都已过时或根本不存在。这类技术债务经常无人处理，会拖慢效率，也增加维护代码库或让新开发者加入团队的难度。Copilot 擅长创建文档，因此非常适合将这项工作交给 Copilot 云代理。它会在后台生成所需文档，后续练习中再回来审查结果。

1. 在新的浏览器选项卡中，转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 pull request（PR），标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 即将完成的任务。
14. 向下滚动 pull request 时间线，应会看到 Copilot 已开始处理此 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适计划，以及编写代码。

这可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来将利用这一功能：先把另一项任务分配给 Copilot 云代理，然后转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云代理的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组任务。为设计团队添加相关游戏区域可能不会花太久，但这段时间仍可用于其他工作。现在就把它分配给 Copilot 云代理吧！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理你的第二个请求！Copilot 云代理的工作方式类似于软件工程师，你无需主动监控，而是在它完成后再审查结果。现在转而了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。使用云代理可以将 issue 分配给 Copilot，让它异步处理任务。你可以用 Copilot 解决技术债务、开发新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云代理正在后台处理任务，现在可以转而学习如何创建和使用自定义代理。[Copilot 云代理也支持 MCP 服务器][cloud-agent-mcp]，并且可以使用之前模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub issues 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
