---
title: "课程 8 - 创建并合并功能 PR"
description: "共同审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保障（QA）配置和测试都已保存在同一分支上。现在可以一起审查这些内容并创建拉取请求。之前你手动合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，我们会将功能、说明更新、技能更新和 QA 代理拆分为几个 PR。为了简化工作坊，我们把完整的筛选和质量工作流保留在同一对话和分支中，并在同一个 PR 中提交所有变更。

本课程将学习如何：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整的功能变更和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流中，你使用 Copilot 规划、实现和验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时让开发者继续掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的后续工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复持续集成（CI）检查失败、回复审查意见，以及在需要时执行 rebase；随后在仓库允许的情况下启用 GitHub 自动合并。

此前一直由你选择 **Merge pull request** 手动合并。Agent Merge 可以接手这项工作。启用之前，请先审查变更并确认其已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成。现在一起审查变更、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 在选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入命令 `/pr create` 创建新 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。全部通过后，它会执行合并。
7. 确认看到 Copilot 提示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过所需审批、分支保护、合并队列、仓库设置或权限限制。如果遇到阻碍，请阅读报告的原因，并在仓库允许时完成经审查的手动合并。

## 总结和后续步骤

你已自动化开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课程中，你：

- 了解了 Agent Merge 及其如何自动化合并流程。
- 检查了完整的功能变更和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，用于管理上下文、模型、会话共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
