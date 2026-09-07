# Implementation Plan: Post-Push External Review Prompts & Ephemeral Living Review Branches

## Execution Strategy
- [x] **Single Active Writer**: Single builder agent executes tasks in sequence within the isolated worktree.
- [ ] **Sequential Subagents**: (Not required for documentation/skill updates under 5 slices).

---

## Task Decomposition

### Task 1: Add Automated Regression & Contract Tests (TDD RED Test Suite)
- **Files**: `scripts/tests/test_skill_references.py`
- **RED Test Spec**:
  - Add `test_external_review_prompts_and_living_branches_contract()` to `scripts/tests/test_skill_references.py`.
  - Assert presence of post-push prompt generation in `skills/make-feature/SKILL.md` across all 4 milestone push gates (spec, plan, test, code) and Heavy Mode.
  - Assert presence of Ephemeral Living Review Branch protocol (`review/${FEATURE_SLUG}/${REVIEWER_ID}` and `review.md` living status).
  - Assert presence of automated cleanup command for review branches in Step 7b (`git push origin --delete`).
  - Assert presence of Precedence Hierarchy (`Human Directives / Approved Spec > Code Invariants > External Reviewer Feedback`).
  - Assert presence of Autonomous Triage Protocol (`ACCEPT` vs `REJECT` with 1-line Ponytail rationale).
  - Assert presence of multi-tier ingestion paths (Git review branch, local scratch file, compact chat table).
  - Assert presence of offline fallback (`REMOTE_ENABLED=false`) and non-blocking asynchronous invariant.
  - Assert corresponding synchronization in `skills/adversarial-review/SKILL.md` and `AGENTS.md`.
- **Verify Command (RED)**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py -k test_external_review_prompts_and_living_branches_contract`
  *(Must fail cleanly prior to implementation)*.

### Task 2: Implement Ephemeral Review Branches & Post-Push Prompts in `skills/make-feature/SKILL.md` and `skills/adversarial-review/SKILL.md`
- **Files**:
  - `skills/make-feature/SKILL.md`
  - `skills/adversarial-review/SKILL.md`
- **GREEN Implementation Target**:
  - Update `make-feature/SKILL.md`:
    - Add post-push review prompt emission to Phase 1a Step 2 / 2b, Phase 1b Step 3 / 3b, Phase 2 Step 4d, Phase 3 Step 6, and Heavy Mode per-slice loops.
    - Add dedicated section "Ephemeral Living Review Branches & External Triage Protocol" detailing the `review/${FEATURE_SLUG}/${REVIEWER_ID}` branch naming, `review.md` living checklist, git fetch & inspection workflow, Ponytail triage matrix, and automated branch pruning in Step 7b.
  - Update `adversarial-review/SKILL.md`:
    - Document the ephemeral living review branch pattern and prompt templates.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py`

### Task 3: Implement Global Governance in `AGENTS.md` (and verify `GEMINI.md` symlink)
- **Files**:
  - `AGENTS.md`
- **GREEN Implementation Target**:
  - Update §3 "Mandatory Default Execution Pipeline & Milestone Gates" in `AGENTS.md` to document post-push external review prompts, ephemeral living review branches, and the autonomous Ponytail triage gate.
  - Verify `GEMINI.md` symlink integrity.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py`
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 ruff check .`
