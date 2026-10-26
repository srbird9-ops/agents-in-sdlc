---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

技术债务几乎是所有组织都会遇到的问题。它可能是尚未解决的安全问题、需要更新的旧代码，或因一直没有时间实现而积压的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。代理完成工作后，会创建草稿 PR，供开发者审查。这样可以把繁琐任务交给代理，加快开发进程，并让开发者专注于更重要的工作。

本课程将学习如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 了解 issue 范围明确的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇创建网站的承包商没有为代码添加文档。团队想先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望让用户更容易发现游戏。他们希望每款游戏的详情页都显示相关游戏，即同类别的其他游戏。暂时不需要精致的实现，只要能用于验收测试用户体验即可。这项工作目前阻碍了进展，但优先级低于其他 issue。

这类任务很容易被搁置，因此很适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者继续做其他工作，之后再回来审查并确认结果符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人工开发者一样在后台执行任务。你可以通过多种方式委派任务，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话运行期间或完成后查看它的过程。建议方案准备好审查时，Copilot 会在拉取请求中提醒你。

## 明确界定任务范围的重要性

GitHub Copilot 并没有魔法，你仍然需要思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot cloud agent 明确界定任务范围][cloud-agent-best-practices]很重要。请把它当作 AI 结对程序员：分阶段工作、学习、尝试并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本仓库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已经完成自定义说明的练习，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。在把任务分配给 Copilot cloud agent 之前，先只读浏览仓库中已有的说明文件，之后便能留意这些说明带来的影响。

在 GitHub 网页界面或正在运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明限定在 `**/*.test.ts` 文件。

下一部分将 **Code lacks documentation** issue 分配给 cloud agent 时，留意生成的拉取请求中的 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自你查看的说明文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要设置专门的环境并运行一些脚本，确保环境状态正常。将任务分配给 Copilot 时也是如此；它会以类似软件工程师的方式执行任务。

cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建专用的设置工作流来定制环境，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中。工作流会在代理开始工作前运行，使它能够访问所需的开发工具和依赖项。实验已预先配置此工作流，以便流程顺利进行，并让你有机会了解此功能。它会为单个 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充种子数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流文件相似，但有几个要点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。此作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 留意 `workflow_dispatch` 触发器，它允许你在仓库的 **Actions** 选项卡中手动运行工作流。这便于确认工作流能否正常运行，而不必等待 Copilot 触发它。

## 添加文档

所有人都知道文档的重要性，但大多数项目的信息都已经过时，或完全缺少文档。这类技术债务往往得不到处理，会降低开发效率，也会增加维护代码库或引入新开发者的难度。Copilot 擅长创建文档，因此很适合将这个 issue 分配给 Copilot cloud agent。它会在后台生成所需文档。之后的练习中，你会回来审查它的工作。

1. 在新的浏览器选项卡中转到 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定合适的处理方案。

13. 审查待办列表和即将完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、理解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以继续处理其他工作。我们将利用这个特点：再给 Copilot cloud agent 分配一项任务，然后把注意力转向为应用编写代码。

## 为游戏详情页添加相关游戏区域

如前所述，GitHub Copilot cloud agent 的优势之一是可以拆分工作，让你专注处理一部分任务，同时让它处理另一部分。为设计团队添加相关游戏区域可能不需要太多时间，但这段时间也可以用于其他任务。现在将它分配给 Copilot cloud agent！

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，issue 中的第一条评论会出现 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二个请求！Copilot cloud agent 的工作方式与软件工程师类似，你无需一直监控，而应在任务完成后进行审查。现在将注意力转向创建和使用自定义代理。

## 总结和后续步骤

本课程介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以通过 cloud agent 将 issue 分配给 Copilot，让它异步处理任务。你可以用 Copilot 解决技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 了解 issue 范围明确的重要性。
- 将 issue 分配给 Copilot。

cloud agent 正在后台处理任务。现在可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
