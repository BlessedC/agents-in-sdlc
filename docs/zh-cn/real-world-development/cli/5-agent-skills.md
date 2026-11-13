---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "浏览现有 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是敲代码。前面已经手动验证代码能够运行，也使用指令文件确保代码符合规范。但测试、lint 和持续集成 (CI) 中的其他步骤又该如何处理？

对于这类任务，**代理技能**最为合适！技能可以帮助 Copilot 了解如何正确执行此类操作。

本课将完成以下内容：

- 浏览现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易阅读。

## 指令、脚本和资源

代理技能将可复用的任务指令、可执行脚本和支持资源打包在一起，由代理按需加载。技能的核心是一个同名文件夹，其中包含 Markdown 文件 `SKILL.md`。Markdown 文件包含带有名称和描述的 frontmatter、技能概述，以及调用时机的指导。文件夹中还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要包含其他文件和文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目中已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为存储库资源供团队共享和复用；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

现在浏览 Tailspin Toys 团队创建的 `quality-checks` 技能，了解它如何运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述有助于 Copilot 判断何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 执行测试和 lint 流程。
4. 留意技能中目前还没有 **Results output formatting** 部分。

## 先运行技能，再进行更改

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在让 Copilot 运行该技能中的三项检查。

1. 返回 Interactive 模式下的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

现在需要改进报告，让它说明运行了什么、是否成功，以及工具实际报告了什么。更新技能，让它生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在查看效果！无需重新启动对话，Copilot CLI 就能重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告，并与第一次的报告比较。

## 总结和后续步骤

你自定义并使用了现有的代理技能。本课中，你：

- 浏览了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行了技能。

筛选功能的 PR 也会包含此更改。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接操作网站。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
