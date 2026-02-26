# Repository Rules

## Scope
This repository governs a modular, phase-based build of a hand keypoint detection pipeline targeting Axelera Metis, with Orange Pi 5 Plus as host.

Primary sequence:
1. Train keypoint model on Windows using Ultralytics.
2. Export model on Linux for Axelera runtime compliance.
3. Validate runtime execution on Metis.
4. Integrate post-processing as a later phase.

Agent guidance is defined in `AGENTS.md` and should stay concise, practical, and testable.

## Non-Negotiable Rules
1. Preserve validated outputs. Never overwrite known-good phase artifacts.
2. Work one phase at a time. Only one active phase is allowed.
3. Every phase must declare explicit inputs, outputs, and acceptance criteria.
4. Every phase transition must be logged in `plan.md` (`Current Step` + `Progress Log`).
5. Keep experiments isolated. Experimental results are not promoted without validation evidence.
6. Changes must be additive or versioned when touching previously validated work.

## Modular Folder Conventions
Use numbered, stable modules for docs and artifacts:

- `docs/00_governance`
- `docs/01_train_keypoints_windows`
- `docs/02_export_axelera_linux`
- `docs/03_runtime_validation_metis`
- `docs/04_postprocessing_integration`
- `artifacts/01_train_keypoints_windows`
- `artifacts/02_export_axelera_linux`
- `artifacts/03_runtime_validation_metis`
- `artifacts/04_postprocessing_integration`

Step IDs are mandatory and must follow `P<phase>.<step>`:
- Example: `P01.01`, `P01.02`, `P02.01`

Artifact version naming:
- `vYYYYMMDD-N` (example: `v20260226-1`)

## Progress Tracking Rules
`plan.md` is the source of truth for current status.

Required behavior:
1. Keep exactly one active step in `Current Step`.
2. Append a dated line in `Progress Log` on each meaningful status change.
3. Use factual entries only (what changed, why, result).
4. Record blockers explicitly and mark impacted step IDs.

## Definition Of Done (Per Step)
A step is done only when all are true:
1. Inputs and outputs are documented.
2. Command/config used is recorded and reproducible.
3. Validation evidence is stored or linked.
4. Risks/issues are captured with next action.

## Change Control
When updating completed modules:
1. Create a new versioned artifact instead of replacing prior validated outputs.
2. Update `plan.md` with rationale and impact.
3. Keep backward traceability between source data, training config, and produced model.

## Version Control Guardrails
1. `main` is the stable branch and must stay deployable/documented.
2. Do not develop phase work directly on `main`; use `feature/<topic>` branches.
3. Every merged change must map to at least one phase step ID when applicable.
4. Use pull requests for merge, with at least one review before merging.
5. Tag phase-ready milestones (`phase-p01-ready`, etc.) after acceptance criteria pass.

## Response Mode: --explain
If a user request contains `--explain`, switch to guided mode for that request.

Mandatory output format:
1. Provide an ordered step-by-step procedure.
2. Include exact runnable commands in code blocks.
3. Add a brief one-line explanation per command or step.
4. End with verification commands (how to confirm success).

Scope:
1. Applies to any request that includes `--explain`.
2. Without `--explain`, keep responses concise by default.
3. For risky operations, include a safety checkpoint before execution.

## AGENTS.md Quality Rules
1. Keep `AGENTS.md` concise (target <150 lines unless clearly needed).
2. Maintain these six core sections:
   - Commands
   - Testing
   - Project Structure
   - Code Style
   - Git/PR Workflow
   - Boundaries (`Always / Ask first / Never`)
3. Prefer concrete commands, real paths, and examples over generic guidance.
4. Add new rules only after repeated agent mistakes (progressive disclosure).
5. Keep Codex-specific behavior explicit: guidance is loaded from docs, but actions still require explicit execution.
6. If using other tools requiring proprietary files, mirror `AGENTS.md` via wrapper/symlink instead of diverging guidance.

## Documentation Cleanup Routine
Regularly execute or propose a cleanup pass to keep governance and onboarding clear.

Cadence:
1. Trigger cleanup every 3 to 5 meaningful updates, or at phase boundary completion.
2. Propose cleanup explicitly if scope/status has shifted but docs lag behind.

Checklist:
1. `README.md`: refresh current status, quick-start commands, and structure overview.
2. `plan.md`: ensure one active step, append factual progress log entries, remove stale "next action" items.
3. `learning.md`: consolidate duplicates, promote stable procedures to playbooks, archive outdated notes.
4. `AGENTS.md`: keep concise, remove generic redundancy, keep boundaries and commands accurate.
5. `rules.md`: adjust only for repeated failure patterns or governance drift.
6. Validate cross-file consistency (`README.md` <-> `plan.md` <-> `learning.md` <-> `rules.md`).

Output expectation for cleanup proposals:
1. State why cleanup is due.
2. List files to update.
3. List intended tidy actions per file.

## Practical Guardrail
If a decision could break previously working results, branch it into a new versioned step/artifact and do not mutate the validated baseline.
