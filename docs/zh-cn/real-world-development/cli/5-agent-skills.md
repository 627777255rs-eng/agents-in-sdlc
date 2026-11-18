---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

开发工作不只是写代码。我们已经手动验证了代码是否正常运行，也使用指令文件确保代码符合标准。接下来还要考虑测试、lint 检查以及持续集成 (CI) 中的其他环节。

**智能体技能**非常适合这类任务！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课程将介绍如何：

- 探索现有的 `quality-checks` 技能。
- 自定义技能的结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 检查和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和辅助资源打包在一起，由智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件包含说明技能用途的 `name` 和 `description` frontmatter，以及技能概述和调用时机等指南。技能文件夹还可以包含供技能使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以存放在项目的 `.github/skills` 文件夹中，作为存储库资源供团队共享和复用；也可以存放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在来看看 Tailspin Toys 团队用于运行单元测试、lint 检查和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。`description` 会帮助 Copilot 判断何时调用技能。
3. 阅读指令，了解它们如何引导 Copilot 完成测试和 lint 检查。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

你可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在请 Copilot 运行该技能中的三项检查。

1. 返回 Interactive 模式下的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

现在改进报告，让它说明运行了什么、是否成功，以及工具实际报告了什么。更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看它的效果。无需重启对话，Copilot CLI 就可以重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次的报告比较。

## 总结和后续步骤

你已自定义并使用了现有智能体技能。本课程中，你：

- 探索了用于运行单元测试、lint 检查和类型检查的 `quality-checks` 技能。
- 自定义了技能的结果格式。
- 重新加载并运行了技能。

这项更改会与筛选功能一起纳入功能 PR。接下来，你将通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
