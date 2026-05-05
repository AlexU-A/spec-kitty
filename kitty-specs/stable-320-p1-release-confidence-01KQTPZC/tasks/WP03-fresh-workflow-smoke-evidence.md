---
work_package_id: WP03
title: Fresh Workflow Smoke Evidence
dependencies:
- WP01
- WP02
requirement_refs:
- FR-010
- FR-011
- FR-012
- FR-013
- NFR-001
- NFR-002
- NFR-004
- C-001
- C-002
- C-003
planning_base_branch: main
merge_target_branch: main
branch_strategy: Planning artifacts for this mission were generated on main. During /spec-kitty.implement this WP may branch from a dependency-specific base, but completed changes must merge back into main unless the human explicitly redirects the landing branch.
subtasks:
- T013
- T014
- T015
- T016
- T017
- T018
agent: codex
history: []
agent_profile: python-pedro
authoritative_surface: kitty-specs/stable-320-p1-release-confidence-01KQTPZC/
execution_mode: planning_artifact
model: ''
owned_files:
- kitty-specs/stable-320-p1-release-confidence-01KQTPZC/smoke-evidence.md
- kitty-specs/stable-320-p1-release-confidence-01KQTPZC/smoke-artifacts/**
role: implementer
tags: []
---

# Work Package Prompt: WP03 - Fresh Workflow Smoke Evidence

## ⚡ Do This First: Load Agent Profile

Use the `/ad-hoc-profile-load` skill to load the agent profile specified in the frontmatter, and behave according to its guidance before parsing the rest of this prompt.

- **Profile**: `python-pedro`
- **Role**: `implementer`
- **Agent/tool**: `codex`

If no profile is specified, run `spec-kitty agent profile list` and select the best match for this work package's `task_type` and `authoritative_surface`.

---

## Objective

Run and document one fresh local-only lifecycle smoke and one SaaS-enabled lifecycle smoke for the current prerelease line. Every hosted auth, tracker, SaaS, or sync command on this computer must set `SPEC_KITTY_ENABLE_SAAS_SYNC=1`.

Implementation command: `spec-kitty agent action implement WP03 --agent <name>`

## Context

This WP depends on WP01 and WP02 because the smoke should run after the release-confidence fixes or evidence gates are in place. The WP is evidence-focused and owns mission-local smoke artifacts only. If a smoke uncovers a narrow code failure, record it and ask whether to widen or file/link an issue unless the fix is clearly within already-owned files from an earlier WP.

Branch strategy: planning artifacts were generated on `main`; completed changes must merge back into `main`. During implementation, Spec Kitty may allocate a worktree per computed lane from `lanes.json`; follow the workspace path provided by the implement command.

## Subtasks

### Subtask T013: Prepare disposable smoke workspace

**Purpose**: Keep smoke execution isolated from committed mission artifacts.

**Steps**:
1. Create a temporary workspace outside the mission artifact directory.
2. Record the path in `smoke-evidence.md`.
3. Confirm the CLI version under test from `uv run spec-kitty --version`.
4. Ensure the local-only smoke path has no hosted auth, tracker, SaaS, or sync dependency.

**Files**: write evidence only under `kitty-specs/stable-320-p1-release-confidence-01KQTPZC/smoke-evidence.md` and optional `smoke-artifacts/**`.

**Validation**: Smoke evidence names the workspace and version under test.

### Subtask T014: Run local-only lifecycle smoke

**Purpose**: Prove the core CLI lifecycle works without hosted services.

**Steps**:
1. Run init or project setup in the disposable workspace.
2. Run specify.
3. Run plan.
4. Run tasks.
5. Run implement/review or a bounded equivalent fixture path.
6. Run merge.
7. Record PR-ready branch evidence or explain the bounded equivalent used.

**Files**: evidence files only.

**Validation**: `smoke-evidence.md` includes commands, exit statuses, and key output summaries.

### Subtask T015: Prepare SaaS-enabled smoke path

**Purpose**: Select hosted/sync commands that exercise current CLI surfaces safely.

**Steps**:
1. Identify which commands touch hosted auth, tracker, SaaS, or sync.
2. Prefix every such command with `SPEC_KITTY_ENABLE_SAAS_SYNC=1`.
3. Do not claim tracker rollout gating exists.
4. Record any auth prerequisite or blocker precisely.

**Files**: evidence files only.

**Validation**: Every hosted/sync command in the evidence shows the env var.

### Subtask T016: Run SaaS-enabled smoke

**Purpose**: Prove hosted/sync paths behave on this machine's dev deployment path.

**Steps**:
1. Run selected hosted/sync commands with `SPEC_KITTY_ENABLE_SAAS_SYNC=1`.
2. Capture success, expected unauthenticated status, or actionable failure.
3. Avoid destructive hosted operations.
4. Preserve logs or summarized outputs in `smoke-artifacts/**` if useful.

**Files**: evidence files only.

**Validation**: Evidence distinguishes pass, expected auth limitation, issue-linked failure, or stable-release blocker.

### Subtask T017: Handle failures according to the contract

**Purpose**: Ensure failures are not buried.

**Steps**:
1. For each failure, decide whether it is narrow enough to fix in this mission.
2. If not fixed, file or reference a GitHub issue.
3. If it blocks stable, mark that explicitly.
4. If it is a bounded non-blocking limitation, explain why.

**Files**: evidence files only.

**Validation**: No smoke failure appears without issue/fix/blocker/defer status.

### Subtask T018: Finalize smoke evidence for WP04

**Purpose**: Make release evidence compilation straightforward.

**Steps**:
1. Summarize local-only result.
2. Summarize SaaS-enabled result.
3. List exact command sequence.
4. List artifacts and issue references.

**Files**: `smoke-evidence.md` and optional `smoke-artifacts/**`.

**Validation**: WP04 can cite the smoke evidence without rerunning commands.

## Definition of Done

- Local-only smoke is run and documented.
- SaaS-enabled smoke is run and documented with required env var use.
- Any failure has a fix, linked issue, or stable-release blocker decision.
- Evidence is committed in mission-owned files.

## Risks

- Hosted auth state may block parts of the SaaS smoke; record this as evidence rather than hiding it.
- Smoke commands may mutate temporary repos; keep them outside committed mission artifacts.
- Do not run destructive hosted operations.

## Reviewer Guidance

Reviewers should check that the local-only smoke truly avoids hosted dependencies and that every hosted/sync command in the SaaS-enabled smoke uses `SPEC_KITTY_ENABLE_SAAS_SYNC=1`.
