---
title: "练习 3 - 自定义代理"
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

你将在自定义代理练习中了解：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前的深色主题难以阅读，因为文本与背景颜色之间的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。你将使用自定义代理生成所需功能。

## 检查无障碍自定义代理

我们已为你创建无障碍自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必需内容。

3. 接着浏览并检查下列部分：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 创建 issue 并分配任务

Mission control 是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot cloud agent、监控任务并重定向代理，还可以为进行中的会话提供更多指引。现在，我们将任务分配给 Copilot，以实现高对比度模式。

1. 转到仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![cloud agent 分配界面的截图，其中高亮显示了自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是面向特定任务和领域的专用 AI 助手。通过自定义代理，你可以将团队的专业知识和标准编码为可复用代理，引导 Copilot 更有效地完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。现在转到 [mission control][next-lesson]，了解如何监控和引导代理会话。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
