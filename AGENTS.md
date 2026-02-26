---
name: axelera-hand-pipeline-agent
description: Repository operating guide for AI coding agents working on the hand keypoint pipeline.
---

# AGENTS Guide

Use this file as agent-specific execution guidance for this repository.
Keep changes modular, reversible, and phase-aligned with `plan.md`.

## Commands
Use PowerShell from repo root unless noted.

```powershell
git status --short --branch
```
Quick state check before and after edits.

```powershell
git checkout -b feature/<topic>
```
Start scoped work on a feature branch.

```powershell
git add <files>; git commit -m "docs(scope): message"
```
Commit small, single-purpose changes.

```powershell
gh auth status
```
Check GitHub CLI auth before remote actions.

## Testing
- For doc-only changes: verify links/paths/step IDs are internally consistent.
- For code changes: run the narrowest relevant check first, then broaden.
- Never report "done" without at least one explicit verification result.

## Project Structure
- Governance docs:
  - `rules.md`
  - `plan.md`
  - `learning.md`
  - `docs/00_governance/version-control.md`
- Phase docs:
  - `docs/01_train_keypoints_windows`
  - `docs/02_export_axelera_linux`
  - `docs/03_runtime_validation_metis`
  - `docs/04_postprocessing_integration`
- Artifacts are phase-scoped under `artifacts/`.

## Code Style And Documentation Style
- Prefer concise, concrete instructions over long prose.
- Use explicit paths, exact commands, and step IDs (`P01.01`, etc.).
- Keep AGENTS/rules guidance short; add a rule only after repeated failure.
- Prefer examples over abstract style statements.

## Git / PR Workflow
1. Branch from `main` using `feature/<topic>`.
2. Keep commits atomic and traceable to a phase step when applicable.
3. Open PR and merge after review.
4. Do not force-push `main`.

## Boundaries
### Always
- Preserve validated outputs.
- Update `plan.md` progress log when scope/state changes.
- Keep one active step in `plan.md`.
- Ask for confirmation before destructive or high-risk commands.

### Ask First
- Large refactors across multiple phases.
- Changes to accepted baselines or artifact naming/version conventions.
- Any operation requiring secrets/tokens not already configured.

### Never
- Commit secrets or credentials.
- Rewrite history on shared branches.
- Overwrite known-good artifacts in place.
- Assume Linux-only commands work on Windows without adaptation.

## --explain Mode
If user message includes `--explain`:
1. Provide ordered steps.
2. Include exact commands.
3. Briefly explain each step/command.
4. End with verification commands.

## When Stuck
1. Re-read `rules.md` and current step in `plan.md`.
2. Reduce change size and isolate one decision.
3. State blockers clearly with exact failing command/output.
4. Propose 1-2 concrete next actions with tradeoffs.
