# Axelera Community Challenge

Hand keypoint detection pipeline for Axelera Metis (accelerator) with Orange Pi 5 Plus (host).

## Current Status
- Active step: `P01.01`
- Active platform: `Windows`
- Next platform: `Linux` (export/runtime alignment)

For live progress and step tracking, see `plan.md`.

## Goal
Build a modular pipeline in four phases:
1. Train hand keypoint model with Ultralytics on Windows.
2. Export trained model for Axelera runtime on Linux.
3. Validate runtime execution on Metis.
4. Integrate post-processing.

## Repository Structure
- `rules.md`: repository guardrails and workflow rules.
- `AGENTS.md`: agent-focused execution guide.
- `plan.md`: active step and execution plan.
- `learning.md`: reusable learnings and command playbooks.
- `docs/`: phase-specific documentation.
- `artifacts/`: phase-specific outputs and evidence.

## Working Workflow
1. Read `plan.md` and confirm current step.
2. Create a branch from `main` (`feature/<topic>`).
3. Make scoped changes for one step/module.
4. Update docs (`plan.md`, `learning.md`, and this file if status/scope changed).
5. Commit, push, and open PR.

## Quick Start
```powershell
git status --short --branch
git checkout -b feature/<topic>
```

## Documentation Maintenance
This file is a living summary and should stay concise.
Update when any of these change:
- Active step or phase status.
- High-level architecture or workflow.
- Onboarding/quick-start commands.
- Directory/module layout.

Detailed execution history belongs in `plan.md` and `learning.md`.
