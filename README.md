# dsh-superagent-test-project

用于验证 DSH Super Agent 的测试仓库。

## 项目用途

本仓库是 [DeepSeek Harness (DSH) Super Agent](https://github.com/deepseek-ai/dsh) 的测试项目，主要用于：

- **验证 Super Agent 工作流**：测试从 Issue 创建到 PR 提交的完整自动化流程。
- **功能回归测试**：确保 Super Agent 的各项能力（代码修改、文档更新、PR 创建等）按预期运行。
- **示例参考**：作为使用 DSH Super Agent 管理项目的参考示例。

## 使用说明

### 前置条件

- 拥有 GitHub 账号，并被添加为本仓库的协作者。
- 已安装并配置 [DSH Super Agent](https://github.com/deepseek-ai/dsh)。

### 通过 Issue 触发任务

1. 在本仓库中创建一个 Issue，描述你希望 Super Agent 完成的任务。
2. 在 Issue 内容中 `@super-agent-bot923`，明确指派给 Super Agent。
3. Super Agent 将自动接收任务，创建分支、修改代码并提交 Pull Request。
4. 在 PR 中审查变更，确认无误后合并即可。

### 注意事项

- Super Agent 不会直接推送到 `main` 分支，所有变更均通过 PR 提交。
- 每个 PR 需要人工审查后手动合并。
- 如需停止正在进行的任务，请在对应的 Issue 或 PR 中留言说明。
