# Hand Keypoint Pipeline Plan

## Current Step
`P01.01` - Problem definition and keypoint target specification.

## Current Platform
Windows (active for training preparation and model training).

## Next Platform
Linux (planned for Axelera-compliant export and runtime preparation).

## Overall Goal
Build a hand keypoint detection pipeline for Axelera Metis (accelerator) with Orange Pi 5 Plus (host), starting with model training and then export/runtime integration.

## Phase Index
- `P01` In Progress: Train hand keypoint model with Ultralytics on Windows.
- `P02` Planned: Export trained model on Linux in accordance with Axelera runtime requirements.
- `P03` Planned: Validate model runtime execution on Metis.
- `P04` Planned: Define and integrate post-processing.

## Phase Details

### P01 - Train Keypoint Model On Windows
Goal:
Produce a validated hand keypoint model checkpoint suitable for downstream export.

Inputs:
- Hand keypoint task definition.
- Dataset and annotation schema.
- Ultralytics training configuration.

Outputs:
- Trained model checkpoint(s).
- Metrics summary and run metadata.
- Handoff package for export phase.

Steps:
1. `P01.01` Define problem scope, keypoint set, and success metrics.
2. `P01.02` Finalize dataset source/format and annotation mapping.
3. `P01.03` Build baseline Ultralytics config and training recipe.
4. `P01.04` Execute training run(s), capture metrics, evaluate quality.
5. `P01.05` Select best model and prepare export handoff bundle.

Acceptance Criteria:
- Task definition and keypoint schema are frozen for Phase 1.
- At least one reproducible training run is documented.
- Best checkpoint and training metadata are archived in phase artifacts.

Risks:
- Dataset quality and keypoint consistency may limit training convergence.
- Early config choices may impact export compatibility later.

### P02 - Export Model On Linux For Axelera Runtime
Goal:
Convert trained model into the required Axelera runtime-compatible format on Linux.

Inputs:
- Best checkpoint and metadata from `P01`.
- Axelera runtime/export toolchain requirements.

Outputs:
- Exported model package for target runtime.
- Export command history and compatibility notes.

Acceptance Criteria:
- Export completes without blocking compatibility errors.
- Output package is ready for runtime deployment testing.

### P03 - Runtime Validation On Metis
Goal:
Run and validate the exported model on Metis with Orange Pi 5 Plus host.

Inputs:
- Exported package from `P02`.
- Target runtime environment setup.

Outputs:
- Runtime validation report (latency, throughput, functional checks).

Acceptance Criteria:
- Model runs successfully on target hardware.
- Core functional behavior matches expected keypoint output pattern.

### P04 - Post-Processing Integration
Goal:
Define and implement post-processing after baseline runtime success.

Inputs:
- Stable runtime output from `P03`.
- Post-processing requirements and output contract.

Outputs:
- Post-processing module design and implementation plan.

Acceptance Criteria:
- Clear integration path defined with measurable validation criteria.

## Progress Log
- `2026-02-26`: Initialized repository governance and modular phase structure. Set active step to `P01.01`.
- `2026-02-26`: Initialized Git repository, added `.gitignore`/`.gitattributes`, and defined branch/PR flow in `docs/00_governance/version-control.md`.
- `2026-02-26`: Added `--explain` response mode rules and created root `learning.md` with phase-based knowledge structure and reusable command playbooks.
- `2026-02-26`: Added root `AGENTS.md` with six-core sections and boundary tiers; aligned `rules.md`/`learning.md` with iterative AGENTS maintenance guidance.
- `2026-02-26`: Added living `README.md` and formalized recurring documentation cleanup routine in `rules.md`.

## Next Immediate Action
Complete `P01.01` by freezing:
1. Hand keypoint list and ordering.
2. Target training/evaluation metrics.
3. Minimum acceptance threshold for advancing to `P01.02`.
