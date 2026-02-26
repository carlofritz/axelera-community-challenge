# Pose Trigger Software (Loose Plan)

## Goal
After hand keypoint detection is running, enable user-defined gesture triggers:
1. User provides 1-10 reference images for a target pose.
2. System learns a pose template from those references.
3. System fires a trigger when live pose matches template.

## Scope For V0
1. Single hand trigger recognition.
2. Static pose matching (not full dynamic gesture sequence yet).
3. Configurable trigger threshold and cooldown.

## Out Of Scope For V0
1. Complex temporal gesture grammar.
2. Multi-user identity tracking.
3. Production-grade calibration UI.

## Proposed Pipeline
1. Input frame -> hand detector + 21 keypoints.
2. Keypoint normalization:
   - translation normalize (center to wrist or palm center),
   - scale normalize (hand size),
   - optional rotation alignment.
3. Template creation from reference images:
   - run detection on each reference,
   - keep valid samples,
   - aggregate into one or more template vectors.
4. Runtime matching:
   - compute similarity score between live keypoints and template(s),
   - apply visibility-aware weighting,
   - smooth scores across consecutive frames.
5. Trigger logic:
   - trigger on `N` consecutive matches above threshold,
   - enforce cooldown to avoid repeated spam events.

## Data Structures (Draft)
1. `TriggerProfile`
   - `id`, `name`, `enabled`
   - `hand_mode` (`left`, `right`, `any`)
   - `threshold`
   - `min_consecutive_frames`
   - `cooldown_ms`
   - `templates`
2. `PoseTemplate`
   - `source_id`
   - `normalized_keypoints` (21x3)
   - `visibility_mask`
3. `RuntimeTriggerState`
   - `current_streak`
   - `last_score`
   - `cooldown_until`

## Matching Strategy (V0)
1. Baseline scorer:
   - weighted L2 distance on normalized `(x, y)` for visible points.
2. Optional improvement:
   - Procrustes alignment before scoring for better rotation robustness.
3. Decision:
   - convert distance to confidence score,
   - compare to threshold.

## Trigger Output Contract (Draft)
1. `trigger_id`
2. `timestamp`
3. `score`
4. `hand_bbox`
5. `keypoints`
6. `frame_index`

## Minimal UX Flow
1. User creates trigger name.
2. User uploads 1-10 reference images.
3. System previews extracted landmarks and accepted samples.
4. User sets threshold (or uses suggested default).
5. User starts live detection and sees trigger events.

## Evaluation Plan
1. Positive set: pose intended to trigger.
2. Negative set: similar but non-target poses.
3. Metrics:
   - precision/false-trigger rate,
   - recall/missed-trigger rate,
   - trigger latency (frames to fire).

## Risks
1. Left/right ambiguity and mirrored camera feeds.
2. Occlusion and low visibility keypoints.
3. Thresholds overfitting to a small reference set.

## Next Design Step
1. Finalize normalization and scoring formula.
2. Define JSON schema for `TriggerProfile`.
3. Build a small offline evaluator before live integration.
