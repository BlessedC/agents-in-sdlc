---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些操作实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中查看每个差异、接受或拒绝单项更改，并通过后续提示引导对话、完善或扩展工作，全程无需离开编辑器。

本练习将完成以下内容：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导代理，在高对比度模式之外添加浅色模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了一个请求：为觉得默认主题太暗的用户添加浅色模式开关。无需从头开始新对话，你可以引导当前会话继续完成这项工作。

## 监控 Copilot 进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会在聊天中显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 代理工作时滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 代理提出文件编辑时，差异会直接显示在聊天中：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用要求授权（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。它应该会显示总结所做更改的最终消息。

## 引导代理添加浅色模式

高对比度模式已就绪，现在扩展同一对话，添加浅色模式开关。由于是在同一个聊天会话中继续，代理会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会接着工作，先探索已有更改，再提出浅色模式的新增内容。
3. 和高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo** 进行处理。
4. 如果代理的方向不符合预期，例如试图以你不喜欢的方式重构开关 UI，可以选择聊天输入框旁边的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“同时为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个更改的文件并查看差异。确认其中体现了自定义代理所述的无障碍模式，包括 ARIA 属性、键盘导航、语义 HTML 和本地存储。
4. 确认满意后，在源代码管理面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留现有上下文。

本课介绍了以下概念：

- 实时监控代理的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍工作发布为 pull request，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
