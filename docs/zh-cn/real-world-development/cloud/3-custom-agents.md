---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可针对开发工作流中的特定任务或领域创建专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面则是定义代理行为、专业能力和指令的 Markdown 提示词。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]的功能有一些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的角色，技能是工具**。

自定义代理有自己的上下文窗口，并负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；在此过程中，它可以调用 pull request 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他应用一样，应针对自己的环境和场景反复测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理时，你将了解以下内容：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力状况或偏好的影响。最近的用户反馈指出，当前暗色主题的文本与背景颜色对比度不足，导致部分用户难以阅读。为了解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义代理生成相关功能。

## 审查无障碍自定义代理

我们已经为你创建了一个无障碍自定义代理。现在查看其内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接下来浏览并审查以下内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见错误和陷阱。

## 创建并分配 issue

任务控制台是环境中所有代理的中心位置。你可以在此处将任务分配给 Copilot 云代理、监控任务，甚至重定向并补充指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建 issue 对话框。
4. 选择 **Blank issue**，创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配截图，其中高亮显示自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 就会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域设计的专用 AI 助手。借助自定义代理，可以将团队专业知识和标准整理成可复用的代理，指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义代理的定义方式。
- 将任务分配给自定义代理。

Copilot 正在后台实现高对比度模式，现在可以转到任务控制台，[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
