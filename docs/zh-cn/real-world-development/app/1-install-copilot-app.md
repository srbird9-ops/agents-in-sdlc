---
title: "课程 1 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的仓库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于代理驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和 CI 管道都可以直接使用。它适用于同时指挥多个代理的工作流：每个代理都在独立工作区中运行；你无需亲自处理所有工作，还可以自动执行重复任务。安装 Node.js 并准备好项目副本后，下一步是安装应用并连接仓库。

本课程将学习如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 仓库将项目添加到应用。
- 熟悉工作区，包括模板预置的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正在采用 AI 代理处理不断增加的待办事项。Copilot app 提供了统一的工作位置，可领取 issue、运行代理、审查变更并合并拉取请求。本课程将帮助你完成安装和连接，并熟悉如何发起有关项目的对话。

> [!NOTE]
> 必须拥有符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装它。应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证，并将 Tailspin Toys 仓库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程提供仓库选择，请选择通过模板创建的 Tailspin Toys 仓库。也可以跳过仓库选择，在引导完成后再添加项目。
6. 出现主题选择提示时，选择主题，然后完成引导流程。
7. 在侧边栏的 **Sessions** 下查找仓库。如果已经列出，就无需再次添加。
8. 如果尚未连接仓库，请选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记下的所有者和仓库名称。
9. 确认侧边栏中显示仓库。在本工作坊使用的布局中，`tailspin-toys` 等仓库文件夹归类在 **Sessions** 下，各仓库下面显示其会话。

## 熟悉工作区

连接项目后，花些时间熟悉工作区。应用在侧边栏中将功能组织为以下区域：

- **New**：从这里开始新的 Copilot 聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示你的拉取请求。无需离开应用即可浏览和筛选拉取请求、检查 CI 状态并进行审查。
- **Issues**：以同样的方式显示 issue。你可以浏览和筛选 issue，并从 issue 启动会话。
- **Automations**：可按计划或按需运行的已保存代理任务。适合管理待办事项、定期维护项目，或处理其他重复工作。总结课程会将其作为后续学习方向，而不会增加一项工作坊练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。你将使用此区域配置 Playwright MCP。
- **Chats**：适合提问和集思广益的轻量对话，无需单独创建分支或工作区。开始第一个聊天后，此区域才会显示。如果尚未显示，请选择 **Sessions** 旁的 **+** 开始第一个聊天。
- **Sessions**：这里按仓库文件夹分组显示代理会话。选择 `tailspin-toys` 等仓库名称可打开仓库视图；选择仓库下方的会话可回到相应对话。每个会话都在独立工作区中运行，因此你可以同时运行多个会话而不发生变更冲突。添加星级评分时，你将开始第一个会话。

完成工作坊时，你会逐步探索工作区。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作，或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找模板预置的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在来查看创建模板副本时自动生成的待办事项。

1. 在侧边栏 **Sessions** 下选择 Tailspin Toys 仓库文件夹，而不是其下方的某个会话。然后在仓库顶部导航中选择 **Issues**。
2. 按标题查找以下 issue，不要假设它们的 issue 编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个 issue 阅读详细信息。每个 issue 都可以作为代理会话的起点。完成一项快速的首次修改后，你将从筛选 issue 开始。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的仓库中的项目。要查看其他仓库中的工作项，请将那些仓库也添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是用它来了解*应用本身*，而**快速聊天**正适合这种场景。快速聊天无需创建分支或工作树即可提问或集思广益，适合无需专门会话的一次性问题。

1. 在侧边栏中选择 **Chats** 旁的 **+**，打开新聊天。如果 **Chats** 尚未显示，请选择 **Sessions** 旁的 **+** 开始第一个聊天；完成后会显示 **Chats**。
2. 询问应用自身如何处理会话：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。你会看到每个会话都在独立的 git 工作树中运行，因此可以并行运行多个代理而不发生变更冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。你了解了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中找到模板预置的待办事项。
- 使用快速聊天提出一次性问题。

接下来，你将[启动第一个代理会话][next-lesson]，并在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用代理会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
