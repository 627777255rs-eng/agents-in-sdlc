---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 完成了多个改进用户体验和添加功能的任务：为代码添加文档、为设计团队构建相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循常规 DevOps 流程。接下来审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提供的建议，然后再允许团队其他成员审查。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此设有安全限制：

- Copilot 对存储库仅有读取权限，对用于提交代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。通过 GitHub.com 上的标准 PR 界面审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)
5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能不同；留意 TSDoc 注释、TypeScript 约定和文件注释头，它们来自练习 2 中审查的自定义指令。
6. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查部分排队。正常情况下，单一 Astro 应用的项目检查将通过。此步骤可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以在 PR 评论或代码行内评论中像提及团队成员一样提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果并非确定性，无法规定确切提示；可以请求 Copilot：

- 在每个代码文件开头添加简短的文件用途注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，描述 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档相关要求。
2. 选择 **View Session** 观察 Copilot 工作。注意 Copilot 会启动新会话来更新内容。
3. 选择 **Back to pull request** 返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿状态改为就绪，然后合并到主分支。

![将 PR 转为可供审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为游戏详情页显示相关游戏的议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完毕后返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查部分排队。正常情况下，单一 Astro 应用的项目检查将通过。此步骤可能需要几分钟。
9. **可选：**在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，可以通过云智能体迭代请求修改，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 注释（请记住，分配此任务时尚未更新自定义指令）。在 **Conversation** 选项卡中添加评论即可请求 Copilot 启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在代理控制台请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注：
   - 用于切换模式的界面组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查部分排队。正常情况下，单一 Astro 应用的项目检查将通过。此步骤可能需要几分钟。
10. **可选：**在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常且刷新页面后设置仍保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习：继续委派

云智能体适合处理真实待办事项，让你专注于其他工作。可在存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体路线。通过这些课程，你：

- **检查了存储库自带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍工作，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为代理控制台**，在无障碍会话执行期间监控并引导它。
- **审查并迭代改进云智能体的拉取请求**，通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云智能体路线已完成。其他路线可以补充你在此处学到的内容：

- 🖥️ **[VS Code 路线](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 路线](../../cli/)**：在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
