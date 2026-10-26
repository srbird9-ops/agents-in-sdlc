---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 承担了三个任务，以改善用户体验并添加功能。cloud agent 虽然可以异步自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心，你可以查看所有拥有开放拉取请求（PR）的代理任务、探索执行过的操作，甚至引导正在进行的会话。

本课程将学习如何：

- 浏览代理页面，监控 cloud agent 任务。
- 引导正在运行的会话，要求添加其他功能。

## 场景

将任务分配给代理来创建高对比度模式后，团队意识到也可以趁此机会添加浅色模式。既然正在更新网站样式并添加切换功能，将浅色模式一并实现就很合理。你将引导代理，同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务当前状态。

1. 转到 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，其中可能包括：
   - 从仓库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 工作进度和采用的方法而异。

4. 另请留意右侧显示的 PR 面板。你可以在此查看 PR 和已更改文件，以便进一步监控。

## 引导 cloud agent

了解当前任务后，在 Copilot 实现高对比度模式期间，我们也来请它添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 采用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始工作。出现类似下方的消息时，表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面上 cloud agent 任务的截图，其中突出显示了 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！收到消息时，它可能正在执行某个步骤；完成该步骤后，它会将新要求纳入工作流程。

和之前一样，这需要几分钟。你可以趁此机会停下来回顾目前学到和探索的内容。

## 总结和后续步骤

本课程介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心控制台。你可以跟踪所有正在进行和已完成的任务、审查执行中的工作，甚至重定向进行中的任务以调整范围或提供更多指引。

你了解了以下内容：

- 使用 mission control 和代理页面监控 cloud agent 任务。
- 重定向进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，查看它创建的[拉取请求并迭代结果][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
