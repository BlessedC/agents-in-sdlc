---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建 pull request 前，应确认它在浏览器中能够正常工作。你不必亲自操作应用，而是可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器为你测试功能，然后发布分支并创建 PR。

本练习将完成以下内容：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具。
- 在工作区中添加 Playwright MCP 服务器。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建 pull request。

## 什么是 Model Context Protocol (MCP)？

当代理模式能够访问编辑器之外的资源时，它的能力会大幅增强。Copilot 正是通过 Model Context Protocol (MCP) 实现这一点——MCP 是代理与外部工具和服务通信的标准方式。

![代理模式的内部工作原理及其与上下文、LLM 和工具（包括 MCP 服务器及 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

MCP 工具和资源通过 MCP 服务器访问。MCP 服务器负责管理 AI 代理与外部工具之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

目前有一些广受欢迎的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组管理 GitHub 存储库的 API。AI 代理可以使用它创建存储库、更新已有存储库，以及管理 issues 和 pull request。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 代理可以使用它浏览网页、填写表单和选择按钮。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 从安全角度看，应像对待项目的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板自带此 `github` 配置，因此 Copilot 才能在上一练习中读取 issue 待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境可显示浏览器的 codespace 来说是必需的。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器（并要求你确认是否信任）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操作的真实浏览器。你无需亲自操作应用来验证更改；代理可以打开页面、导航、应用筛选条件、读取结果并向你总结观察结果。这是在不离开对话的情况下，快速确认功能是否符合预期的方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，代理会基于带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同——快速功能检查也能兼顾基本的无障碍检查。

连接服务器并启动应用后，要求 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。根据 issue 中的验收条件审查报告；如果发现异常，请提出后续问题，或要求 Copilot 在创建 pull request 前修复代码。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，就可以创建 pull request（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标，暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的源代码管理面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建 pull request

可以通过多种方式创建 pull request，例如使用 github.com 或 GitHub 命令行界面（CLI）。不过，既然已在使用 GitHub Copilot，不如让它创建 PR！它可以找到相关 issue，并创建与之关联的 PR。

1. 转到 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在代理下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你登录 GitHub；按照提示完成登录。
5. 留意 Copilot 如何搜索 issues、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接审查 pull request，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 使用该服务器在发布筛选功能前，通过浏览器手动测试。
- 发布了分支，并为筛选功能创建了 pull request。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
