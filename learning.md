# Learning Log

## How To Use This Document
This file is the durable knowledge base for the project.
It captures reusable procedures, lessons learned, and phase-specific decisions.

Update triggers:
1. Current step changes in `plan.md`.
2. A blocker is resolved or a new blocker appears.
3. A reusable command sequence is identified.

Tidy policy:
1. Every 3 to 5 meaningful entries, consolidate duplicates.
2. Promote stable procedures into the playbooks section.
3. Keep short, factual entries and link to phase docs/artifacts when available.

## Project Snapshot
- Date: `2026-02-26`
- Active step: `P01.02`
- Active training environment: `Lightning.ai Studio`
- Next platform: `Linux` (Voyager deployment on Orange Pi 5 Plus + Metis)

## Governance Learnings (Agents + Workflow)
Accepted into repository process:
1. `AGENTS.md` is added as the agent-facing operations guide.
2. Keep guidance minimal and iterative; expand only after repeated failure.
3. Use six-core structure for high-signal guidance:
   - Commands
   - Testing
   - Project Structure
   - Code Style
   - Git/PR Workflow
   - Boundaries (`Always / Ask first / Never`)
4. Keep explicit `--explain` behavior for step-by-step command guidance.
5. Treat safety boundaries as high priority (no secrets, no destructive surprises).
6. Maintain a living `README.md` for current status/onboarding and apply recurring cleanup passes.

Adjusted for this repo:
1. This project remains phase-first (`plan.md`) and hardware-pipeline focused.
2. AGENTS guidance complements, but does not replace, `rules.md` and `plan.md`.
3. Codex behavior is handled pragmatically: provide commands and run only when requested/appropriate.
4. Primary model family is `YOLO26 pose`; fallback model family is `YOLOv8 pose` on documented blocker only.
5. Official docs currently show a YOLO26/Axelera pose-support inconsistency; we treat export support as a test gate and keep fallback paths ready.

## Phase Learnings

### P01 Training (Lightning.ai)
Goal:
Train a hand keypoint model with Ultralytics and produce a reproducible handoff checkpoint.

What Worked:
- Phase-based governance and progress tracking are now in place.
- Parallel preparation while training runs reduces idle time and keeps phase transitions smooth.

What Failed / Risks:
- YOLO26 may require fallback to YOLOv8n-pose if deployment compatibility blocks progress.

Reusable Commands:
```powershell
git status --short --branch
```
Check current branch and working tree state.

Decisions Taken:
- Use modular phase folders.
- Track one active step at a time in `plan.md`.
- Use Lightning.ai Studio for cloud-GPU training.
- Use `yolo26n-pose` as primary and `yolov8n-pose` as fallback.
- Freeze P01.01 with explicit keypoint order, metric floor, and fallback hard gates.

Open Questions:
- Final training outcome and gate decision from current 100-epoch run.

Next Update Trigger:
- Current training run finishes and `P01.04` gate decision is recorded.

### P02 Export (Linux/Axelera)
Goal:
Export the trained model in Axelera runtime-compliant format on Linux.

What Worked:
- Not started.

What Failed / Risks:
- Toolchain compatibility constraints are not yet validated.

Reusable Commands:
- Placeholder until phase execution starts.

Decisions Taken:
- Linux is the export environment.

Open Questions:
- Exact Axelera export commands and model constraints.

Next Update Trigger:
- Begin `P02.01` with concrete export checklist.

### P03 Runtime Validation (Metis + Orange Pi 5 Plus)
Goal:
Validate runtime behavior of exported model on target hardware.

What Worked:
- Not started.

What Failed / Risks:
- Not started.

Reusable Commands:
- Placeholder until phase execution starts.

Decisions Taken:
- Host device is Orange Pi 5 Plus; accelerator is Metis.

Open Questions:
- Runtime KPIs and acceptance thresholds.

Next Update Trigger:
- Start runtime deployment validation.

### P04 Post-Processing
Goal:
Define and integrate post-processing after baseline runtime success.

What Worked:
- Deferred by design.
- Initial loose software concept for user-defined pose triggers is now documented.
- P04 now has an implementation-ready V0 blueprint (API, types, pipeline, defaults, tests).

What Failed / Risks:
- Static-pose V0 is defined, but runtime integration risks remain (latency, false triggers under occlusion).

Reusable Commands:
- Placeholder until phase execution starts.

Decisions Taken:
- Post-processing intentionally follows successful runtime validation.
- V0 uses local webapp + FastAPI backend on Orange Pi, same-process topology.
- V0 trigger outputs are webhook + local logs, with JSON persistence.
- Dynamic gestures are explicitly deferred and planned as a post-V0 extension.

Open Questions:
- Threshold tuning and robustness targets under real lighting/camera variation.

Next Update Trigger:
- First live trigger event emitted on target hardware from integrated runtime.

## Reusable Command Playbooks

### GitHub Setup And Connection
```powershell
gh auth login -h github.com
gh repo create axelera-community-challenge --public --source . --remote origin --push
git push -u origin main
```
Authenticate, create/connect remote repository, and push `main`.

### Branch + PR Flow
```powershell
git checkout -b feature/<topic>
git add <files>
git commit -m "feat(scope): short summary"
git push -u origin feature/<topic>
```
Create a feature branch, commit scoped changes, and publish for PR.

### Training Run Skeleton (Ultralytics)
```powershell
yolo train model=<model>.pt data=<data>.yaml epochs=<n> imgsz=<size>
```
Baseline skeleton for training commands (to be pinned in P01 with exact values).

### Export Skeleton (Linux/Axelera)
```bash
# Placeholder: fill with exact Axelera export commands in P02.
```
Reserved section for validated export procedure.

## Periodic Cleanup Notes
- Keep only validated and repeatable command flows in playbooks.
- Move phase-specific detail to module docs once they grow.
- Keep this file concise and operational.
