# Implementation Plan: Post-Push External Review Prompts, Ephemeral Living Review Branches & Reviewer Signal Triage

## Execution Strategy
- [x] **Single Active Writer**: Single builder agent executes tasks in sequence within the isolated worktree.
- [ ] **Sequential Subagents**: (Not required for documentation/skill updates under 5 slices).

---

## Task Decomposition

### Task 1: Add Automated Regression & Contract Tests (TDD RED Test Suite)
- **Files**: `scripts/tests/test_skill_references.py`
- **RED Test Spec**:
  - Add `test_external_review_prompts_and_living_branches_contract()` to `scripts/tests/test_skill_references.py`.
  - Assert exact literal anchors in `skills/make-feature/SKILL.md`:
    - Branch naming & format: `review/${FEATURE_SLUG}/${REVIEWER_ID}` and `AUDITED_SHA:`
    - Identifier grammar: `^[A-Za-z0-9._-]+$`
    - Precedence Hierarchy: `Human Directives / Approved Spec > Code Invariants > External Reviewer Feedback`
    - Autonomous Ponytail Triage & Scorecard: `Reviewer Signal Scorecard`, `HIGH SIGNAL`, `LOW SIGNAL / NOISE`, `UNRESPONSIVE / STUCK`, `Retain List`, `Drop List`
    - Whitespace-safe cleanup command: `git for-each-ref --format='%(refname:strip=3)' "refs/remotes/origin/review/${FEATURE_SLUG}/*"`
    - Non-merging PR enforcement: `PR source MUST be \`gemini/${FEATURE_SLUG}\``
    - Untrusted input defense: prohibition against executing unverified scripts/commands suggested by reviews.
    - Offline fallback: `REMOTE_ENABLED=false` local diff handling.
    - Scoped emission anchors across all 4 milestone steps (Step 2, Step 3, Step 4d, Step 6) and Heavy Mode.
  - Assert synchronization in `skills/adversarial-review/SKILL.md` and `AGENTS.md`.
  - Assert `GEMINI.md` symlink integrity: `assert os.path.islink("GEMINI.md")` targeting `AGENTS.md`.
- **Verify Command (RED)**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py -k test_external_review_prompts_and_living_branches_contract`
  *(Must fail cleanly prior to implementation)*.

### Task 2: Implement Ephemeral Review Branches & Signal Triage in `skills/make-feature/SKILL.md` and `skills/adversarial-review/SKILL.md`
- **Files**:
  - `skills/make-feature/SKILL.md`
  - `skills/adversarial-review/SKILL.md`
  - `.gitignore` (add `scratch/`)
- **GREEN Implementation Target**:
  - Update `make-feature/SKILL.md`:
    - Add post-push review prompt emission to Step 2, Step 3, Step 4d, Step 6, and Heavy Mode per-slice loops.
    - Add dedicated section "Ephemeral Living Review Branches & Reviewer Signal Triage Protocol":
      - `review/${FEATURE_SLUG}/${REVIEWER_ID}` branch naming with `^[A-Za-z0-9._-]+$` grammar.
      - `AUDITED_SHA` freshness handshake.
      - Living `review.md` checklist with append-only resolution rules (`[ ] Open`, `[x] Resolved`).
      - Reviewer Signal Scorecard with explicit user action directives (`Retain List` / `Drop List`).
      - Precedence hierarchy and Ponytail triage matrix (`ACCEPT` vs `REJECT`).
      - Whitespace-safe `for-each-ref` cleanup in Step 7b, Step 8, and early abort Step 2c/3c.
      - Non-merging PR enforcement in Step 8.
      - Untrusted input defense rule.
      - Fallback ingestion at `scratch/external_reviews/<REVIEWER_ID>.md`.
  - Update `adversarial-review/SKILL.md`:
    - Document canonical external review prompt templates, grammar, and triage scorecard.
  - Update `.gitignore` to include `scratch/` to prevent untracked reviewer pastes from being committed.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py`

### Task 3: Implement Global Governance in `AGENTS.md` (and verify `GEMINI.md` symlink)
- **Files**:
  - `AGENTS.md`
- **GREEN Implementation Target**:
  - Update §3 "Mandatory Default Execution Pipeline & Milestone Gates" in `AGENTS.md` to document post-push external review prompts, living review branches, and the Reviewer Signal Scorecard.
  - Confirm `GEMINI.md` symlink integrity.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py`
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> ruff check .`
