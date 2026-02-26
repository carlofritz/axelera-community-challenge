# Hand Keypoint Pipeline Plan

## Current Step
`P01.02` - Finalize dataset source/format and annotation mapping.

## Current Platform
Lightning.ai Studio (active for cloud-GPU training and model preparation).

## Next Platform
Linux (planned for Axelera-compliant export and runtime preparation).

## Overall Goal
Build a hand keypoint detection pipeline for Axelera Metis (accelerator) with Orange Pi 5 Plus (host), starting with model training and then export/runtime integration.

## Phase Index
- `P01` In Progress: Train hand keypoint model with Ultralytics on Lightning.ai.
- `P02` Planned: Export trained model on Linux in accordance with Axelera runtime requirements.
- `P03` Planned: Validate model runtime execution on Metis.
- `P04` Planned: Define and integrate post-processing.

## Phase Details

### P01 - Train Keypoint Model On Lightning.ai
Goal:
Produce a validated hand keypoint model checkpoint suitable for downstream export.

Inputs:
- Hand keypoint task definition.
- Ultralytics Hand Keypoints dataset schema (`kpt_shape: [21, 3]`).
- Ultralytics training configuration.
- Cloud GPU training environment on Lightning.ai.

Outputs:
- Trained model checkpoint(s).
- Metrics summary and run metadata.
- Handoff package for export phase.

Steps:
1. `P01.01` Define problem scope, keypoint set, and success metrics.
2. `P01.02` Finalize dataset source/format and annotation mapping.
3. `P01.03` Build baseline Ultralytics config and training recipe (`yolo26n-pose` primary).
4. `P01.04` Execute training run(s), capture metrics, evaluate quality.
5. `P01.05` Select best model and prepare export handoff bundle.

P01.01 Frozen Spec (2026-02-26):
1. Keypoint schema is fixed to 21 points with dimensionality `(x, y, visibility)` and `kpt_shape: [21, 3]`.
2. Keypoint ordering is fixed to MediaPipe hand landmark order:
   - `0 wrist`
   - `1 thumb_cmc`
   - `2 thumb_mcp`
   - `3 thumb_ip`
   - `4 thumb_tip`
   - `5 index_finger_mcp`
   - `6 index_finger_pip`
   - `7 index_finger_dip`
   - `8 index_finger_tip`
   - `9 middle_finger_mcp`
   - `10 middle_finger_pip`
   - `11 middle_finger_dip`
   - `12 middle_finger_tip`
   - `13 ring_finger_mcp`
   - `14 ring_finger_pip`
   - `15 ring_finger_dip`
   - `16 ring_finger_tip`
   - `17 pinky_mcp`
   - `18 pinky_pip`
   - `19 pinky_dip`
   - `20 pinky_tip`
3. Demo-quality acceptance thresholds for advancing from P01:
   - `metrics/pose(mAP50-95) >= 0.30`
   - `metrics/pose(mAP50) >= 0.60`
   - visual check: at least 85/100 sampled frames show one hand with stable, plausible landmarks.
4. Model policy remains:
   - Primary: `yolo26n-pose`
   - Fallback: `yolov8n-pose` only on documented blocker criteria.
5. Fallback trigger hard gates:
   - two focused incompatibility failures in primary path, or
   - missed timebox for first correct overlay on Metis by end of Phase D window.

Acceptance Criteria:
- Task definition and keypoint schema are frozen for Phase 1.
- At least one reproducible training run is documented.
- Best checkpoint and training metadata are archived in phase artifacts.
- Fallback gate is defined: move to `yolov8n-pose` only on documented blocker.

Risks:
- Dataset quality and keypoint consistency may limit training convergence.
- Early config choices may impact export compatibility later.
- YOLO26 integration risk may require fallback to YOLOv8n-pose within timebox.

Critical Double-Checks (web-verified on 2026-02-26):
1. Ultralytics docs are currently inconsistent:
   - Pose task page shows `Axelera` as available export format for YOLO26-pose.
   - Axelera integration page "Supported Tasks" still lists Pose as "Coming soon".
2. Treat YOLO26 -> Axelera pose export as a must-verify gate, not an assumption.
3. Axelera integration docs list Linux-only export requirements and known first-run issues (including first-run export/inference friction).
4. Voyager custom-weights flow is the operational fallback because the compiler supports direct PyTorch model deployment with dataset-based calibration.

Fallback Decision Tree:
1. Attempt primary path: `yolo26n-pose` + Voyager custom YAML + `torch-aipu` correctness run.
2. If blocked by model/toolchain compatibility after two focused attempts, switch to `yolov8n-pose`.
3. If `gst` path fails on keypoint shape handling, keep demo on `torch-aipu` and defer `gst` optimization.
4. Record fallback trigger and evidence in `plan.md` progress log and `learning.md`.

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
- `2026-02-26`: Switched primary P01 training path to Lightning.ai + `yolo26n-pose`, with `yolov8n-pose` as explicit fallback plan.
- `2026-02-26`: Final web-validation pass added explicit double-check gates and fallback decision tree for YOLO26/YOLOv8 and `torch-aipu`/`gst` runtime strategy.
- `2026-02-26`: Rebased execution branch onto latest governance branch and froze `P01.01` spec (keypoint order, acceptance thresholds, fallback hard gates); moved active step to `P01.02`.

## Next Immediate Action
Complete `P01.02` and `P01.03` preparation:
1. Validate dataset files and `hand-keypoints.yaml` consistency against frozen `P01.01` schema.
2. Finalize Lightning.ai environment checklist and reproducible training command templates.
3. Prepare smoke-run and main-run configs for `yolo26n-pose`, plus explicit fallback command for `yolov8n-pose`.
