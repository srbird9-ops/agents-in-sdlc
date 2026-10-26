---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才使用代理模式构建了筛选功能。在创建拉取请求之前，应先确认功能在浏览器中正常运行。接下来，不再手动点击应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器来测试功能；之后再发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区中添加 Playwright MCP 服务器。
- 请 Copilot 使用该服务器，在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式可以使用编辑器之外的资源时，它会更加强大。Copilot 借助 Model Context Protocol (MCP) 实现这一点。MCP 是一种让代理与外部工具和服务通信的标准方式。

![示意图：代理模式的内部工作原理，以及它如何与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时访问外部工具和服务，获取最新信息（使用资源），并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理 AI 代理与外部工具（例如现有 API 或 npm 包等本地工具）的通信。每个 MCP 服务器都提供一组 AI 代理可访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 仓库的 API。AI 代理可以使用它创建仓库、更新现有仓库，以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以使用它浏览网页、填写表单并选择按钮。

还有许多 MCP 服务器可以访问不同工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，方便发现并为生态系统做贡献。

> [!CAUTION]
> 安全方面，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取你的待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次调用工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正好可用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加与 `github` 并列的 `playwright` 条目，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记表示 Playwright 会在没有可见窗口的情况下运行浏览器；codespace 中没有桌面环境，因此必须使用此选项。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 执行端到端测试，因此 Playwright 所需的浏览器通常已安装。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 第一次需要服务器工具时，VS Code 会启动服务器，并询问你是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请检查配置并选择信任，使服务器能够启动。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次使用相关工具时启动服务器并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]允许 Copilot 驱动真实浏览器。你无需手动点击应用来检查工作，代理可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在后台根据网页的[无障碍树][playwright-mcp-server]工作，而不是依赖截图。因此，代理会依据结构化且有标签的元素（按钮、链接、列表项）进行推理，方式与辅助技术类似。这样，快速的功能检查也能顺便进行简单的无障碍检查。

连接服务器并启动应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐步测试，然后报告观察结果。根据 issue 中的验收标准审阅报告；如果发现问题，请继续提问或让 Copilot 修复代码，然后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果你已停止开发服务器，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 的变更。
2. 选择 **+** 图标暂存变更。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示 Copilot 所做变更的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到仓库。

## 创建拉取请求

创建拉取请求的方法有多种，例如使用 github.com 或 GitHub 命令行界面（CLI）。但既然已经在使用 GitHub Copilot，不妨让它代为创建 PR！它可以查找相关 issue 并将 PR 与该 issue 关联。

1. 打开 Copilot Chat 面板并选择 **New Chat** 开始新会话。
2. 确认在代理下拉列表中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据提示选择 **Continue**，允许 Copilot 执行查找信息和操作所需任务。第一次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按照提示授权即可。
5. 观察 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 提供的链接以审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 使用服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，你将[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
