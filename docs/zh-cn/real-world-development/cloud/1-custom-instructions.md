---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 路线不同，本路线不会使用前后提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时演示其影响。后续审查云智能体创建的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 界面应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过指令文件，可确保 Copilot 获得相关信息并按这些实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码约定、库以及希望包含的注释类型。可以创建适用于整个存储库的指令，也可以创建适用于特定文件类型或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会传递给 Copilot 的单个指令文件。内容应包括与大多数聊天或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的指令。可用于说明 TypeScript、Astro 等语言，也可用于创建界面组件或单元测试。

> [!NOTE]
> 在 IDE 中，这些指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

虽然完整讲解如何创建指令文件超出了本工作坊的范围，但示例项目展示了常见方法：

- 将 `copilot-instructions.md` 限定为项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门指令。
- 使用自然语言，保持指南清晰，并提供代码应当和不应当采用的示例。

创建指令文件并没有唯一正确的方法，使用 AI 也是如此。通过尝试，找到适合项目的方法。

> [!TIP]
> 所有使用 GitHub Copilot 的项目都应包含完善的指令文件。浏览本项目中的文件时，会看到它们涵盖了多种任务，包括[界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面中的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应课程会在相关处说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

阅读存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 模式，确定指令适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 查看此项目专门用于创建单元测试的指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供示例。（其中的路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件注释头。云智能体稍后处理本路线中分配的议题时会遵循这些规则。

编辑前先创建分支。云智能体会从议题指向的分支读取指令，但仍建议干净地提交更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分记录了项目编码标准，但尚未要求添加代码文档，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所见，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指南，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。存储库级文件适合放置刚添加的文档注释规则等项目级标准。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中，云智能体处理议题时会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目的工作方式。

## 总结和后续步骤

你已探索 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续课程中，你会看到云智能体创建的拉取请求如何遵循该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
