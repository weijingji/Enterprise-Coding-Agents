---
description: ECA 企业级编程智能体群入口——按编排协议执行当前任务
argument-hint: "[review|security|<任务描述>]"
---
加载 orchestration-protocol 技能后执行：

- 无参数或任务描述：对当前任务判级（L0/L1/L2）→ 按"交付验收分级"确认级别（询问用户）→ 走对应管道；L2 停在用户确认门。
- 参数为 review：只派 code-reviewer（只读），按 dispatch 模板注入领域约束。
- 参数为 security：只派 security-reviewer（只读），询问用户 ASVS 级别（默认 L1）。

任何"完成"结论必须附机械验证证据（门禁规则 2）。
