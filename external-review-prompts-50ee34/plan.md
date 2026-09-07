# Implementation Plan: Post-Push External Review Prompts & Autonomous Triage Protocol

## Execution Strategy
- [x] **Single Active Writer**: Single builder agent executes tasks in sequence within the isolated worktree.
- [ ] **Sequential Subagents**: (Not required for documentation/skill updates under 5 slices).

---

## Task Decomposition

### Task 1: Add Automated Regression & Contract Tests (TDD RED Test Suite)
- **Files**: `scripts/tests/test_skill_references.py`
- **RED Test Spec**:
  - Add `test_external_review_prompts_and_triage_contract()` to `scripts/tests/test_skill_references.py`.
  - Assert presence of post-push prompt generation in `skills/make-feature/SKILL.md` across all 4 milestone push gates (spec, plan, test, code).
  - Assert presence of strict low-token output contract (Markdown table with `Severity`, `File:Line`, `Defect`, `Minimal Fix`).
  - Assert presence of Precedence Hierarchy (`Human Directives / Approved Spec > Code Invariants > External Reviewer Feedback`).
  - Assert presence of Autonomous Triage Protocol (`ACCEPT` vs `REJECT` with 1-line Ponytail rationale).
  - Assert presence of dual ingestion paths (chat table paste vs `<brain>/scratch/external_reviews.md`).
  - Assert presence of offline fallback (`REMOTE_ENABLED=false`) and non-blocking asynchronous invariant.
  - Assert corresponding synchronization in `skills/adversarial-review/SKILL.md` and `AGENTS.md`.
- **Verify Command (RED)**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py -k test_external_review_prompts_and_triage_contract`
  *(Must fail behavioral assertion prior to implementation)*.

### Task 2: Implement Post-Push Prompts & Autonomous Triage in `skills/make-feature/SKILL.md` and `skills/adversarial-review/SKILL.md`
- **Files**:
  - `skills/make-feature/SKILL.md`
  - `skills/adversarial-review/SKILL.md`
- **GREEN Implementation Target**:
  - Update `make-feature/SKILL.md`:
    - Phase 1a Step 2 / 2b: Add prompt emission block for Spec review.
    - Phase 1b Step 3 / 3b: Add prompt emission block for Plan review.
    - Phase 2 Step 4d: Add prompt emission block for RED test review.
    - Phase 3 Step 6: Add prompt emission block for GREEN code review.
    - Add Heavy Mode per-slice prompt emission instructions.
    - Add dedicated subsection for "Post-Push External Review Prompts & Autonomous Triage Protocol" detailing strict output contract, precedence rules, Ponytail triage matrix, and dual ingestion paths.
  - Update `adversarial-review/SKILL.md`:
    - Add standard copy-paste external review prompt templates and autonomous triage guidelines.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py`

### Task 3: Implement Global Guidelines in `AGENTS.md` (and verify `GEMINI.md` symlink)
- **Files**:
  - `AGENTS.md`
- **GREEN Implementation Target**:
  - Update §3 "Mandatory Default Execution Pipeline & Milestone Gates" in `AGENTS.md` to formally document post-push external review prompt emission and builder agent autonomous triage.
  - Verify `GEMINI.md` symlink remains intact and functional.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 pytest scripts/tests/test_skill_references.py`
  `python3 ~/.gemini/scripts/run_in_env.py /Users/jerrylin/.gemini/tmp/worktrees/gemini_external-review-prompts-50ee34 ruff check .`
