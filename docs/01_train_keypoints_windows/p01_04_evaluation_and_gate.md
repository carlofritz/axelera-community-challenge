# P01.04 Evaluation And Gate

## Purpose
Define the exact post-training checks and pass/fallback decision process.

## Inputs
1. Training output folder (for example `runs/p01/y26n_main`).
2. `best.pt` and run metadata.
3. Frozen thresholds from `p01_01_frozen_spec.md`.

## Required Checks
1. Quantitative metrics:
   - `metrics/pose(mAP50-95) >= 0.30`
   - `metrics/pose(mAP50) >= 0.60`
2. Qualitative validation:
   - sample 100 frames
   - at least 85 frames with plausible stable landmarks on one visible hand
3. Runtime sanity:
   - inference with `best.pt` completes on sample video/images
   - no keypoint shape mismatch in prediction stage

## Commands (Lightning/CLI)
Read best metrics from results:
```bash
python - <<'PY'
from pathlib import Path
import pandas as pd
p = Path("runs/p01/y26n_main/results.csv")
df = pd.read_csv(p)
last = df.iloc[-1]
print("mAP50-95", last.get("metrics/pose(mAP50-95(B))"))
print("mAP50", last.get("metrics/pose(mAP50(B))"))
PY
```

Run explicit validation with best weights:
```bash
yolo pose val model=runs/p01/y26n_main/weights/best.pt data=hand-keypoints.yaml device=0
```

Run quick prediction sanity:
```bash
yolo pose predict model=runs/p01/y26n_main/weights/best.pt source=sample.mp4 device=0 save=True project=runs/p01 name=y26n_predcheck
```

## Decision Gate
Pass primary model (`yolo26n-pose`) if all are true:
1. Both metric thresholds pass.
2. Qualitative frame check passes.
3. Prediction sanity completes without shape or decode anomalies.

Trigger fallback (`yolov8n-pose`) if any is true:
1. Two focused primary attempts fail (training/deploy compatibility).
2. Metrics are materially below threshold and cannot be recovered in sprint timebox.
3. Downstream deployment constraints block first-correct overlay timeline.

## Log Output (Required)
Record these in `notes/training_run_log.md` and `learning.md`:
1. Command used.
2. Final metrics.
3. Gate decision (pass or fallback).
4. Rationale and next action.
