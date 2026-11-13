---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选委派给云代理的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、幕后信息和额外配置选项。之前已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在再来了解一些其他实用命令。

本课将完成以下内容：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索并行工作、worktree 和委派给云代理等可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他一些功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云代理][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及上下文在系统指令、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将填满时自动压缩内容。需要自行选择压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 可以列出模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验，可以帮助所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的可用选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出到本地 Markdown 文件。
3. 发送或发布导出内容前，请先审查内容。会话导出可能包含提示、回复和项目细节。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊内容已完成。Copilot CLI 还为大型任务提供了其他命令：

- `/fleet` 可以将独立子任务分配给多个子代理并行运行。
- `/worktree` 可以为单独的任务创建隔离的 Git worktree。
- `/delegate` 可以将任务发送给 Copilot 云代理，由它异步工作并可能创建 pull request。

这些命令是可选的，因为它们可能创建额外的 worktree 或远程任务。尝试前，请开始一项范围明确的新任务，并通过常规工作流审查结果。若想深入了解异步代理工作，可以继续学习[云代理工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以对其进行配置、共享会话并了解其幕后工作。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了并行工作、worktree 和委派给云代理等可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！现在通过[回顾所学内容][next-lesson]和后续步骤来结束本次学习。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
