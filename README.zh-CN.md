# ECA — 企业级编程智能体群

[English](README.md)

**同时适用于 ZCode 与 Claude Code。** 一个仓库、两个生态：插件自带**双清单**（`.zcode-plugin/` 为 ZCode 首选，`.claude-plugin/` 供 Claude Code 识别），任何 Git 托管的市场均可一键安装。

六个专职编程子智能体 + 一套**门禁式编排协议**。设计信条：*可靠性来自机械化的门禁与检查表，而不是智能体的临场记忆或当天状态*。ECA 要解决的问题很具体：大功能或大范围重构之后，环节被遗漏，只能等有人踩到才补。

> **状态：正式（v0.1.1）。** 终极测试通过（2026-09-30，分层计分，流程性缺陷 0 起）：智能体群以 L2 管道端到端交付本地多 GPU 推理调度器——五批次实现、573 项测试 + 真机部署验收（幂等安装/回滚演练）、7 次评审打回全部闭环、一次真实供应链修复循环（bash errexit fail-open 根因发现并回归锁定）。提示词为中文正文 + 英文术语。

## 兼容性与安装

| 宿主 | 方法 |
|---|---|
| **ZCode** | 设置 → 插件管理 → 发现 → `+` → 添加插件市场 → 粘贴 `https://github.com/weijingji/Enterprise-Coding-Agents`→ 安装 **eca**。用户级启用 = 所有工作区可用。 |
| **Claude Code** | `/plugin marketplace add https://github.com/weijingji/Enterprise-Coding-Agents` → 安装 `eca@eca`（已含 `.claude-plugin/plugin.json` 清单）。 |

两个宿主获得完全相同的六子智能体、`orchestration-protocol` 技能与 `/eca` 命令；子智能体文件格式（markdown + YAML frontmatter：`name`/`description`/`color`/`tools`）跨宿主一致。

## 组件

| 组件 | 职责 |
|---|---|
| `eca:code-architect` | 实现前的影响面（blast radius）分析 + 方案设计，按五条 agent 友好代码库原则自查。只读。 |
| `eca:code-implementer` | TDD 红-绿-重构实现 + 机械验证闭环（构建/类型/lint/测试证据齐全才可声明"完成"）。 |
| `eca:code-reviewer` | 质量评审：ISO 25010 可维护性子特性、六种危险耦合（重点语义耦合）、错误处理完整性、一致性、类型化 DoD 核对。只读。 |
| `eca:security-reviewer` | 基于 OWASP ASVS 的安全评审（默认 L1，级别可注入）。安全缺陷一票否决。只读；凭据内容永不入上下文。 |
| `eca:test-engineer` | 由影响面生成测试矩阵（边界/异常/回归），报告覆盖缺口。 |
| `eca:debugger` | 系统化调试：最小复现 → 证据链根因 → 最小修复 → 回归锁定。 |
| `orchestration-protocol`（技能） | 任务分级（L0/L1/L2）、三级交付验收（系统项目级/功能任务级/fixbug 与体验完善级，含先确认后派发的级别确认门）、类型化 DoD 清单库、领域约束注入的 dispatch 模板、重试与上报规则。 |
| `/eca` 命令 | 显式入口：`/eca [review|security|<任务>]`。 |

## 思路来源与参考文献

ECA 是综合，不是发明。每个核心机制都可追溯到公开来源：

- 架构师与评审检查表所用的**五条代码库原则**（局部性、小爆炸半径、边界完整性、可导航性、窄验证范围）—— *Agentic Codebase Principles*：<https://maintainable.software/agentic-engineering-part-2-agentic-codebase-principles/>
- **"策划模型所见"与"控制的幻觉"**——关键动作必须挂在机械验证而非提示词措辞上的依据 —— *Context Engineering for Coding Agents*（Birgitta Böckeler，Thoughtworks，发表于 Martin Fowler 站点）：<https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html>
- **给 agent 的规范要比给人的更显式、带正反例** —— *Coding guidelines for AI (and people too)*，Stack Overflow Blog：<https://stackoverflow.blog/2026/03/26/coding-guidelines-for-ai-agents-and-people-too/>
- **explore → plan → code → verify 管道骨架** —— *Best practices for Claude Code*，Anthropic：<https://code.claude.com/docs/en/best-practices>
- **可维护性评审维度** —— ISO/IEC 25010:2023 质量模型（模块性、可分析性、可修改性、可测试性、易复用性）：<https://www.iso.org/obp/ui/#iso:std:iso-iec:25010:en>
- **安全检查表与 L1–L3 分级** —— OWASP ASVS：<https://owasp.org/www-project-application-security-verification-standard/>
- **spec → plan → tasks 门禁化** —— GitHub Spec Kit（MIT）：<https://github.com/github/spec-kit>
- **可伸缩的规划深度**（小改动直通、大改动加深规划）—— BMAD-METHOD（MIT）：<https://github.com/bmad-code-org/BMAD-METHOD>
- **子智能体文件格式**（markdown + YAML frontmatter）—— VoltAgent/awesome-claude-code-subagents（MIT）：<https://github.com/VoltAgent/awesome-claude-code-subagents>
- **协议引用的流程技能**（TDD、系统化调试、完成前验证）—— obra/superpowers（需另行安装）：<https://github.com/obra/superpowers>

## 与同类插件的差异

- `superpowers` 向主智能体传授流程*技能*；ECA 用*专职子智能体*在门禁协议下运转（互补关系：ECA 协议引用 superpowers 技能）。
- `code-review` / `pr-review-toolkit` 只覆盖评审环节；ECA 管道覆盖 架构 → 实现 → 并行评审 → 修复循环。
- `feature-dev` 提供专项智能体的功能工作流；ECA 的差异化在：三级交付验收、类型化 DoD 库、只读工具硬边界、dispatch 时注入领域约束的协议。

## 验证状态（诚实披露）

- 预埋缺陷靶场（注入、硬编码凭据、时序耦合、魔法错误码、吞异常、零测试）：v0.1.1 评审能检出全部预埋类别，并正确区分**增量门禁**（新缺陷阻断）与**存量扫描**（列出移交不阻断）。
- 本地多 GPU 调度器实测（L2 管道）：五批次实现、567 测试零警告、7 次评审打回全部闭环；真机验收进行中。
- 单样本基线对照的诚实结论：小单文件场景下，普通通用智能体的单次评审可能与 ECA 检出相当；ECA 的价值在**可审计、可版本化的下限**（检查表驱动）与流程门禁，其设计主场是多模块大工程。

## 许可

MIT，见 [LICENSE](LICENSE)。本插件对上列 MIT/Apache 项目为**思路重写而非代码复制**；各引用项目遵循其自身许可。
