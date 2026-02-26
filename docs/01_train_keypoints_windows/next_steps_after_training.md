# Next Steps After Training

Use this checklist immediately after the current training run finishes.

## Step 0: Model Evaluation
1. Run validation on `best.pt`.
2. Run quick prediction sanity on sample video/images.
3. Confirm keypoint outputs are coherent for hand landmarks.

Reference:
- `docs/01_train_keypoints_windows/p01_04_evaluation_and_gate.md`

## Step 1: Gate Decision
1. Compare metrics against frozen thresholds:
   - `metrics/pose(mAP50-95) >= 0.30`
   - `metrics/pose(mAP50) >= 0.60`
2. Run qualitative 100-frame check.
3. Decide:
   - `primary_passed` (`yolo26n-pose`) or
   - `fallback_triggered` (`yolov8n-pose`).

## Step 2: Log Results
1. Fill `notes/training_run_log.md`.
2. Update `learning.md` with run outcome and rationale.
3. Append decision note in `plan.md` progress log.

## Step 3: Package Handoff Bundle
1. Prepare structured artifact folder.
2. Include `best.pt`, dataset yaml, command/env metadata, metrics, and sample outputs.
3. Create compressed handoff archive.

Reference:
- `docs/01_train_keypoints_windows/p01_05_handoff_bundle.md`

## Step 4: Start Linux/Voyager Bring-Up
1. Move handoff bundle to Linux host.
2. Run environment sanity and a stock zoo pipeline first.
3. Start custom-weights bring-up with hand model.
4. Keep `torch-aipu` correctness path first; defer `gst` optimization if needed.

Reference:
- `docs/02_export_axelera_linux/p02_01_linux_voyager_bringup.md`
