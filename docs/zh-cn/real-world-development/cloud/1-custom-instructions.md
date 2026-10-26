---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取相关上下文。[说明文件][instruction-files]可以提供这些指引，让 Copilot 不仅知道你希望它做什么，还知道你希望它如何完成任务。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过仓库自定义说明及路径范围说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用修改前后的提示进行对照，因为 Copilot cloud agent 会异步处理 GitHub issue，较难实时并排展示效果。之后审查 cloud agent 创建的拉取请求时，你会看到说明文件的影响。

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

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，例如技术栈、项目概况、最佳实践，以及适用于大多数 Copilot 聊天或 CLI 请求的其他全局指引。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件，为 TypeScript 或 Astro 等语言，以及创建 UI 组件或单元测试等任务提供指引。

> [!NOTE]
> 在 IDE 中工作时，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用仓库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

创建说明文件的详细讨论超出本练习的范围，但示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 用于项目级指引，例如项目说明、项目结构和全局编码标准。
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
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于仓库根目录的 glob，用于确定说明适用哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 留意为本项目编写单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以把较大的说明拆成更小、可复用的文件，并为 Copilot 提供参考示例。（这里的路径相对于说明文件所在位置，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但还没有要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

接下来要做的唯一编辑，是添加项目级规则，要求在代码中使用 TSDoc 文档注释并添加文件级注释头。之后的练习中，cloud agent 处理你分配的 issue 时会使用这条规则。

编辑前，先创建一个工作分支。cloud agent 会从 issue 指定的分支读取说明；无论如何，最好将编辑单独提交：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置约在第 27 行。留意这里记录了项目编码标准，但还没有要求在代码中添加文档，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以位于仓库级别的 `.github/copilot-instructions.md`，用于提供全局指引；也可以使用 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指引。项目级标准（例如刚添加的文档注释规则）应放在仓库级说明文件中。

## 提交、推送并合并说明更新

cloud agent 会从 issue 目标分支读取说明。下一课将 issue 分配给 Copilot 时，它会从 `main` 创建分支，因此你的说明变更必须先合并到 `main`，cloud agent 才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后 cloud agent 处理下一课分配的 issue 时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让工作坊更贴近实际项目的处理方式。

## 总结和后续步骤

你了解了 Copilot 如何从本项目的说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的仓库级标准。之后审查 cloud agent 创建的拉取请求时，你会看到这条标准发挥作用。

接下来，将第一个 issue [分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
