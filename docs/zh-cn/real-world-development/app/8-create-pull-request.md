---
title: "课程 8 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保障（QA）配置和测试都已保存在同一分支上。现在一并审查这些内容并创建拉取请求。你已自行合并星级评分拉取请求（PR）；这次将让 **Agent Merge** 管理该流程。

> [!NOTE]
> 通常，我们会将功能、说明更新、技能更新和 QA 代理拆分为几个独立的 PR。为简化工作坊流程，这里将整个筛选和质量工作流保留在同一会话和分支中，并将所有变更放在此 PR 中。

本课程将学习如何：

- 了解 Agent Merge 及其如何自动执行合并生命周期。
- 检查完整功能 PR 和验证证据。
- 审查后再授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时仍由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动处理拉取请求合并前的剩余工作。启用后，应用会话会读取拉取请求并处理阻碍项，包括修复持续集成（CI）检查失败、回复审查意见，以及在需要时执行 rebase；只要 GitHub 允许，就会合并 PR。它会在后台运行，即使应用重启也会继续，并在 PR 合并后自动关闭。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以接手这项工作，但要允许它编辑代码和合并仍需你明确授权。授予合并权限前，请审查它可执行的操作和实际工作。

## 使用 Agent Merge 管理 PR

代码已全部完成并审查后，可以让 Agent Merge 管理 PR 流程。

1. 使用代理选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮开始流程。仅选择下拉菜单中的选项并不会启动流程。

Agent Merge 随即开始工作。它会：

- 创建包含标题和说明的拉取请求。
- 如果会话从 issue 启动，则在说明正文中引用相关 issue。
- 对目标分支执行 rebase 或处理潜在合并冲突。
- 监控 CI，确保所有检查通过。
- 监控 PR 中其他开发者或 Copilot 代码审查提供的反馈，并做出更新以解决意见。
- 可选择在所有操作成功后自动合并 PR。

让 Agent Merge 在所有检查通过后也合并 PR！

5. 授予合并权限前，先审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求代理合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟踪代理进度并解决报告的阻碍。继续之前，确认 PR 状态为 **Merged**；启用合并权限或代理正在运行，都不代表 PR 已成功合并。

> [!IMPORTANT]
> Agent Merge 不会绕过仓库保护或权限限制。继续之前，请先解决这些阻碍。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课程中，你：

- 了解了 Agent Merge 及其如何自动执行合并生命周期。
- 检查了完整功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，你将[使用现有画布并创建 issue 分类画布][next-lesson]，探索一种更丰富的方式，与代理一起检查、规划和可视化工作。

## 资源

- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
