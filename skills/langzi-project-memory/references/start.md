# start 模式：任务开始前读取与摘要

本模式只读。不得创建、修改、删除或重命名任何 `memory/` 文件。

当主 Agent 或 Agent Team Lead 执行：

```text
/langzi-project-memory start
```

执行以下流程：

1. 根据当前 `CLAUDE.md` 中的 Task Mode 规则判断任务类型，不在此处重复完整定义。
2. `start` 必须按需读取 memory；不得在启动后自动读取整个 `memory/` 目录，也不得为了“全面了解项目”全量读取所有 memory 文件。
3. 默认只读取一个索引文件作为入口：
   1. 如果 `memory/README.md` 存在，优先读取它。
   2. 如果 `memory/README.md` 不存在但 `memory/index.md` 存在，读取 `memory/index.md`。
   3. 如果两个索引文件都不存在，不得退化为全量读取；只能根据文件名、当前任务类型和必要的搜索结果选择候选文件。
4. 根据用户问题类型、当前 issue / 验收标准、任务涉及模块，以及索引中对 memory 文件职责 / 定义的描述，先选择最相关的 `1～2` 个 memory 文件读取。
5. 只有在读取索引和首批 `1～2` 个相关 memory 后，经过明确分析仍不足以回答当前任务，才允许继续读取第 `3` 个及更多 memory 文件；继续读取时应说明需要扩展读取范围的原因。
6. 对超过 `300` 行的 memory 文件，禁止整文件读取；必须先用搜索 / grep 定位关键词、标题或相关段落，再只读取命中的相关片段。
7. 对 Standard、Complex、Risky 任务，优先考虑以下核心文件，但“考虑”不等于“必读”；只能在它们与当前问题类型和索引定义匹配时读取：
   - `memory/project-overview.md`
   - `memory/current-progress.md`
   - `memory/active-issues.md`
   - `memory/decisions.md`
   - `memory/handoff.md`
8. 如果任务涉及运行、测试、构建、依赖或环境，再考虑读取：
   - `memory/environment.md`
   - `memory/validation-log.md`
9. 如果任务涉及 bug、风险、兼容性或历史问题，再考虑读取：
   - `memory/known-problems.md`
10. 如果任务需要参考已完成 issue，再考虑读取：
   - `memory/issue-history.md`
11. 如果某个相关 memory 文件不存在，记录重要缺失情况并继续处理其他可用文件。
12. 只输出与当前任务直接相关的简洁记忆摘要，不要复制全部 memory 原文。
13. 无需枚举或解释所有未读取的 memory 文件；只说明会影响当前任务的重要缺失文件。
14. 按当前 `CLAUDE.md` 的 Contract-First Development 规则检查可能受影响的合同边界；如果暂未发现合同变化，可写“暂未发现合同变更”。

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
