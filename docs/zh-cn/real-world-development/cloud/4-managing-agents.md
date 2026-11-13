---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云代理处理了三项任务，目标是改善用户体验并添加功能。云代理虽然以异步、自主的方式工作，但仍需监控这些任务。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制台中，可以查看所有创建了未关闭 pull request（PR）的代理任务。你可以查看代理执行的操作，甚至引导正在进行的会话。

本课将完成以下内容：

- 浏览代理页面，监控云代理任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

团队要求创建高对比度模式后，意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，加入这个功能也很合理。现在需要引导代理，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云代理任务

先查看分配给 Copilot 云代理的所有任务的当前状态。

1. 转到 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。这里应能看到你分配给 Copilot 的任务，包括：
    - 为代码库补充文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 的工作状态和执行方式而异。

4. 同时留意右侧显示的 pull request（PR）窗格。可以在这里查看 PR 和更改的文件，以便进一步监控。

## 引导云代理

现在已经了解正在进行的任务，可以要求 Copilot 在实现高对比度模式时同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现类似下方的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中高亮显示 Steer active session while Copilot is working 对话框的云代理任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！发送消息时，如果它正在执行某个步骤，完成该步骤后就会把新要求纳入工作流程。

如前所述，这需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面——用于监控和引导 GitHub Copilot 云代理任务的中心位置。在此任务控制台中，可以跟踪所有正在进行和已完成的任务、审查代理所做的工作，甚至重定向进行中的任务以调整范围或补充指导。

本课介绍了以下概念：

- 使用任务控制台和代理页面监控云代理任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能，接下来可以[迭代优化 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
