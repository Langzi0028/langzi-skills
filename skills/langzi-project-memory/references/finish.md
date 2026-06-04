# finish 模式：按真实变化更新共享 memory

本模式由主 Agent 或 Agent Team Lead 在任务结束前执行，是唯一允许创建或修改共享 `memory/` 文件的模式。删除或重命名 memory 文件属于高风险操作，只有用户明确要求或 memory 结构已确认变化时才允许执行。

当主 Agent 或 Agent Team Lead 执行：

```text
/langzi-project-memory finish
```

执行以下流程：

1. 如果本次任务使用了 Subagent 或 Agent Team，确认所有相关 Agent 已完成并返回结果；如果未使用，则跳过此检查。
2. 如果使用了 Agent Team，Lead 必须汇总 teammate 返回的 `Memory Delta Proposal`，并以用户要求、合同、代码事实、Git diff 和验证结果为准。
3. 先判断本次任务是否产生真实、长期有效的项目状态变化；如果没有，直接说明：

   ```text
   无需更新 memory。
   ```

4. 不得无条件读取、检查或更新全部 memory 文件；只读取和更新与本次真实变化直接相关的目标文件。
5. 优先使用以下信息判断是否需要更新以及更新哪些文件：
   - `start` 阶段已经读取的相关记忆摘要
   - 当前 issue 和验收标准
   - 已确认合同文档和当前代码事实
   - 实际 Git diff
   - 实际修改文件
   - 实际验证命令和结果
   - Subagent 或 teammate 返回的 `Memory Delta Proposal`
   - 未完成事项、风险和阻塞点
6. 只根据已验证的真实变化更新 memory；不得仅根据计划、猜测或 Agent 自述将任务记录为已完成。
7. 普通任务通常只更新 `0～2` 个 memory 文件；只有 issue 完成、重大架构变化、合同变化、环境变化或多项真实状态变化时，才允许更新多个 memory 文件。
8. 不得为了保持“所有文件最新”而重复改写无变化的 memory 文件；不得将完整任务总结复制到多个 memory 文件中。
9. 不得将完整终端输出、完整 Git diff 或完整测试日志写入 memory。
10. 写入内容必须简洁、事实化、可验证、去重，并适合下一个 session 直接使用。
11. 如果需要更新的目标 memory 文件不存在：
   - 先参考 `memory/README.md` 和现有文件风格。
   - 只在确实需要记录已验证变化时创建对应文件。
   - 不得无条件批量创建所有 memory 文件。
12. 按当前 `CLAUDE.md` 的 Contract-First Development 规则检查合同边界；如果合同不变，应在摘要中说明“不涉及合同变更”或“暂未发现合同变更”。如果合同需要变化，必须先有 Draft 合同并在实现后同步为 Implemented / Changed；不得让 memory 与已确认合同漂移。

## 各文件的精简更新规则

- `memory/project-overview.md`
  - 只在项目目标、技术栈、核心模块、主要目录或整体架构实际变化时更新。
- `memory/current-progress.md`
  - 只记录总体进度变化、当前阶段变化和下一步变化。
  - 不记录每个小修改的流水账。
- `memory/active-issues.md`
  - 只在 issue 新增、完成、暂停、拆分、阻塞或验收标准变化时更新。
- `memory/decisions.md`
  - 只记录已经确认的重要技术决策。
  - 不记录讨论过程、候选方案或未采用方案。
- `memory/handoff.md`
  - 只记录下个 session 必须知道的信息。
  - 不重复完整任务总结、完整修改文件列表或完整验证日志。
- `memory/validation-log.md`
  - 只记录验证命令、结果、关键失败原因和必要备注。
  - 不复制完整终端输出。
- `memory/issue-history.md`
  - 只在 issue 确认完成后更新。
- `memory/known-problems.md`
  - 只记录未来任务仍可能遇到的 bug、风险、兼容性问题或历史坑点。
- `memory/environment.md`
  - 只在依赖、端口、环境变量、启动命令、测试命令或运行环境实际变化时更新。

## Finish 更新范围示例

```text
普通 bug 修复：
- 可能更新 `validation-log.md`
- 如果存在未来仍需注意的问题，再更新 `known-problems.md`

普通功能开发：
- 可能更新 `current-progress.md`
- 如果下个 session 需要接手，再更新 `handoff.md`

issue 完成：
- 更新 `active-issues.md`
- 更新 `issue-history.md`
- 必要时更新 `current-progress.md` 和 `handoff.md`

仅执行验证：
- 只更新 `validation-log.md`

没有长期有效变化：
- 不更新任何 memory 文件
```

## Project Memory Finish Summary 输出模板

```text
## Project Memory Finish Summary

- 已确认完成的任务事实：...
- 已检查的证据：Git diff / 修改文件 / 验证结果 / Memory Delta Proposal / 风险
- 更新的 memory 文件：...
- 未更新但需要关注的相关 memory：无 | ...
- 未完成事项：...
- 风险与阻塞：...
- 合同边界：不涉及合同变更 | ...
- 写入权限确认：由主 Agent 或 Agent Team Lead 在 finish 模式执行
```
