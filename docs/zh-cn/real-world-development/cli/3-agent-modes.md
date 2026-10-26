---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定实施方案，使用 Autopilot 根据 issue 实现筛选功能，并在转发的浏览器中手动验证结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。较大的变更则需要更完善的流程。幸运的是，GitHub Copilot CLI 可以融入组织现有的工作流程，帮助我们以正确的方式构建正确的功能。本课程是多个课程中的第一课，你将遵循典型的代理驱动开发流程：从 issue 开始实现新功能，验证代码和功能行为，最终将其成功合并到项目中。

> [!NOTE]
> 后续处理该功能时，请继续使用同一对话和分支。通常，不同类型的文件会使用不同分支或 PR；为了让我们专注于核心概念，本工作坊会简化这部分流程。

本课程将学习如何：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新仓库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和发布商筛选游戏。待办 issue 描述了这项功能，但类别组合等细节需要在编码前达成一致。你将使用 Plan 模式解决这些问题，然后授权执行范围明确的实现任务。

## 背景

在开发流程中引入 AI 编码代理，并不会改变基本原则；事实上，这些原则变得更加重要！多数开发者遵循的流程大致如下：

1. 从描述待办工作的 issue 开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程与上述步骤大同小异。

遵循这套标准流程，可以确保 AI 生成的代码符合需求，并接受与手写代码相同的审核。

## 对话模式

**对话模式**决定代理具有多大的自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出变更建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，编写代码、运行测试并迭代，直到任务完成。

先在 Plan 模式中审查计划，再使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能开发前，返回 Codespace 并确认仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用箭头键高亮标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你有机会在交给 Copilot 执行之前，确定功能实现方式。对于复杂任务，花些时间制定计划总是有益的。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实施方案，从而制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答；没有唯一的正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在可以开始实现。我们使用 Autopilot，让 Copilot 自行完成工作。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照已确认的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也需要在合并前审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与 issue 及规划时的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发布商筛选，以及按计划中商定的方式组合筛选条件。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。本课程中，你：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义说明][next-lesson]了解如何更深入地生成符合已记录实践的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
