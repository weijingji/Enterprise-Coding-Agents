# Changelog

## 0.1.1 (2026-09-08)

- code-reviewer: added "error-handling integrity" checklist block (swallowed exceptions / silent failures); semantic-coupling item now explicitly sweeps literal error/status codes crossing function boundaries; workflow gains a mandatory legacy-code sweep step (findings listed separately, never silently dropped).
- security-reviewer: error-handling section now checks swallowed exceptions and fake-success responses; same mandatory legacy-code sweep step.
- Both changes came out of a controlled acceptance run: v0.1.0 missed a planted bare-except defect; v0.1.1 detects it reliably (see README "Validation status").

## 0.1.0 (2026-09-07)

- Initial release: six agents, orchestration-protocol skill, `/eca` command entry.
