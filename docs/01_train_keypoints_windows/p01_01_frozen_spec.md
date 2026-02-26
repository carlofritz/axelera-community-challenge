# P01.01 Frozen Specification

Date frozen: `2026-02-26`

## Scope
Freeze task definition for initial hand-keypoint demo path before dataset/config execution work.

## Task Contract
1. Task type: single-stage pose model (boxes + keypoints).
2. Landmark format: 21 keypoints per hand.
3. Keypoint dimensions: `(x, y, visibility)` -> `kpt_shape: [21, 3]`.

## Keypoint Order (Fixed)
0. `wrist`
1. `thumb_cmc`
2. `thumb_mcp`
3. `thumb_ip`
4. `thumb_tip`
5. `index_finger_mcp`
6. `index_finger_pip`
7. `index_finger_dip`
8. `index_finger_tip`
9. `middle_finger_mcp`
10. `middle_finger_pip`
11. `middle_finger_dip`
12. `middle_finger_tip`
13. `ring_finger_mcp`
14. `ring_finger_pip`
15. `ring_finger_dip`
16. `ring_finger_tip`
17. `pinky_mcp`
18. `pinky_pip`
19. `pinky_dip`
20. `pinky_tip`

## Success Criteria (Demo Bar)
Quantitative minimums:
1. `metrics/pose(mAP50-95) >= 0.30`
2. `metrics/pose(mAP50) >= 0.60`

Qualitative minimum:
1. On a sampled 100-frame validation clip, at least 85 frames show one hand with stable, plausible landmark geometry.

## Model Policy
1. Primary model: `yolo26n-pose`.
2. Fallback model: `yolov8n-pose`.

## Hard Fallback Gates
Switch to fallback model if any is true:
1. Two focused primary-path incompatibility failures.
2. No first correct overlay on Metis by the end of Phase D timebox.
3. Primary model cannot be deployed through planned Voyager flow within sprint constraints.

## Required Handoff Artifacts (for P01.05)
1. `best.pt`
2. Exact dataset yaml used.
3. Training commands and hyperparameters.
4. Metrics snapshot and sample predictions.
