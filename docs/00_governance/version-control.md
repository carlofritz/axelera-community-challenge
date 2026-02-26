# Version Control Flow

## Branch Model
- Protected baseline branch: `main`
- Working branches: `feature/<topic>` (example: `feature/p01-dataset-schema`)
- Optional urgent fixes: `hotfix/<topic>`

## Commit Rules
- Keep commits small and single-purpose.
- Use message format:
  - `feat(scope): ...`
  - `fix(scope): ...`
  - `docs(scope): ...`
  - `chore(scope): ...`
- Include step ID in commit body when relevant (example: `Step: P01.02`).

## Merge Rules
- Do not commit directly to `main` for feature work.
- Merge via pull request after review.
- Prefer squash merge for clean history unless preserving granular history is required.

## Tags And Milestones
- Tag completed phases:
  - `phase-p01-ready`
  - `phase-p02-ready`
  - etc.
- Use semantic release notes per phase with:
  - scope
  - acceptance result
  - known risks

## GitHub Repository Settings (Recommended)
- Default branch: `main`
- Require PR before merge.
- Require at least 1 review.
- Require branch to be up to date before merge.
- Block force pushes to `main`.

## Local Working Cycle
1. Sync `main`.
2. Create feature branch from `main`.
3. Implement one scoped change.
4. Commit with clear message and step ID.
5. Push branch and open PR.
6. Merge after review and checks.
7. Update `plan.md` current step/progress log.
