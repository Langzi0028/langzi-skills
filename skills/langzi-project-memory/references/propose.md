# propose 模式：Subagent 只生成 Memory Delta Proposal

本模式只生成更新建议。不得直接创建、修改、删除或重命名任何 `memory/` 文件。

当 Subagent 或 Agent Team teammate 执行：

```text
/langzi-project-memory propose
```

执行以下流程：

1. 仅根据该 Agent 实际完成的工作、实际修改内容和实际验证结果，生成结构化 `Memory Delta Proposal`。
2. 不得将未验证推断、未采用方案、计划、临时过程或个人思考作为项目事实提交。
3. 如果身份无法确认，也按本模式处理，只输出提案。
4. `Memory Delta Proposal` 只输出存在真实变化的条目，不再强制输出所有值为 `None` 的字段。
5. 如果没有任何值得长期保存的变化，只输出：

```text
## Memory Delta Proposal

无需更新 memory。
```

如存在真实变化，按需使用以下条目：

```text
## Memory Delta Proposal

- project-overview:
  - 项目结构、架构或核心模块变化

- current-progress:
  - 建议新增或修改的项目进度

- active-issues:
  - issue 状态变化、阻塞点、验收标准或下一步

- decisions:
  - 新增或变化的技术决策

- validation-log:
  - 执行的验证命令、结果和失败原因

- issue-history:
  - 已完成 issue 的摘要、涉及文件、验证结果和遗留问题

- known-problems:
  - 新发现、已修复或仍存在的问题

- environment:
  - 环境、依赖、命令、端口或变量变化

- handoff:
  - 下个 session 必须知道的信息
```
