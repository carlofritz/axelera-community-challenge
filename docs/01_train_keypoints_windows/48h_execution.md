# 48-Hour Execution Runbook

## Objective
By Saturday, deliver an end-to-end demo:
- video/camera input
- hand detections + 21 keypoints overlay
- running on Orange Pi 5 Plus + Metis

Accuracy refinement is explicitly out of critical path for this sprint.

## Ground Truth
1. Dataset baseline: Ultralytics Hand Keypoints.
2. Keypoint shape: `kpt_shape: [21, 3]`.
3. Primary training model: `yolo26n-pose`.
4. Fallback model: `yolov8n-pose` (only on documented blocker).

## Strategy
1. Train on Lightning.ai Studio (cloud GPU).
2. Deploy on Linux through Voyager custom-weights YAML flow.
3. Validate correctness first via `torch-aipu` style path.
4. Attempt `gst` optimization only if 21x3 decode path is confirmed.

## Phase Plan

### A: Setup + Sanity (Hour 0-2)
1. Activate Voyager environment on deployment host.
2. Verify device visibility (`axdevice` or equivalent).
3. Run one known-good zoo pipeline to de-risk toolchain.

Deliverable:
- One stock command path that runs end-to-end.

### B: Dataset + Fine-Tune (Hour 2-10)
1. Download Ultralytics hand-keypoint dataset.
2. Confirm `kpt_shape: [21, 3]` in dataset yaml.
3. Train from pretrained `yolo26n-pose`.
4. Run smoke training first, then one main run.
5. Save `best.pt` and command log.

Deliverables:
- `best.pt`
- Reproducible training command in notes.

### C: Conversion (Hour 10-14)
1. Copy Voyager pose YAML template into customer area.
2. Replace weight path with absolute path to `best.pt`.
3. Update model/task fields for keypoint task.
4. Configure dataset adapter (`ultralytics_data_yaml` if available).
5. Deploy with Voyager tooling.

Fallback rule:
- If this path blocks repeatedly, switch training artifact to `yolov8n-pose`.

Deliverable:
- Deployable custom YAML for hand keypoints.

### D: First Correct Inference (Hour 14-18)
1. Run on video file first.
2. Decode keypoints as `(N, 21, 3)`.
3. Draw overlays and validate visually.

Deliverable:
- Recorded output with visible hand keypoints.

### E: Live Camera + Packaging (Hour 18-24)
1. Move to live camera on Orange Pi.
2. Keep pipeline simple: capture -> infer -> decode -> overlay -> display.
3. Capture exact run commands in project docs.

Deliverable:
- One-liner demo command for live run.

### F: Performance Pass (Hour 24-32, Optional)
1. Reduce input resolution: 640 -> 512 -> 416.
2. Keep smallest model.
3. Add calibration frames if needed.
4. Try `gst` path only after correctness passes.

Deliverable:
- Stable best-effort FPS without breaking output correctness.

## Blocking Conditions To Trigger Fallback
Switch from `yolo26n-pose` to `yolov8n-pose` if any is true:
1. Two focused attempts fail due model/toolchain compatibility.
2. Deployment remains blocked past sprint timebox.
3. Demo cannot reach stable correct outputs with primary model path.

## Notes
- Left/right hand classification is optional and non-blocking.
- "LLM-assisted kernels" is treated as advanced/non-critical unless vendor provides exact supported workflow.
