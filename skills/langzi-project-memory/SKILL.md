---
name: langzi-project-memory
description: 管理 memory/ 中的共享项目长期记忆。主 Agent 或 Agent Team Lead 在任务开始前使用 start 按需读取，在任务结束前使用 finish 按真实变化更新；Subagent 和 teammate 只能使用 propose 返回更新建议。涉及项目进度、issue、决策、验证、风险、环境变化或 session 交接时使用。
argument-hint: "[start|propose|finish]"
arguments:
  - mode
---

# langzi-project-memory

管理项目根目录 `memory/` 中的共享项目长期记忆。本 Skill 只处理项目长期记忆的按需读取、更新提案和最终写入，不替代任务实现、代码审查或测试验证。

支持三种调用方式：

```text
/langzi-project-memory start
/langzi-project-memory propose
/langzi-project-memory finish
```

当前模式：`$mode`

---

## 执行者身份与写入权限

本节规则优先级高于各模式参考文件中的流程。

- 主 Agent 或 Agent Team Lead 可以执行 `start` 和 `finish`。
- Subagent 和 Agent Team teammate 只能执行 `propose`。
- `finish` 是唯一允许创建或修改共享 `memory/` 文件的模式。
- Subagent 和 teammate 不得直接修改 `memory/`。
- 身份无法确认时，按 Subagent 权限处理，禁止修改 `memory/`，只能输出 `Memory Delta Proposal`。
- 删除或重命名 memory 文件属于高风险操作，只有用户明确要求或 memory 结构已确认变化时才允许执行。

---

## 项目记忆信息优先级

当信息冲突时，按以下优先级处理：

```text
用户当前明确指令
> 当前 issue 与验收标准
> 已确认合同
> 当前代码事实
> memory/
> AI 推断
```

不得使用旧 memory 覆盖用户当前要求、当前 issue、已确认合同或当前代码事实；不得将未经验证的 AI 推断写入项目记忆。

---

## 通用安全规则

- 不得写入密钥、Token、密码、私有 URL。
- 不得写入临时日志、调试输出、未验证推断、未采用方案、低价值流水账或单个 Agent 的思考过程。
- 只记录真实、长期有效、适合未来 session 使用的项目状态。

---

## 模式路由规则

根据 `$mode` 只读取并执行对应的参考文件：

- `start`：读取 [references/start.md](references/start.md)
- `propose`：读取 [references/propose.md](references/propose.md)
- `finish`：读取 [references/finish.md](references/finish.md)

不得读取与当前模式无关的其他参考文件。

如果 `$mode` 不是 `start`、`propose` 或 `finish`，说明可用模式并停止，不要猜测模式。
