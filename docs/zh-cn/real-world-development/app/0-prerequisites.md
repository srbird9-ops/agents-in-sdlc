---
title: "课程 0 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：为 Tailspin Toys 项目安装 Node.js，并通过模板创建自己的仓库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，可作为 Copilot 和 GitHub 的中心枢纽。它支持快速访问 issue 和拉取请求，也可用于借助 GitHub Copilot 构建项目。本工作坊中，你将在本地使用 GitHub Copilot app 更新基于 Astro 构建的 Tailspin Toys 应用。开始前，请先确认本地已安装 Node.js，然后再安装 Copilot app。

本课程将学习如何：

- 安装 Node.js，以便在本机运行项目测试。
- 通过模板创建自己的 Tailspin Toys 项目副本。

## 安装 Node.js

多节课程会要求代理构建功能，并在本地运行 Tailspin Toys 测试套件。这需要 **[Node.js][nodejs]**，它是项目唯一需要的运行时。请安装当前的 **LTS** 版本。

所有平台上最简单的方式都是使用官方安装程序：

1. 在操作系统中使用 Windows Terminal、macOS 终端或常用工具打开终端窗口。
2. 运行以下命令，检查已安装的 Node.js 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两个命令都成功，且 Node.js 符合项目 README 和 `package.json` 中的要求，可以跳到下一节。

> [!TIP]
> 仅当尚未安装 Node 或需要更新时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于当前操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认设置。在 Windows 上，保留选中的 **Add to PATH** 选项。
7. 安装完成后，打开新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装成功：

    ```bash
    node --version
    npm --version
    ```

9. 应显示已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 中的代理也必须能够运行 `node --version` 和 `npm --version`。常用终端中运行成功，不代表应用也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开应用。如果会话中仍有任何一个命令运行失败，请继续执行[会话环境故障排查步骤][session-environment]后再继续。

> [!IMPORTANT]
> 每个工作树还需要项目依赖项及用于 E2E 检查的 Playwright Chromium。准备工作树时，请遵循 Tailspin Toys 仓库的 README，并在批准任何安装请求前先审查。

## 设置实验仓库

你将使用自己的 Tailspin Toys 项目副本。现在通过[模板仓库][template-repository]创建副本。新仓库包含实验所需的全部文件，下一课中你会将它连接到应用。

1. 在新的浏览器窗口中，转到本实验的 GitHub 仓库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验仓库页面选择 **Use this template** 按钮，再选择 **Create a new repository**，创建自己的仓库副本。

    ![模板下拉菜单中选中 Create a new repository 的 Use this template 按钮](../../../_images/app-0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新仓库。

    ![Create a new repository 表单，其中 github-samples/tailspin-toys 被设为模板，且已填写仓库名称](../../../_images/app-0-create-repository.png)

4. 记下创建的仓库路径（**organization-or-user-name/repository-name**），稍后实验中会用到。

> [!NOTE]
> 通过模板创建仓库时，系统会自动创建一组 GitHub issue 作为待办事项。整个工作坊都会使用这些 issue，无需自行创建。

请使用工作坊模板的新副本。其中包含仓库说明、应用代码、测试、`quality-checks` 技能和现有画布扩展。你将在工作坊中自定义该技能并创建 QA 代理。如果使用旧副本，请向讲师确认其中包含所需文件。

## 总结和后续步骤

准备工作已完成。本课程中，你：

- 安装了 Node.js，以便在本机构建和测试项目。
- 通过模板创建了自己的 Tailspin Toys 仓库副本。

接下来，你将[安装 GitHub Copilot app][next-lesson]、连接刚创建的仓库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [通过模板创建仓库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从-issue-启动会话
