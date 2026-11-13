---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个 pull request、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选 issue。
- 添加了星级评分，在转发到浏览器的端口中审查结果，并手动合并了第一个 pull request（PR）。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建功能，并在 Interactive 模式下审查。
- 使用自定义指令引导代理、自定义现有 `quality-checks` 技能，并使用该技能运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，以检查需求、测试覆盖率、技能运行结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云代理委派的斜杠命令。

## 交付内容

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能、更新指令并将其应用于功能代码、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化本工作坊，我们将这些工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中查看，以及让 Copilot 通过 MCP 在浏览器中验证。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求审查和测试覆盖率检查汇总，然后再提交 PR。

新增测试应解决实际缺口；如果 QA 检查后认为无需添加测试，也可能是正确结果。授权合并前，请审查代码和证据；如果代码更改，请重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置并设置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令定义标准，技能描述可重复的任务，自定义代理定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 参与专注的小型更改，并使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂任务则选择能力更强的模型。

基础设施固然重要，上下文同样重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响生成结果。

## 探索更多内容

核心工作流已完成。以下 Copilot CLI 功能也值得一试：

- 使用 `/review` 请求代码审查代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务发送给 Copilot 云代理。

## 后续步骤

提高任何工具使用能力的最佳方法就是持续使用它！可以用它编写生产代码、兴趣项目，或多年来一直想做却没时间实现的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果想继续在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云代理工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理 pull request][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
