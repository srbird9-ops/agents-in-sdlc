---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "了解现有的 quality-checks 技能、自定义报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不仅仅是写代码。我们已经手动验证代码是否正常运行，并使用说明文件确保代码遵循标准。但测试、lint 和持续集成 (CI) 的其他部分该如何处理？

对于这类任务，**代理技能**最合适！技能可以帮助 Copilot 正确执行这些操作。

本课程将学习如何：

- 浏览现有的 `quality-checks` 技能。
- 自定义其结果报告格式。
- 重新加载并运行此技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

代理技能会打包可复用的任务说明、可执行脚本和辅助资源，供代理按需加载。技能的基本结构是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。Markdown 文件通过 frontmatter 中的名称和描述定义技能，并介绍技能用途及适用时机。文件夹也可以包含供技能调用的脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以存储在项目的 `.github/skills` 文件夹中，作为可供团队共享和复用的仓库资源；也可以存储在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

现在查看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。description 可帮助 Copilot 判断何时调用技能。
3. 阅读说明，留意它如何引导 Copilot 执行测试和 lint 流程。
4. 注意当前技能尚未包含 **Results output formatting** 部分。

## 修改前先运行技能

技能可以通过 Copilot CLI 直接调用，也可以使用自然语言调用。现在请 Copilot 运行技能中的三项检查。

1. 返回筛选功能的 Interactive 对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告更清晰，能说明运行了什么、是否成功，以及工具报告的实际结果。现在更新技能，让它生成这样的报告！

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在运行一下更新后的技能！Copilot CLI 无需重启对话即可重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前**完全相同的提示**：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次报告比较。

## 总结和后续步骤

你已自定义并使用现有代理技能。本课程中，你：

- 浏览了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

该变更会与筛选功能一起纳入功能 PR。接下来，你将通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例供参考，并非本工作坊的额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
