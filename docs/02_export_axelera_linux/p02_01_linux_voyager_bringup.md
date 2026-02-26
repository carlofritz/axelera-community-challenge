# P02.01 Linux + Voyager Bring-Up Checklist

## Purpose
Prepare Linux host and Voyager toolchain so deployment starts immediately after P01 handoff.

## Inputs From P01
1. Handoff bundle from `p01_05_handoff_bundle.md`.
2. `best.pt` (or fallback model artifact).
3. Dataset yaml used for training.

## Environment Checklist
1. Confirm Linux host and Metis visibility.
2. Activate Voyager environment.
3. Verify framework/runtime environment variables are set.
4. Run `axdevice` (or equivalent) and save output.

## Toolchain Sanity
1. Run one known-good zoo model first.
2. Confirm deployment path works before custom model.
3. Confirm inference path works on sample source.

## Custom-Weights Preparation
1. Copy reference keypoint/detection YAML into customer area.
2. Set absolute `weight_path` to handed `best.pt`.
3. Set task/category fields for keypoints.
4. Configure dataset reference/calibration.
5. Prefer correctness path first (`torch-aipu` style) before `gst` optimization.

## Fallback Rules During P02
1. If primary model (`yolo26n-pose`) blocks at deployment integration:
   - switch to fallback artifact (`yolov8n-pose`) using same pipeline flow.
2. If `gst` decode causes keypoint-shape mismatch:
   - stay on correctness path and defer `gst`.

## Deliverables
1. First successful deploy command.
2. First successful inference command.
3. Runtime notes and any blocker entries in `notes/booth_answers.md`.
