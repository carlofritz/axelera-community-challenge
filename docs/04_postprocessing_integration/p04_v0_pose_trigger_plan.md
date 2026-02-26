# P04 V0 Pose Trigger Plan

## Summary
Build a lean post-processing module that runs on Orange Pi with the inference loop, lets a user define static hand-pose triggers from live capture (1-10 samples), and emits webhook/log events when a pose is recognized.

Primary objective:
1. Reliable static-pose trigger detection.
2. Fast setup via local webapp.
3. Clear extension path to dynamic gestures later.

## V0 Scope
1. Static pose triggers only.
2. Any-hand matching by default.
3. Local webapp + backend on LAN.
4. Same-process runtime topology (with clean boundaries for later service split).
5. Trigger outputs via webhook and local event log.

Out of scope:
1. Sequence/dynamic gesture recognition.
2. Multi-user tracking.
3. Cloud deployment/security hardening beyond local LAN + optional token.

## Expected Runtime Input
Per frame:
1. Timestamp/frame index.
2. Detected hands list.
3. For each hand:
   - bbox
   - detection confidence
   - keypoints `(21,3)` in `(x,y,visibility)` format.

Internal normalized contract (`PoseFrame`):
1. `frame_index: int`
2. `timestamp_ms: int`
3. `hands: PoseHand[]`

`PoseHand`:
1. `bbox_xyxy: float[4]`
2. `det_conf: float`
3. `kpts: float[21][3]`
4. `hand_side_estimate: "unknown" | "left" | "right"`

## Core Data Types
`TriggerProfile`:
1. `id: str`
2. `name: str`
3. `enabled: bool`
4. `hand_mode: "any" | "left" | "right"`
5. `threshold: float`
6. `min_consecutive_frames: int`
7. `cooldown_ms: int`
8. `templates: PoseTemplate[]`
9. `created_at_ms: int`
10. `updated_at_ms: int`

`PoseTemplate`:
1. `id: str`
2. `source: "live_capture"`
3. `sample_count: int`
4. `normalized_kpts_xy: float[21][2]`
5. `visibility_mask: bool[21]`
6. `quality_score: float`

`RuntimeTriggerState`:
1. `trigger_id: str`
2. `current_streak: int`
3. `last_score: float`
4. `cooldown_until_ms: int`

`TriggerEvent`:
1. `event_id: str`
2. `trigger_id: str`
3. `trigger_name: str`
4. `timestamp_ms: int`
5. `score: float`
6. `frame_index: int`
7. `hand_index: int`
8. `bbox_xyxy: float[4]`
9. `model_tag: str`

## API Contract (FastAPI)
1. `GET /api/health`
2. `GET /api/triggers`
3. `POST /api/triggers`
4. `PUT /api/triggers/{id}`
5. `DELETE /api/triggers/{id}`
6. `POST /api/triggers/{id}/capture`
   - body: `{ "samples": 1..10 }`
7. `GET /api/events?limit=<n>`
8. `POST /api/settings/webhook`
9. `GET /api/preview.mjpg` (or simple stream equivalent)

## Persistence (V0)
1. `artifacts/04_postprocessing_integration/trigger_profiles/profiles.json`
2. `artifacts/04_postprocessing_integration/events/events.ndjson`
3. Optional per-trigger template files under `artifacts/04_postprocessing_integration/templates/`.

## Processing Pipeline
### Stage P04.01: Adapter
1. Read runtime keypoint output each frame.
2. Map to `PoseFrame` schema.
3. Reject malformed hand detections (non-21 keypoint shape).

### Stage P04.02: Normalize + Score
1. Candidate filter:
   - `det_conf >= 0.25`
   - visible keypoints count `>= 12`
2. Normalize:
   - translate around wrist (`kp0`)
   - scale by wrist-to-middle_mcp (`kp9`) with bbox fallback
   - optional rotation alignment by wrist->middle_mcp vector
3. Score:
   - visibility-weighted L2 distance on normalized `(x,y)`
   - convert to confidence score
4. Select best matching template per trigger.

### Stage P04.03: Trigger Engine
1. Match when `score >= threshold`.
2. Require `min_consecutive_frames` streak.
3. Enforce `cooldown_ms`.
4. Emit:
   - webhook POST
   - `events.ndjson` append
   - in-app event feed update.

### Stage P04.04: Webapp
1. Trigger CRUD page.
2. Live capture burst page (1-10 samples).
3. Event feed page.
4. Settings page (webhook + optional token).

## Default Parameters (V0)
1. `threshold = 0.72`
2. `min_consecutive_frames = 3`
3. `cooldown_ms = 1000`
4. `det_conf_min = 0.25`
5. `min_visible_points = 12`
6. `webhook_timeout_ms = 1000`
7. `webhook_retry_count = 2`

## Failure Handling
1. No hand detected: reset streak.
2. Low-visibility frame: skip scoring.
3. Webhook failure: retry twice, then log failure and continue.
4. Corrupted profiles file: load backup snapshot and continue.
5. Performance pressure: lower preview rate before reducing trigger evaluation rate.

## V0 Acceptance Criteria
1. User can create a trigger from live capture in webapp.
2. Trigger fires on intended static pose.
3. Trigger events appear in UI and webhook payloads.
4. Config persists after restart.
5. Runtime operates on Orange Pi in same-process mode.

## Test Plan
Unit tests:
1. Normalization invariance (translation/scale/rotation).
2. Visibility-aware scoring behavior.
3. Trigger streak + cooldown logic.

Integration tests:
1. Capture 1/5/10 samples and template creation.
2. Positive/negative pose sets.
3. Webhook success/retry behavior.
4. Restart persistence checks.

## Rollout Sequence
1. Offline matcher prototype with recorded keypoints.
2. Runtime adapter integration.
3. Trigger engine integration.
4. Webapp capture + CRUD.
5. Webhook + event log integration.
6. Field tuning on real camera setup.

## Dynamic Gesture Expansion (Post-V0)
1. Add `match_mode` in profile: `"static"` or `"sequence"`.
2. Add ordered pose-state sequence schema.
3. Add temporal window + state machine matcher.
4. Keep same event output contract for backward compatibility.
