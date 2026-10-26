---
title: "课程 10 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到预置的筛选 issue。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现功能，并在 Interactive 模式中审查。
- 使用自定义说明引导代理，随后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中体验筛选功能。
- 创建并选择质量保障（QA）自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整筛选变更，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云端委派的斜杠命令。

## 你完成的工作

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到打开其 PR，你始终使用同一对话和分支。为了简化工作坊，我们将这些工作放在同一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 在浏览器中体验。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 汇总这些结果，并在 PR 创建前检查需求和测试覆盖率。

新增测试应弥补实际缺口；如果 QA 运行后确认无需新增测试，也可能是正确结果。授权合并之前，请先审查代码和证据；变更后，请更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义功能，并根据团队需求进行调整。说明定义标准，技能描述可重复执行的任务，自定义代理定义专家角色，MCP 服务器连接外部工具。请审查实际变更和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先思考方案，使用 **Interactive** 处理需要保持沟通的专注任务，使用 **Autopilot** 处理范围明确的任务。日常修改可以选择更快的模型，复杂任务则可选择能力更强的模型。

上下文仍与基础设施同样重要。明确说明想构建什么、为什么构建，以及如何实现，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 CLI 功能：

- 使用 `/review` 请代码审查代理分析变更。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

要提升对任何工具的熟练度，最好的方式就是持续使用它！将它用于生产代码、业余项目，或你多年来一直想构建的小应用。与团队分享经验，也向其他人学习。最后，别忘了探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，请浏览 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
