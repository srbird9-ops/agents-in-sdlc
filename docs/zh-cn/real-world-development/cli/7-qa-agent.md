---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，你将通过自定义代理整合这些能力，并定义一致的 QA 流程。

本课程将学习如何：

- 了解自定义代理如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保障配置。
- 选择 QA 代理，根据筛选 issue 审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的专用 Copilot 版本。配置文件描述代理的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义功能各有用途：仓库说明描述团队标准；`quality-checks` 技能打包可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会替代这些功能，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该文件会定义 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先检查其配置，确认 Copilot 已记录预期 QA 流程和边界。这样可以避免配置不完整或权限过宽的代理在你只想验证功能时修改代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但提供它能让代理显示名称更清楚。
3. 阅读配置说明，确认 QA 会先检查需求、遵循仓库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、在更改实现代码前征求同意，并且不会提交变更或创建拉取请求。
5. 如果配置遗漏了这些职责或边界，请默认代理修改后再继续。

## 根据 issue 运行 QA

创建配置文件后，Copilot CLI 会在对话开始时加载项目代理。恢复同一个筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建代理，它可能暂时不会显示在可用代理列表中。不过，你仍然可以使用上述命令激活它。

2. 使用以下提示，请 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已在工作流程中添加可复用的专家角色，并审查了它的工作。本课程中，你：

- 了解了自定义代理如何使用说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保障配置。
- 选择 QA 代理，根据筛选 issue 审查结果。

现在，你已有实现、技能更新、QA 配置、测试和验证报告，可以开始审查。接下来，你将[汇总这些变更并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
