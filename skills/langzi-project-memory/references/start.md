# start 模式：任务开始前读取与摘要

本模式只读。不得创建、修改、删除或重命名任何 `memory/` 文件。

当主 Agent 或 Agent Team Lead 执行：

```text
/langzi-project-memory start
```

执行以下流程：

1. 根据当前 `CLAUDE.md` 中的 Task Mode 规则判断任务类型，不在此处重复完整定义。
2. 根据当前 issue、验收标准和任务涉及模块按需选择 memory，读取顺序优先参考：
   1. 当前 issue 和验收标准
   2. 当前任务涉及的模块
   3. 与任务相关的 memory 文件
3. `start` 只读取与当前任务直接相关的项目记忆；不得因为任务属于 Standard、Complex 或 Risky，就无条件读取全部 `memory/`。
4. 如果 `memory/README.md` 存在，应优先参考其中的文件职责、格式和更新约定。
5. 对 Standard、Complex、Risky 任务，优先考虑以下核心文件，但“考虑”不等于“必读”；如果文件与当前任务无关，不读取该文件：
   - `memory/project-overview.md`
   - `memory/current-progress.md`
   - `memory/active-issues.md`
   - `memory/decisions.md`
   - `memory/handoff.md`
6. 如果任务涉及运行、测试、构建、依赖或环境，还要考虑读取：
   - `memory/environment.md`
   - `memory/validation-log.md`
7. 如果任务涉及 bug、风险、兼容性或历史问题，还要考虑读取：
   - `memory/known-problems.md`
8. 如果任务需要参考已完成 issue，还要考虑读取：
   - `memory/issue-history.md`
9. 如果某个相关 memory 文件不存在，记录重要缺失情况并继续处理其他可用文件。
10. 只输出与当前任务直接相关的简洁记忆摘要，不要复制全部 memory 原文。
11. 无需枚举或解释所有未读取的 memory 文件；只说明会影响当前任务的重要缺失文件。
12. 按当前 `CLAUDE.md` 的 Contract-First Development 规则检查可能受影响的合同边界；如果暂未发现合同变化，可写“暂未发现合同变更”。

输出模板：

```text
## Project Memory Start Summary

- Task mode: Tiny | Standard | Complex | Risky
- 已读取的相关 memory：...
- 影响当前任务的重要缺失 memory：无 | ...
- 相关项目背景：...
- 当前进度：...
- 活动 issue：...
- 已确认决策：...
- 最近验证结果：...
- 阻塞点与风险：...
- Handoff 信息：...
- 可能过期或冲突的 memory：...
- 可能受影响的合同边界：...
- 本模式写入状态：未修改 memory
```
