# Axelera Community Challenge

Hand keypoint detection pipeline for Axelera Metis (accelerator) with Orange Pi 5 Plus (host).

## Current Status
- Active step: `P01.02`
- Active training environment: `Lightning.ai Studio (cloud GPU)`
- Deployment platform: `Linux` (Voyager SDK on Orange Pi 5 Plus + Metis)
- Primary model plan: `yolo26n-pose`
- Fallback model plan: `yolov8n-pose` (only if primary path blocks)

For live progress and step tracking, see `plan.md`.

## Goal
Build a modular pipeline in four phases:
1. Train/fine-tune hand keypoint model with Ultralytics on Lightning.ai.
2. Export trained model for Axelera runtime on Linux.
3. Validate runtime execution on Metis.
4. Integrate post-processing.

## Repository Structure
- `rules.md`: repository guardrails and workflow rules.
- `AGENTS.md`: agent-focused execution guide.
- `plan.md`: active step and execution plan.
- `learning.md`: reusable learnings and command playbooks.
- `docs/01_train_keypoints_windows/48h_execution.md`: execution runbook (Lightning + Voyager path).
- `docs/01_train_keypoints_windows/p01_01_frozen_spec.md`: frozen task/metric/fallback contract.
- `docs/01_train_keypoints_windows/p01_02_p01_03_prep_checklists.md`: dataset/environment/command preparation checklists.
- `docs/01_train_keypoints_windows/p01_04_evaluation_and_gate.md`: post-training gate and fallback decision flow.
- `docs/01_train_keypoints_windows/p01_05_handoff_bundle.md`: standardized training-to-Linux handoff bundle.
- `docs/02_export_axelera_linux/p02_01_linux_voyager_bringup.md`: Linux/Voyager bring-up checklist.
- `notes/training_run_log.md`: run-by-run metrics and gate log template.
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

## Model Strategy (Time-Boxed)
1. Start with `yolo26n-pose` for the first end-to-end demo.
2. Keep `yolov8n-pose` as fallback if YOLO26 path blocks due tooling compatibility.
3. Prioritize correctness on Metis first (`torch-aipu` path), optimize performance second.

## Documentation Maintenance
This file is a living summary and should stay concise.
Update when any of these change:
- Active step or phase status.
- High-level architecture or workflow.
- Onboarding/quick-start commands.
- Directory/module layout.

Detailed execution history belongs in `plan.md` and `learning.md`.
