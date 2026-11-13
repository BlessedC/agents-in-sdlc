---
title: "练习 1 - 自定义指令（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够获取这些上下文。[指令文件][instruction-files]可以提供相关指导，帮助 Copilot 理解你不只希望它完成什么，也希望它如何完成。

本练习将完成以下内容：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较提示词修改前后的效果。Copilot 云代理会异步处理 GitHub issues，因此难以实时并排展示差异。后续审查云代理创建的 pull request 时，你会看到指令文件的影响。

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

- `.github/copilot-instructions.md` 是单个指令文件，会随存储库中的**每个**请求发送给 Copilot。此文件应包含项目级信息，即与发送给 Copilot Chat 或 Copilot CLI 的大多数请求相关的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。例如，可以为 TypeScript、Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，[Copilot 还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何创建指令文件，本工作坊不作深入探讨。不过，示例项目展示了一种有代表性的方法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指导，例如项目描述、项目结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明正确和错误的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过尝试，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览此项目中的文件时，你会发现其中有针对各种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同产品界面中的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关学习路径会在适用时介绍具体方法。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于指定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 留意针对本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件底部。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以将大型指令集拆分成更小、可复用的文件，并提供可供参考的示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码规范，但目前还没有要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库级规范

接下来要编辑一次：添加一条全局规则，要求以 TSDoc 文档注释和文件头注释的形式在代码中添加文档。稍后分配 issue 时，云代理会遵循这条规则。

在编辑前，先创建并切换到一个新分支（云代理会读取 issue 目标分支中的指令，但仍应以规范的方式提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来，在其中记录编码规范，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。留意它如何记录项目编码规范，但尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建指令文件以提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置刚才添加的文档注释规则等项目级规范。

## 提交、推送并合并指令更新

云代理会从 issue 目标分支读取指令。下一练习中，你会将 issue 分配给 Copilot；届时 Copilot 会从 `main` 分支创建新分支，因此必须先将指令更新合并到 `main`，云代理才能使用这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的 pull request 并合并。下一练习中，云代理处理分配给它的 issue 时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让工作坊更贴近真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目的指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级规范。接下来几节中，云代理创建的 pull request 会体现这条规范。

下一步，将[第一个 issue 分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
