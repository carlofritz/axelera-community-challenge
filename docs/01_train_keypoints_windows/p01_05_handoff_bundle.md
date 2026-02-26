# P01.05 Handoff Bundle

## Purpose
Standardize what is handed from training (Lightning) to Linux/Voyager deployment.

## Required Bundle Contents
1. Model artifacts:
   - `best.pt`
   - optional `last.pt`
2. Dataset config:
   - exact `hand-keypoints.yaml` used for run
3. Run metadata:
   - train command(s)
   - package versions (`ultralytics`, `torch`)
   - GPU type
4. Metrics/evidence:
   - validation metrics output
   - representative prediction outputs (images or short video)
5. Decision note:
   - `primary_passed` or `fallback_triggered`
   - short reason

## Suggested Folder Layout
```text
artifacts/01_train_keypoints_windows/vYYYYMMDD-1/
  weights/
    best.pt
    last.pt
  config/
    hand-keypoints.yaml
    train_command.txt
    env_versions.txt
  metrics/
    val_metrics.txt
    gate_decision.txt
  samples/
    pred_*.jpg
    pred_preview.mp4
```

## Packaging Commands
Create handoff folder:
```bash
mkdir -p artifacts/01_train_keypoints_windows/vYYYYMMDD-1/{weights,config,metrics,samples}
```

Copy core files:
```bash
cp runs/p01/y26n_main/weights/best.pt artifacts/01_train_keypoints_windows/vYYYYMMDD-1/weights/
cp hand-keypoints.yaml artifacts/01_train_keypoints_windows/vYYYYMMDD-1/config/
```

Create compressed handoff:
```bash
cd artifacts/01_train_keypoints_windows
tar -czf vYYYYMMDD-1.tar.gz vYYYYMMDD-1
```

## Exit Criteria To Close P01.05
1. Bundle exists with all mandatory files.
2. Gate decision is documented.
3. Linux/Voyager phase can start without missing inputs.
