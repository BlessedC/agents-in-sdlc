---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是敲代码。还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 的内置功能，使用更多工具和服务。

本课将完成以下内容：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让代理操作浏览器，体验筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，仍需实际操作 UI。你希望允许 Copilot 像用户一样操作正在开发的网站，进一步自动验证更改是否符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

MCP 工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

目前有一些广受欢迎的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issues 和 pull request 的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目的其他依赖项一样对待 MCP 服务器。使用前，请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入服务器名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前已手动确认功能符合预期。现在使用 Copilot 中配置好的 Playwright MCP 服务器来完成相同操作！

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确告诉 Copilot 使用 MCP 服务器，它会自行判断。但既然已经知道要使用哪个工具，明确指出也有好处！这样可以提高结果的一致性，还能节省一些 token。

2. 观察 Copilot 如何在浏览器中执行各个步骤，以确认功能是否正常。
3. 阅读报告，确认一切符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后提供报告。

## 总结和后续步骤

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器在真实浏览器中体验了功能！本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了它。
- 让代理操作浏览器，体验筛选功能。

接下来，创建一个[自定义 QA 代理][next-lesson]，将技能和浏览器工具整合到专门角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
