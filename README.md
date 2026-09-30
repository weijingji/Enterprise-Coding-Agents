# ECA — Enterprise Coding Agents

[中文说明](README.zh-CN.md)

**Works in ZCode and Claude Code.** One repo, both ecosystems: the plugin ships **two manifests** (`.zcode-plugin/` preferred by ZCode, `.claude-plugin/` read by Claude Code) and installs from any Git-hosted marketplace.

A six-role coding agent group plus a gate-based **orchestration protocol**. The design premise: *reliability comes from mechanical gates and checklists, not from an agent's memory or mood on the day*. ECA exists to prevent the classic failure mode where a large feature or a wide refactor leaves loose ends that only get fixed when someone stumbles into them.

> **Status: beta / experimental.** Validated on a planted-defect acceptance project (see below). Prompts are written Chinese-first with English technical terms. Multi-GPU scheduler field trial (567 tests, 7 review rejections closed) passed its code phases; hardware acceptance is in progress.

## Compatibility & install

| Host | How |
|---|---|
| **ZCode** | Settings → Plugin Management → Discover → `+` → Add marketplace → paste `https://github.com/weijingji/Enterprise-Coding-Agents` → install **eca**. User-scope enable = available in every workspace. |
| **Claude Code** | `/plugin marketplace add https://github.com/weijingji/Enterprise-Coding-Agents` → install `eca@eca`. The `.claude-plugin/plugin.json` manifest is included. |

Both hosts get the same six subagents, the `orchestration-protocol` skill, and the `/eca` command. Subagent file format (markdown + YAML frontmatter: `name` / `description` / `color` / `tools`) is identical across hosts.

## What's inside

| Component | Role |
|---|---|
| `eca:code-architect` | Pre-implementation blast-radius analysis + design, checked against five agent-friendly codebase principles. Read-only. |
| `eca:code-implementer` | TDD red-green-refactor implementation with a mechanical verification loop (build/type/lint/test evidence required to claim "done"). |
| `eca:code-reviewer` | Maintainability review: ISO 25010 sub-characteristics, six dangerous coupling forms (semantic coupling emphasized), error-handling integrity, consistency, typed DoD check. Read-only. |
| `eca:security-reviewer` | OWASP ASVS-based security review (default L1, level injectable). Security defects are a hard veto. Read-only; credentials never enter context. |
| `eca:test-engineer` | Test matrix (boundary / exception / regression) derived from the impact list, coverage-gap reporting. |
| `eca:debugger` | Systematic debugging: minimal repro → evidence-backed root cause → minimal fix → regression lock. |
| `orchestration-protocol` (skill) | Task grading (L0/L1/L2), three-level delivery acceptance (system-project / feature-task / bugfix-polish with a confirm-before-dispatch gate), typed DoD checklist library, dispatch template for injecting domain constraints, retry/escalation rules. |
| `/eca` command | Explicit entry point: `/eca [review\|security\|<task>]`. |

## Design sources & references

ECA is a synthesis, not an invention. Every core mechanism traces to a public source:

- **Five codebase principles** used by the architect and reviewer checklists (locality, small blast radius, boundary integrity, navigability, narrow test scope) — *Agentic Codebase Principles*: <https://maintainable.software/agentic-engineering-part-2-agentic-codebase-principles/>
- **"Curate what the model sees" and the illusion of control** — the reason critical actions hang on mechanical verification instead of prompt wording — *Context Engineering for Coding Agents* (Birgitta Böckeler, Thoughtworks, on Martin Fowler's site): <https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html>
- **Explicit, example-bearing conventions for agents** — *Coding guidelines for AI (and people too)*, Stack Overflow Blog: <https://stackoverflow.blog/2026/03/26/coding-guidelines-for-ai-agents-and-people-too/>
- **Explore → plan → code → verify pipeline skeleton** — *Best practices for Claude Code*, Anthropic: <https://code.claude.com/docs/en/best-practices>
- **Maintainability review dimensions** — ISO/IEC 25010:2023 quality model (modularity, analysability, modifiability, testability, reusability): <https://www.iso.org/obp/ui/#iso:std:iso-iec:25010:en>
- **Security checklist and L1–L3 levels** — OWASP Application Security Verification Standard: <https://owasp.org/www-project-application-security-verification-standard/>
- **Spec → plan → tasks gating** — GitHub Spec Kit (MIT): <https://github.com/github/spec-kit>
- **Scalable planning depth** (small changes go straight to implementation; big ones deepen planning) — BMAD-METHOD (MIT): <https://github.com/bmad-code-org/BMAD-METHOD>
- **Subagent file format** (markdown + YAML frontmatter) — VoltAgent/awesome-claude-code-subagents (MIT): <https://github.com/VoltAgent/awesome-claude-code-subagents>
- **Process skills referenced by the protocol** (TDD, systematic debugging, verification-before-completion) — obra/superpowers (installed separately): <https://github.com/obra/superpowers>

## Positioning vs. related plugins

- `superpowers` teaches process *skills* to the main agent; ECA runs *specialized subagents* behind a gate protocol (they complement: ECA's protocol references superpowers skills).
- `code-review` / `pr-review-toolkit` cover the review stage only; ECA pipelines architecture → implementation → parallel review → fix loop.
- `feature-dev` offers a specialized-agent feature workflow; ECA's differentiators are the delivery-acceptance levels, typed DoD library, read-only hard tool boundaries, and domain-constraint injection at dispatch time.

## Validation status

- Planted-defect acceptance project (injection, hardcoded token, temporal coupling, magic error code, swallowed exception, zero tests): v0.1.1 reviewers detect all planted classes and correctly distinguish *increment gate* (block new defects) from *legacy sweep* (list, hand over, don't block).
- Field trial on a local multi-GPU scheduler (L2 pipeline): five implementation batches, 567 tests zero warnings, 7 review rejections all closed. Real-hardware acceptance ongoing.
- A single-sample baseline comparison showed a plain general-purpose review may match ECA's detection on a *small single file*; ECA's value is the auditable, versionable lower bound (checklist-driven) plus gates — see README.zh-CN for the honest note.

## License

MIT. See [LICENSE](LICENSE). The plugin rewrites (not copies) ideas from the MIT/Apache projects listed above; see each reference for its own license.
