---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，为代码生成提供指导。

本练习将完成以下内容：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 在当前指令的指导下，为筛选功能生成第一个数据切片（发布方辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。
- 再次运行相同提示，观察生成的代码如何遵循新规范。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有自定义规范。Copilot 具有概率性，并非确定性工具。本练习的目标是观察更新指令后行为变化的**趋势**，而不是要求输出逐字一致。

## 指令文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码规范、库，甚至希望在代码中添加的注释类型。可以为整个存储库创建指令，也可以针对特定类型的文件或任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随存储库中的**每个**请求发送给 Copilot。此文件应包含项目级信息，即与发送给 Copilot 的大多数请求相关的上下文。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，[Copilot 还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

## 浏览本项目中的自定义指令文件

起始项目中已包含一组指令文件。在更改之前，先浏览已有内容并了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目的简要说明和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置一个 glob，以指定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
6. 留意针对本项目单元测试的具体指令。
7. 最后，打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型指令集拆分成更小、可复用的文件，并提供可供参考的示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码规范，但目前还没有要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件头注释的规则。

## 为更改创建分支

接下来遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中打开的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始新的对话。不同练习所需的模式和模型可能不同，相关步骤会在适用时说明。

要查看自定义指令的效果，先在现有指令下发送提示。稍后更新指令文件后，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在聊天视图的代理下拉菜单中选择 **Agent**，以便 Copilot 编辑文件。

   ![聊天视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发布方数组。这来自 `.github/instructions/drizzle.instructions.md` 中针对数据层的规范（该文件适用于 `src/lib/*.ts`）。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级头注释。

> [!CAUTION]
> Copilot 具有概率性，有时即使没有相应要求，也可能添加文档注释。如果出现这种情况也没关系，本练习的重点仍是观察更新指令后行为的一致性改进。

## 添加新的存储库级规范

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来，在其中记录编码规范，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。留意它如何记录项目编码规范，但还没有代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件以提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置刚才添加的文档注释规则等项目级规范。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在建议的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用到真实代码中；下一练习将以此为基础继续开发。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理流程进行管理。现在将分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并 pull request

分支已推送，现在应创建 pull request 并关联待办事项中的文档规范 issue。也可以手动完成，但 GitHub 工具已连接到项目，可以让 Copilot 代劳：要求它查找 issue、创建可关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已配置好 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，说明指令更新和新的发布方辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. 创建 PR 后，要求 Copilot 合并 PR 并将本地分支切回 `main`，可以使用以下提示：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许它代表你执行 GitHub 操作和运行 shell 命令，请审查命令，并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并 pull request！

## 总结和后续步骤

你了解了 Copilot 如何从指令文件中获取项目上下文，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 在**现有**指令下的输出。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。
- 再次运行相同提示，观察建议的代码如何遵循新规范。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，使用[代理模式][next-lesson]让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
