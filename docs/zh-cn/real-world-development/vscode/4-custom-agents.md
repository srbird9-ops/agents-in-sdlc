---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是一种专用 AI 助手，可针对开发工作流中的特定任务或领域进行定制。通过在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，你可以提供专门的指令、最佳实践、编码模式和领域知识，引导 Copilot 更有效地执行特定类型的工作。团队可以把专业经验编码为可复用代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少设置 `name` 和 `description` 的 YAML frontmatter，后续的 Markdown 提示则定义代理的行为、专长和指令。

### 自定义代理与代理技能的比较

自定义代理与[代理技能][agent-skills-concept]在概念上有一些重合。两者主要使用 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理有自己的上下文窗口，负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或运行、管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方法。与 AI 中的其他做法一样，请在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前的深色主题难以阅读，因为文本与背景颜色之间的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。你将使用自定义代理生成所需功能。

本练习将学习如何：

- 审查现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 检查无障碍自定义代理

我们已为你创建无障碍自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，为无障碍工作从 `main` 创建新分支（这样可将练习 3 的筛选 PR 单独保留）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必需内容。

3. 接着浏览并检查下列部分：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理，即可让聊天会话使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，请返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始全新对话。不同练习使用的模式和模型可能不同；各步骤会在适用处说明。

1. 如果尚未选择，请在 Chat 视图的代理下拉列表中选择 **Agent**。

   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（当前活动代理名称默认显示为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 将开始工作，探索代码库、提出变更并编辑项目文件。每项编辑都会在聊天中以内联方式显示，包括文件路径和供你审查的差异。
6. 随着编辑进行，**Files changed** 指示器会更新，显示代理修改过的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改仓库文件，例如 Astro 组件、CSS 和相关测试。

下一课中，你将审查并引导正在进行的工作。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是面向特定任务和领域的专用 AI 助手。通过自定义代理，你可以将团队的专业知识和标准编码为可复用代理，引导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理的工作][next-lesson]，审查进行中的变更，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
