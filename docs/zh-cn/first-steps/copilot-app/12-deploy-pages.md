---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot 应用配置 Pages 工作流、审查其 pull request，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问入口。Copilot 可以准备工作流并指导部署，但你仍需负责审查更改并确认网站已上线。

本课将完成以下内容：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和 pull request。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还没有方便的试用方式。使用 GitHub Pages 发布这个单文件网站，就能通过公开 URL 访问它，无需添加服务器或运行时依赖项。

## 让 Copilot 部署网站

在 Copilot 应用中打开 `space-quiz` 会话，并发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz pull request；GitHub Actions 中成功运行的 Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求代理创建并合并 pull request，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才会添加 README。
2. 审查 pull request 中的更改，包括工作流的构建和部署步骤。
3. 只有必需的 pull request 检查全部通过后才合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 可以正常加载。

如果代理无法启用 Pages 或合并 pull request，请按照它提供的具体说明操作。只有部署成功且网站能够加载，才能将任务视为完成。

## 总结和后续步骤

Space Quiz 现已作为在线网站发布。你还亲自验证了部署，而不是只依赖代理的报告。继续学习[第 13 课：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
