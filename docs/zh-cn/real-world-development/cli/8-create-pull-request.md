---
title: "第 8 课 - 创建并合并功能 PR"
description: "共同审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都已保存在同一个分支中。现在一起审查这些更改并创建 pull request。之前已手动合并星级评分 pull request（PR）；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 代理拆分成几个独立的 PR。为简化本工作坊，我们在同一个对话和分支中完成了整个筛选和质量工作流，并将所有更改放入这个 PR。

本课将完成以下内容：

- 了解 Agent Merge 的用途，以及它如何自动处理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发者掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并 pull request 所需的后续工作。启用后，Copilot 会解决 PR 的阻塞项——修复失败的持续集成（CI）检查、回复审查意见，并在需要时进行变基——然后在存储库允许的情况下启用 GitHub 自动合并。

之前都是由你选择 **Merge pull request**。Agent Merge 可以代为处理，但启用之前仍应先审查工作并确认 PR 已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部生成，现在一起审查代码、创建 PR，并让 Agent Merge 负责处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认看到类似“PR #14 was squash-merged successfully.”的 Copilot 消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理 pull request。本课中，你：

- 了解了 Agent Merge 的用途，以及它如何自动处理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云代理委派。

## 资源

- [使用 Copilot CLI 管理 pull request][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
