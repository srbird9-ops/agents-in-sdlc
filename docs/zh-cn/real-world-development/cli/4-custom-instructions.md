---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "了解仓库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指引。[说明文件][instruction-files]不仅描述你希望编写什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将了解 Copilot 使用的说明、添加文档标准，并将其应用到代码中。

本课程将学习如何：

- 了解仓库说明和路径范围说明文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 与其他优秀开发团队一样，有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并涵盖适用时可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码变更时，注释也应保持最新。
- 应保留现有格式和 lint 指引。

通过说明文件，你可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定首选编码约定、库，以及希望在代码中添加的注释类型。你可以为整个仓库创建说明，也可以针对特定文件类型或任务创建说明。

说明文件分为两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：用于特定语言、文件类型或任务的说明。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 浏览项目中的自定义说明文件

入门项目已包含一组说明文件。先了解已有内容，再进行修改，观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指引。这些说明适用于本仓库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意这里包含 Astro 文件、Drizzle 数据层、测试等相关说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了确定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他说明文件和现有项目文件。这样可以将较大的说明拆分为更小、可复用的文件，并指向可供 Copilot 参考的示例。

## 更新说明文件以符合团队指引

现有文件已经是很好的起点，但仍有不足。我们要修改核心 `copilot-instructions.md` 文件，确保为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置约在第 35 行。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指引

Copilot CLI 会在对话开始时加载仓库说明。保存说明后，退出并恢复筛选对话，让 Copilot 在不丢失功能上下文的情况下加载新指引。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统询问，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指引：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并逐一查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明代码。

## 总结和后续步骤

你了解了 Copilot 如何获取说明文件中的上下文，并将新标准应用到了功能中。具体来说，你：

- 了解了仓库说明和路径范围说明文件如何传递给代理。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保一致地执行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
