---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查变更，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次修改了。我们先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示它。你将请代理展示评分、审查变更，并将其作为第一个拉取请求合并。

本课程将学习如何：

- 在功能分支上开始一次专注的 Copilot 对话。
- 请代理对项目进行小幅修改。
- 使用 `/diff` 审查变更。
- 在转发的浏览器中运行应用并确认变更。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，该评分已显示在游戏详情页上。不过，主页上的游戏卡片只显示标题、类别、发布商和说明。作为热身练习，你将请代理在每张卡片上显示现有评分。这项修改范围小、独立性强，非常适合首次使用会话。

## 对话的组成部分

**对话**是你在 Copilot CLI 中处理任务的空间。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的仓库和 Git 分支，而不会创建专用工作树。保存的对话让你之后可以回到同一讨论，而文件和分支仍是磁盘上的常规 Git 状态。

对话中有三类内容：你的提示和代理的回复、代理探索和编辑文件时的工具活动，以及你可以通过 `/diff` 检查的变更。

## 开始对话并请求修改

我们先开始一个新对话，着手实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保已开始新会话。
5. 使用以下提示请求修改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并编写所需代码。你已经使用 Copilot CLI 添加了一项新功能！

## 审查差异

所有 AI 生成的变更都应在合并前审查，即使变更很小也不例外。我们在 Copilot CLI 中直接检查这些变更。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而非确定性，因此实际代码可能不同。请检查行为，不要期待实现完全一致。

## 检查变更

不能只看代码就假设它能够正常工作。我们可以请 Copilot 启动网站，再通过 Codespaces 转发的浏览器检查更新后的界面。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五星制评分。
4. 返回 Copilot，并请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已经完成！现在创建拉取请求（PR），将新代码合并到项目中。

1. 请默认代理提交变更：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查已更改文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个变更！具体来说，你：

- 在功能分支上开始了一次专注的 Copilot 对话。
- 指示代理对游戏卡片进行小幅修改。
- 使用 `/diff` 审查变更。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选 issue 开始，使用 Plan 和 Autopilot 模式][next-lesson]来构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
