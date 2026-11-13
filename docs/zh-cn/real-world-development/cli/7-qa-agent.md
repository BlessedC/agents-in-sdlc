---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将 issue 要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中体验筛选功能。现在将这些能力整合到一个自定义代理中，并定义清晰的 QA 流程。

本课将完成以下内容：

- 了解自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，并根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建 pull request（PR）前，按照一致的流程检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

此前使用过的自定义项各有不同用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力来评估需求并报告发现。它们彼此配合，不会相互取代，也不需要启动另一个对话。

## 创建 QA 配置

创建功能 PR 前，先让 Copilot 创建一个可复用的 QA 配置。该配置将定义 QA 要执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 要求默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先审查其配置，确认 Copilot 捕获了预期的 QA 工作流和边界。这样可以避免配置不完整或范围过大的代理在你只想验证功能时更改代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 虽然可选，但添加后代理会有清晰的显示名称。
3. 阅读配置指令，确认 QA 会先查看需求、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建 pull request。
5. 如果生成的配置文件缺少任何职责或边界，请在继续之前要求默认代理修改。

## 根据 issue 执行 QA

Copilot CLI 会在每次启动对话时加载项目代理。创建配置后，恢复同一个筛选对话并选择 QA，这样它就能使用现有的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建代理，它可能不会显示在可用代理列表中。它仍然存在，可以通过上述命令启用。

2. 使用以下提示要求 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你添加了一个可复用的专用代理角色，并审查了它的工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，并根据筛选 issue 审查其发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来，将它们整合到功能 PR 中，并使用 Agent Merge[完成合并][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
