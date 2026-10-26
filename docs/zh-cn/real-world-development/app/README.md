---
slug: zh-cn/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** 是基于 Copilot CLI 构建的桌面应用，将代理驱动的开发集中到一个专注的工作区。它提供并行代理会话、可切换的会话模式、共享画布，以及原生的 GitHub issue 和拉取请求管理功能。其中包括 **Agent Merge**，可引导拉取请求完成 rebase、处理审查反馈、修复持续集成 (CI) 问题并完成合并。

本工作坊将带你完成 Tailspin Toys 的完整工作流程：

1. 准备项目、安装应用、连接仓库，并熟悉工作区和预置的待办事项。
2. 添加范围明确的星级评分变更，在浏览器中审查，并手动合并第一个拉取请求（PR）。
3. 从筛选 issue 开始，在 **Plan** 模式中确定方案，在 **Autopilot** 模式中构建，然后在 **Interactive** 模式中审查。
4. 更新仓库说明，并将其应用于筛选功能。
5. 自定义现有的 `quality-checks` 技能，并用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在浏览器中体验筛选功能。
7. 创建质量保障（QA）自定义代理，并用它审查需求、覆盖率和验证证据。
8. 审查完整的筛选变更，并对第二个 PR 使用 Agent Merge。
9. 检查画布功能是否可用，然后使用 Database Explorer，并创建和测试由仓库数据支持的 issue 分类画布。如果缺少所需功能，请记录阻碍因素，使用只读替代方案，或继续学习总结课程。

为了让工作坊重点明确，你将创建两个 PR：先提交星级评分，再提交包含说明更新、技能更新、QA 配置和测试的筛选功能。两个 PR 都从更新后的 `main` 创建。筛选和质量工作流共用一个会话、工作树和分支，让你在探索各项工具时继续基于已有工作开展。最后的画布练习保留在其会话中，让你专注于创建并测试共享界面，而无需重复 PR 工作流。

## 课程

| 课程 | 主题 | 说明 |
|--------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 安装 Node.js 并创建自己的 Tailspin Toys 项目副本 |
| [1. 安装 Copilot app][ex1] | 设置 | 安装应用、连接项目并熟悉工作区 |
| [2. 添加星级评分：快速改进][ex2] | 首次变更 | 显示现有评分和空值回退状态，然后合并 PR 1 |
| [3. 代理模式：Plan 和 Autopilot][ex3] | 代理模式 | 根据 issue 规划功能，使用 Autopilot 构建，然后在 Interactive 模式中审查 |
| [4. 使用自定义说明引导 Copilot][ex4] | 上下文 | 浏览并更新说明，然后将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 浏览现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 通过 Customize 配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 代理][ex7] | 需求与覆盖率 | 创建并选择专用配置，然后收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查与合并 | 审查筛选功能、说明、技能、QA 配置和测试，然后对第二个 PR 使用 Agent Merge |
| [9. 探索并创建画布][ex9] | 协作 | 使用 Database Explorer，再创建并测试由仓库数据支持的 issue 分类画布 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、产出和更多资源 |

## 先决条件

参加本工作坊前，请确认你已具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 使用运行 **macOS、Linux 或 Windows** 的计算机
- [ ] 已在计算机上[安装 Git][install-git]

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所需的代理、MCP、代码审查和 Copilot CLI 功能，因此可用于完成每种工作环境的练习。

> [!NOTE]
> Copilot app 在本地计算机上运行，而不是在 codespace 中运行，因此[先决条件练习][ex0]会先指导你安装 Node.js 并创建项目副本，然后再安装应用。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。此策略与 Copilot CLI 策略相互独立。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
