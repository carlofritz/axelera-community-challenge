# P01.02-P01.03 Preparation Checklists

## Purpose
Operational checklist for dataset validation and baseline training setup after `P01.01` freeze.

## P01.02 Dataset Checklist
1. Confirm Ultralytics dataset yaml path is fixed and versioned in notes.
2. Verify `kpt_shape: [21, 3]` in dataset yaml.
3. Verify class mapping is consistent with single-hand class setup.
4. Sample-check label lines for keypoint field consistency.
5. Confirm train/val file lists are resolvable from training environment.
6. Record dataset source URL and retrieval date in notes.

Validation command examples:
```powershell
Get-Content .\hand-keypoints.yaml
```
Inspect dataset yaml values.

```powershell
python - <<'PY'
from pathlib import Path
labels = list(Path("datasets/hand-keypoints/labels/train").glob("*.txt"))
print("label_files", len(labels))
if labels:
    line = labels[0].read_text().strip().splitlines()[0]
    print("first_label_values", len(line.split()))
PY
```
Quick-check label structure availability.

## P01.03 Lightning.ai Environment Checklist
1. Confirm GPU runtime is enabled in Lightning Studio.
2. Confirm Python, ultralytics, torch, and torchvision versions are logged.
3. Confirm CUDA visibility inside runtime.
4. Confirm writable output directory for run artifacts.
5. Confirm dataset location mounted/accessible in runtime.

Environment command examples:
```bash
python -c "import torch; print(torch.cuda.is_available(), torch.cuda.device_count())"
```
Check GPU visibility.

```bash
python -c "import ultralytics, torch; print('ultralytics', ultralytics.__version__, 'torch', torch.__version__)"
```
Log package versions.

## Training Command Templates
Primary smoke run (`yolo26n-pose`):
```bash
yolo pose train model=yolo26n-pose.pt data=hand-keypoints.yaml imgsz=640 epochs=5 batch=16 device=0 workers=8 project=runs/p01 name=y26n_smoke
```

Primary main run (`yolo26n-pose`):
```bash
yolo pose train model=yolo26n-pose.pt data=hand-keypoints.yaml imgsz=640 epochs=60 patience=20 batch=16 device=0 workers=8 project=runs/p01 name=y26n_main
```

Fallback smoke/main template (`yolov8n-pose`):
```bash
yolo pose train model=yolov8n-pose.pt data=hand-keypoints.yaml imgsz=640 epochs=5 batch=16 device=0 workers=8 project=runs/p01 name=y8n_smoke
```

```bash
yolo pose train model=yolov8n-pose.pt data=hand-keypoints.yaml imgsz=640 epochs=60 patience=20 batch=16 device=0 workers=8 project=runs/p01 name=y8n_main
```

## Logging Requirements
For every run, capture:
1. Full command string.
2. Runtime environment metadata (GPU type, package versions).
3. Final metrics and checkpoint path.
4. Short qualitative note (what looked right/wrong).
