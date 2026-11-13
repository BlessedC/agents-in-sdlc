---
title: "第 0 课 - 先决条件"
description: "创建 Tailspin Toys 的个人副本，并为 Copilot CLI 工作坊准备 GitHub Codespace。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

开始 Copilot CLI 课程前，需要先准备好所需环境。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]。下一课会使用集成终端安装并运行 Copilot CLI。

本课将完成以下内容：

- 从模板创建 Tailspin Toys 项目的个人副本。
- 创建 Codespace 并确认项目已准备就绪。

## 设置实验用存储库

你将在 Tailspin Toys 项目的个人副本中进行操作。现在根据 [Tailspin Toys 模板存储库][tailspin-template]创建副本。新存储库会包含实验所需的全部文件。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。
3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建新存储库。
4. 记下创建的存储库路径（`organization-or-user-name/repository-name`），稍后会在工作坊中用到。

> [!NOTE]
> 从模板创建存储库时，系统会自动为你创建一组 GitHub issues。整个工作坊都会基于这些 issues 开展，无需自行创建。

请使用工作坊模板的全新副本。模板包含存储库指令、应用代码、测试、`quality-checks` 技能和练习所需的待办事项。如果使用旧副本，请向活动引导员确认其中包含所需文件。

## 创建 Codespace

接下来，你将使用 Codespace 完成本工作坊。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完整的编辑器，支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择 **Code**。
3. 选择 **Codespaces** 选项卡，然后选择 **Create codespace on main**。
4. 等待 Codespace 设置完成。模板会自动安装项目依赖项、Playwright Chromium 和本地数据库。
5. 如果看到 **Do you trust the authors of the files in this folder?** 提示，请选择 **Trust Folder & Continue**。
6. 在存储库根目录中打开终端并启动应用：

   ```bash
   npm run dev
   ```

7. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**，并确认 Tailspin Toys 网站可以加载。
8. 返回终端，按 <kbd>Ctrl</kbd>+<kbd>C</kbd> 停止开发服务器。

## 总结和后续步骤

环境已准备就绪！本课中，你：

- 从模板创建了 Tailspin Toys 项目的个人副本。
- 创建了 Codespace 并确认项目已准备就绪。

接下来，在 Codespace 中[安装 GitHub Copilot CLI][next-lesson]并使用 GitHub 账户完成身份验证。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
