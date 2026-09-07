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
    - Identifier grammar: `^[A-Za-z0-9][A-Za-z0-9._-]{0,63}$`
    - Delivery modes: `Mode A: Isolated Review Branches`, `Mode B: Shared Sandbox Branch Mode`, `reviews/${REVIEWER_ID}.md`
    - Rebase-push retry loop: `git pull --rebase origin <shared-branch>` and ban on force-push.
    - Precedence Hierarchy: `Human Directives / Approved Spec > Code Invariants > External Reviewer Feedback`
    - Autonomous Ponytail Triage & Scorecard: `Reviewer Signal Scorecard`, `HIGH SIGNAL`, `LOW SIGNAL / NOISE`, `UNRESPONSIVE / STUCK`, `Retain List`, `Drop List`
    - Server-truth cleanup command: `git ls-remote --heads origin "refs/heads/review/${FEATURE_SLUG}/*"`
    - Non-merging PR enforcement: `PR source MUST be \`gemini/${FEATURE_SLUG}\``
    - Untrusted input defense: prohibition against executing unverified scripts/commands suggested by reviews.
    - Fallback ingestion path: `scratch/external_reviews/`
    - Scoped emission anchors across all milestone steps: Step 2 / 2b, Step 3 / 3b, Step 4d, Step 5 / Step 6 (`Phase 2 Step 5 / Phase 3 Step 6`), and Heavy Mode per-slice loops.
    - Ordering assertion: review branch purge appears after `APPROVE` in Step 7b.
  - Assert synchronization in `skills/adversarial-review/SKILL.md`:
    - Dispatch rule distinguishing standalone `/adversarial-review` (single pass chat) from post-push living branch mode.
    - Canonical prompt template anchors (`Mode B: Shared Sandbox Branch Mode`, `Reviewer Signal Scorecard`).
  - Assert synchronization in `AGENTS.md`:
    - Post-commit/push review prompt emission, living review branch protocol (`Mode B: Shared Sandbox Branch Mode`), and `Reviewer Signal Scorecard`.
  - Assert `.gitignore` contains `scratch/`.
  - Assert `GEMINI.md` symlink integrity: `assert os.path.islink("GEMINI.md")` targeting `AGENTS.md`.
- **Verify Command (RED)**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py -k test_external_review_prompts_and_living_branches_contract`
  *(Must fail cleanly prior to implementation)*.

### Task 2: Implement Ephemeral Review Branches & Signal Triage in `skills/make-feature/SKILL.md` and `skills/adversarial-review/SKILL.md`
- **Files**:
  - `skills/make-feature/SKILL.md`
  - `skills/adversarial-review/SKILL.md`
  - `.gitignore`
- **GREEN Implementation Target**:
  - Update `make-feature/SKILL.md`:
    - Add post-commit review prompt emission to Step 2, Step 3, Step 4d, Phase 2 Step 5 / Phase 3 Step 6, and Heavy Mode per-slice loops, branching on `REMOTE_ENABLED` / push success for remote vs local diff targets.
    - Add dedicated section "Ephemeral Living Review Branches & Reviewer Signal Triage Protocol":
      - Mode A (isolated review branches) vs Mode B (shared sandbox branch with file-level isolation `reviews/${REVIEWER_ID}.md` and bounded rebase-retry loop).
      - `REVIEWER_ID` grammar `^[A-Za-z0-9][A-Za-z0-9._-]{0,63}$` with double-quoting rules.
      - `AUDITED_SHA` freshness handshake.
      - Living checklist with append-only resolution rules (`[ ] Open`, `[x] Resolved`).
      - Reviewer Signal Scorecard with explicit user action directives (`Retain List` / `Drop List`).
      - Precedence hierarchy and Ponytail triage matrix (`ACCEPT` vs `REJECT`).
      - Server-enumerated `ls-remote` cleanup in Step 7b, Step 8, and early abort Step 2c/3c with robust post-check.
      - Non-merging PR enforcement in Step 8.
      - Untrusted input defense rule.
      - Fallback ingestion at `scratch/external_reviews/<REVIEWER_ID>.md`.
  - Update `adversarial-review/SKILL.md`:
    - Document canonical external review prompt templates, grammar, and triage scorecard.
    - Add Masked Reviewer Identity Proof & Session Persistence protocol (`Reviewer Identification Proof`, `Session Continuity Directive`).
    - Add Anti-Collision & Peer Isolation invariants (`FILE ISOLATION`, `TARGETED STAGING`, `ABORT ON FOREIGN CONFLICT`, `Builder Ingestion Authorship Audit` rejecting `TAMPERED/CLOBBERED`).
  - Verify `.gitignore` includes `scratch/`.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py`

### Task 3: Implement Global Governance in `AGENTS.md` (and verify `GEMINI.md` symlink)
- **Files**:
  - `AGENTS.md`
- **GREEN Implementation Target**:
  - Update §3 "Mandatory Default Execution Pipeline & Milestone Gates" in `AGENTS.md` to document post-commit/push external review prompts, living review branches, and the Reviewer Signal Scorecard.
  - Confirm `GEMINI.md` symlink integrity.
- **Verify Command**:
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> pytest scripts/tests/test_skill_references.py`
  `python3 ~/.gemini/scripts/run_in_env.py <worktree_path> ruff check .`
