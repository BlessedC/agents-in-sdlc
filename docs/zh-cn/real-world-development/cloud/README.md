---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你可以在 GitHub 上分配工作，云代理会在后台接手任务、探索存储库、进行更改并创建 pull request，让你可以继续处理其他事务。

本系列练习将先添加云代理要遵循的自定义指令，再分配一个 GitHub issue，让它实现相应工作。你还将审查并使用自定义代理来调整工作方式，从代理仪表板监控和引导会话，最后审查代理创建的 pull request 并迭代优化结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云代理将遵循的自定义指令 |
| [2. 云代理][ex2] | 异步代理 | 将 issue 分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代优化 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确认你具备：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为你的账户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的代理、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成所有学习路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均提供云代理；只有 Copilot Free 计划不提供。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用此功能。

> [!NOTE]
> 本学习路径不涉及 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 学习路径](../cli/)或 [VS Code 学习路径](../vscode/)。

## 开始学习

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
