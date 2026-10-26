---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本工作坊中，你与 GitHub Copilot 合作完成了多个任务，以改善用户体验并添加功能。你请 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现包含高对比度和浅色模式切换的无障碍功能。现在来查看代码变更；如有需要，也可以向 Copilot 提供反馈以改进工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队其他成员启用审查之前，请先查看 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制以确保操作安全，包括：

- Copilot 仅对仓库拥有读取权限；写入权限**仅限于**它用于编写代码的分支。
- cloud agent 在 GitHub Actions 中运行，并创建单独的临时环境来处理任务。
- 任何 GitHub Actions 工作流都必须先由人工批准才能运行。
- [默认限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先在 GitHub.com 的标准 PR 界面中查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到提示 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 或内容更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成任务。可能需要几分钟，你可以先休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查变更。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。实际变更会有所不同。

    检查代码时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 确认更新看起来没问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会在 PR 的检查区域看到工作流进入队列。如果一切顺利，单个 Astro 应用的项目检查会通过。可能需要几分钟。

## 请 GitHub Copilot 修改代码

与 Copilot 合作并不是单向的。你可以像在团队 PR 中一样，在评论或代码行内评论中提及 Copilot。Copilot 会看到评论并启动新的会话来处理问题。由于结果不确定，无法规定必须提出哪些修改。可以让 Copilot：

- 在每个代码文件开头添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用结构。

1. 发表评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述建议，也可以提出其他希望添加到代码库中的文档要求。
2. 选择 **View Session**，查看 Copilot 如何处理任务。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成变更后，PR 中应该会出现新的提交。
5. 选择 **Files changed** 选项卡并审查变更。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿状态改为准备审查，然后合并到主分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 创建的 PR，处理在游戏详情页显示相关游戏的 issue。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新看起来没问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会在 PR 的检查区域看到工作流进入队列。如果一切顺利，单个 Astro 应用的项目检查会通过。可能需要几分钟。
9. **可选：**你还可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！与之前一样，你可以继续迭代并请求 Copilot 更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，这项任务在你更新自定义说明**之前**就已分配！）。与之前一样，可以在 **Conversation** 选项卡添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中通过 mission control 提出的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成任务，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式相关的 CSS 或样式变更。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新看起来没问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会在 PR 的检查区域看到工作流进入队列。如果一切顺利，单个 Astro 应用的项目检查会通过。可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常，并且在重新加载页面后仍能保留设置。

请注意，无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进机会，可以像之前一样在评论中提及 **@copilot**，要求它更新。

## 可选练习 — 继续委派

cloud agent 最适合处理真实待办事项，让你可以专注于其他工作。要养成这种习惯，可以再为仓库创建几个 issue 并分配给 Copilot。可以考虑：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作环境的练习。通过这些课程，你：

- **检查了仓库自带的自定义说明文件**，了解这些说明将如何影响 cloud agent 的输出。
- **将 issue 分配给 Copilot cloud agent**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**，实现高对比度和浅色模式切换等无障碍功能。
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍会话。
- **审查并迭代 cloud agent 创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作环境。其他工作环境可以补充本次练习的内容：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接，交付两个经过审查的变更。

在自己的仓库中，可以尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
