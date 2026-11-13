---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "浏览存储库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，就要确保 Copilot 能获取相关指导。[指令文件][instruction-files]不仅能说明你希望代码完成什么，还能说明代码应如何组织。现在已经构建了筛选功能，接下来将了解 Copilot 使用的指令、添加文档规范并将其应用于代码。

本课将完成以下内容：

- 了解存储库指令和按路径限定的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码规范。
- 观察指令文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数也应说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助此功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码规范、库，甚至希望在代码中添加的注释类型。可以为整个存储库创建指令，也可以针对特定类型的文件创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是单个指令文件，会随存储库中的**每个**请求发送给 Copilot。此文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件用于针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

起始项目中已包含一组指令文件。先浏览这些文件，了解现状，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目的简要说明和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置一个 glob，以指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将大型指令集拆分成更小、可复用的文件，并提供可供参考的示例。

## 更新指令文件以符合团队规范

现有文件已经提供了很好的基础，但还有不足之处。接下来修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，应该在第 35 行附近。
2. 在此部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在每次启动对话时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何从指令文件中获取上下文，并将新的规范应用于功能代码。具体来说，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给代理。
- 更新了指令文件，确保遵循编码规范。
- 观察了指令文件对代码的影响。

接下来，将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
