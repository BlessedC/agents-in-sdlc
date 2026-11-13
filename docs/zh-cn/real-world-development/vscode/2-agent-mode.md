---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件、再次修改……如此往复。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云代理虽然名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云代理则像团队成员一样异步工作，通过 issues 和 pull request 与你交互。

本练习将了解以下内容：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 GitHub Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发布方筛选。上一练习中已经添加了发布方辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改代码前，先探索 Tailspin Toys 网站，了解它目前的功能。

此网站是一个以开发者为主题的桌游众筹平台。用户可以浏览游戏列表并查看详情。网站是一个单独的 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过精简、限定作用域的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一练习结束后，当前应位于 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应会看到显示 URL 的提示，大致如下：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace（目前正在托管 Web 服务器）的私有隧道！

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、发布方和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始新的对话。不同练习所需的模式和模型可能不同，相关步骤会在适用时说明。

网站的初始实现已经可以正常使用，但我们希望添加新功能。先查看待办事项。根据模板设置项目时，系统已自动为你创建一组 issues；现在让 GitHub Copilot 显示这些事项。

1. 在聊天视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![聊天视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。我们在编写工作坊时使用了 Claude Sonnet 4.5，因此将它列入说明，希望代码建议相对一致，以便提供良好体验。不过，由于大语言模型具有概率性，你可能会发现得到的建议与工作坊中的说明不同。这完全正常，符合预期。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示 issue 待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有 issues 的命令。
5. 查看生成的 issue 列表。

留意 Copilot 还根据对用户的帮助程度，为待办事项排了优先级。

## 审查指令文件

让代理生成代码前，先查看用于向 Copilot 提供上下文的指令文件。接下来会使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 指令文件，其中包含为网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意添加功能时的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以复用内容，将大型指令拆分成更小、更易管理的文件，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用程序进行三项独立更新：

- 在数据层 (`src/lib/`) 添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 代理模式可以完成这些任务！现在来添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认聊天视图的代理下拉菜单中仍选择 **Agent**。

   ![聊天视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中相关 issue 的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作进行时选择。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会有几次暂停，甚至看到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![完整 Copilot 代理模式聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，寻找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你学习了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库前，如何审查 Copilot 代理模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建 pull request。

### 加分探索练习 - 实现分页

随着游戏列表不断增长，网站需要支持分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。可以考虑以下代码实现事项：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 考虑分页方式：允许用户选择页面大小，还是使用固定页面大小。
- 编写提示时，向 Copilot 提供实现所需的指导。
- 可能需要与 GitHub Copilot 反复协作，提出更改请求并补充上下文。这是使用 Copilot 时的正常工作方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途和适用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
