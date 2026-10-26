---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取相关指引。你可以通过多种方式向 Copilot 提供上下文，其中关键的一种方式是使用[说明文件][instruction-files]来指导代码生成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明及路径范围说明文件传递给 Copilot。
- 在当前说明文件的指导下生成用于筛选的第一个数据切片（发布商 helper）。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新运行相同提示，观察生成代码如何遵循新标准。
- 将说明更新和筛选切片提交到 `main`，让 Copilot 在下一课中使用更新后的指引。

> [!CAUTION]
> 生成的代码可能与部分标准不一致。Copilot 的输出具有概率性。本练习的重点是观察更新说明前后的行为趋势，而不是要求输出逐字相同。

## 说明文件

### 场景

Tailspin Toys 与其他优秀开发团队一样，有一套开发实践指南和要求，包括：

- 数据层必须始终有单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 应在每个文件开头添加一段注释，说明文件的作用。

通过使用说明文件，你可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定首选编码约定、库，以及希望在代码中添加的注释类型。你可以为整个仓库创建说明，也可以针对特定文件类型或任务创建说明。

说明文件分为两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指引。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件，例如为 TypeScript 或 Astro 语言，或创建 UI 组件和单元测试等任务提供指引。

> [!NOTE]
> 在 IDE 中工作时，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用仓库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

创建说明文件的详细讨论超出本练习的范围，但示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指引，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指引。
- 使用自然语言，并让指引清晰易懂。提供代码示例，说明代码应如何（以及不应如何）编写。

说明文件没有唯一的编写方式，正如 AI 的使用也没有唯一方式。通过实践，你会找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。浏览此项目中的文件时，你会发现其中涵盖许多任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成说明文件。不同使用界面的操作方式有所不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；本工作坊会在相关课程中介绍对应方式。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义说明文件

花些时间阅读仓库自带的说明文件：其中有一个核心 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指引。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意这里包含 Astro 文件、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于仓库根目录的 glob，用于确定说明适用哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 留意为本项目编写单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看指向其他说明文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这样可以把较大的说明拆成更小、可复用的文件，并为 Copilot 提供参考示例。（这里的路径相对于说明文件所在位置，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但还没有要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为变更创建分支

按照最佳实践，为这项工作创建分支。

1. 返回上一练习中的 codespace。
2. 在 Codespaces 编辑器（不是终端）中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，请返回 codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始全新对话。不同练习使用的模式和模型可能不同；各步骤会在适用处说明。

为了查看自定义说明的影响，先在当前说明文件下发送提示。稍后更新说明并重新发送相同提示，再比较结果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件变更。

   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件和测试。
6. 留意提议的 helper 是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发布商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意建议的代码**没有** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，即使没有明确要求，它也可能添加文档注释。如果发生这种情况也没关系；更新说明后行为的一致性改善仍然是本练习的重点。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置约在第 27 行。留意这里记录了项目的编码标准，但还没有代码文档规则，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以位于仓库级别的 `.github/copilot-instructions.md`，用于提供全局指引；也可以使用 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指引。项目级标准（例如刚添加的文档注释规则）应放在仓库级说明文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空对话内容。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 专注于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意现在建议的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已经引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续工作。

## 提交说明并推送分支

说明文件与其他仓库资产一样，使用相同的版本控制流程进行管理。现在将说明和代码推送到仓库。

1. 在 VS Code 编辑器（不是终端）中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 变更均已应用：

   ```bash
   git status
   ```

3. 暂存并提交说明变更，以及 Copilot 为 helper 基础实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建拉取请求并关联待办事项中的文档标准 issue。我们可以手动完成，也可以请 Copilot 使用已连接到项目的 GitHub 工具来完成。请它查找 issue、创建可关闭该 issue 的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置；Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习中会介绍这种连接如何工作（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的 issue，并从当前分支创建 PR，同时说明 PR 会关闭该 issue，也包含即将用于筛选工作的 publishers helper 基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 开始查找 issue 并创建 PR。
5. 当 Copilot 提示你允许它代表你执行 GitHub 操作时，请审查命令并在适当时选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请审查命令并在适当时选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下工作：

- 发送代码生成提示，观察 Copilot 在**现有**说明下生成的结果。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 重新发送相同提示，观察建议代码如何遵循新标准。
- 将说明和 helper 基础实现提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
