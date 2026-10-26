---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出变更、编辑文件并运行命令。由于这些工作是在你的计算机上实时进行的，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以直接在编辑器中审查每个差异、接受或拒绝单项变更，并通过后续提示引导对话，以改进或扩展工作。

本练习将学习如何：

- 实时监控无障碍代理的工作。
- 审查并接受提议的文件编辑。
- 在会话进行期间引导代理，添加浅色模式切换并保留高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。任务进行期间，设计团队又提出一个要求：他们还希望添加浅色模式切换，供觉得默认主题太暗的用户使用。与其从头开始新的对话，不如引导现有会话扩展任务。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每一步，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中以内联方式显示差异和 **Keep**/**Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑，以及代理请求运行的终端命令。
3. 代理提出文件编辑后，差异会以内联方式显示：
   - 选择 **Keep** 接受变更。
   - 选择 **Undo** 撤销变更，让代理重新尝试。
4. 你还可以打开聊天顶部的 **Files changed** 指示器，查看此会话期间代理修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式的实现。完成后，它会发送最终消息，总结所做的变更。

## 引导代理添加浅色模式

高对比度模式完成后，你将在同一对话中扩展工作，添加浅色模式切换。由于继续使用当前聊天会话，代理会保留之前工作的上下文，包括修改过的组件、使用的样式方式以及设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会检查已有变更，并继续提出添加浅色模式的修改。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果代理的实现方向不符合你的预期（例如，它打算重构切换界面），可以选择聊天输入框旁的 **Stop** 按钮停止代理，然后发送说明更清楚的后续消息。

> [!TIP]
> 引导正在执行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS，使用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换控件，把浅色模式整合到现有设置面板中”），或扩大范围（“为两个切换控件添加键盘快捷键”）。

## 审查工作集

提交变更前，快速审查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认实现符合自定义代理中的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML，以及通过本地存储保留设置。
4. 确认满意后，在 Source Control 面板中暂存并提交变更。下一课中你会发布这些变更。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话，审查每项文件编辑、接受或拒绝变更，并通过后续提示扩展工作，同时保留上下文。

本课程中，你了解了：

- 实时监控代理工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示引导正在执行的会话。

在[下一课][next-lesson]中，你会将无障碍功能作为拉取请求发布，并审查本实验中本地自定义代理生成的变更。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
