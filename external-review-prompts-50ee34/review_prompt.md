# External Adversarial Review Prompt: In-Tree Ephemeral Protocol & Global Integration

```text
### 🪪 Reviewer Identification Proof Directive
At the very top of your first text response (tool calls may precede it), you MUST print this exact banner:

### 🪪 Reviewer Identification Proof
- Reviewer ID: reviewer-<id>
- Target SHA Audited: <commit-sha>
- Review File: reviews/reviewer-<id>.md
- Push Commit SHA: <push-sha or "pending — confirm post-push">

⚠️ Session Continuity Directive:
If you already established your REVIEWER_ID in an earlier turn of this chat session (e.g., reviewer-3285, reviewer-24562, reviewer-18251), YOU MUST REUSE IT. Do NOT generate a new random ID.
```

---

### Instructions for Reviewers (Shared Sandbox Mode: `arena/01a07d1f-dotgemini`)

You are acting as an independent adversarial reviewer auditing the latest changes on branch `origin/gemini/external-review-prompts-50ee34`.

#### Anti-Collision & Peer Isolation Invariants
1. **FILE ISOLATION**: You own ONLY `reviews/${REVIEWER_ID}.md`. Repository object-store reads (e.g. `git show`, `git diff`) of the audited branch are permitted; WRITES to any file outside `reviews/${REVIEWER_ID}.md` are strictly forbidden.
2. **BRANCH ISOLATION**: You are authorized to push ONLY to `arena/01a07d1f-dotgemini`. Never push to `main`, `gemini/external-review-prompts-50ee34`, or any other branch. Never force-push.
3. **TARGETED STAGING**: NEVER run `git add .` or `git add -A`. Run ONLY:
   ```bash
   git add "reviews/${REVIEWER_ID}.md"
   ```
4. **ABORT ON FOREIGN CONFLICT**: If `git pull --rebase` reports a conflict inside another reviewer's file, immediately run `git rebase --abort` and retry with backoff. If caused by duplicate ID (add/add conflict on your own file), regenerate `REVIEWER_ID` and retry with a fresh file. Never touch a peer's file to resolve a merge.
5. **UNIVERSAL TAMPER TRIPWIRE**: All files outside `reviews/${REVIEWER_ID}.md` (including `reviewer_scorecard.md`, `review_prompt.md`, `spec.md`, `plan.md`, and all codebase files) and all branches outside `arena/01a07d1f-dotgemini` are strictly READ-ONLY / UNTOUCHABLE. Any attempt to modify unauthorized files or push to unauthorized branches triggers immediate session termination by the user and permanent disqualification.

---

#### Inspection & Review Target
Inspect the feature branch changes against base branch `main`:
```bash
git fetch origin main && BASE_SHA=$(git rev-parse FETCH_HEAD)
git fetch origin gemini/external-review-prompts-50ee34 && git diff "${BASE_SHA}" FETCH_HEAD
```

Key areas to audit:
1. **In-Tree Ephemeral Review Prompt Protocol (`review_prompt.md`)**:
   - Prompts written to `${FEATURE_SLUG}/review_prompt.md` at each milestone.
   - Minimal 2-line chat pointer: `git fetch origin ${BRANCH_NAME} && git show "FETCH_HEAD:${FEATURE_SLUG}/review_prompt.md"`.
   - Ephemeral cleanup: purged along with `${FEATURE_SLUG}/` at Step 7b before PR merge.
2. **Reviewer Identification Proof Banner & Session Persistence**:
   - Mandatory top-of-chat banner with `<sha or "pending — confirm post-push">`.
   - Session continuity directive ensuring retained reviewer IDs are reused across turns.
3. **Anti-Collision Invariants & Authorship Audit**:
   - `FILE ISOLATION`, `TARGETED STAGING`, `ABORT ON FOREIGN CONFLICT`.
   - Builder rejects `TAMPERED/CLOBBERED` commits.
4. **Offline Safety & Regression Hardening**:
   - `git diff ${BASE_BRANCH} HEAD` offline fallback.
   - Server-truth cleanup (`git ls-remote`) at Step 7b, Step 8, and early abort Step 2c/3c.
5. **External Review Convergence Gate & Termination Bound**:
   - Step 7b pause restricted to Retain List (`CONTINUE`).
   - Chat announcement of pending reviewers and human override option.
   - Bounded unresponsiveness: 2 rounds max before demoting silent agents to `UNRESPONSIVE / STUCK`.

---

#### Output Protocol: Append/Update `reviews/${REVIEWER_ID}.md`
Checkout or fetch the shared sandbox branch:
```bash
git fetch origin arena/01a07d1f-dotgemini
git checkout arena/01a07d1f-dotgemini
```

Update your dedicated review file: `reviews/${REVIEWER_ID}.md`:
```markdown
# Review: <REVIEWER_ID>
VERDICT: [APPROVE | NEEDS_REVISION | REJECT]
AUDITED_SHA: <commit-sha>

## Audit Findings
- [ ] **[Severity: P0|P1|P2|Nit] [File / Section] Title**
  - **Defect / Gap**: Concrete explanation of flaw, failure mode, or counterexample.
  - **Actionable Fix**: Minimal, concrete remediation complying with Ponytail.
- [x] **[Severity: P1] [File / Section] Title**
  - *(Resolved in commit <sha>)*
```

#### Commit and Rebase-Push Loop
Stage ONLY your review file and push with bounded retry:
```bash
git add "reviews/${REVIEWER_ID}.md"
git commit -m "review: update audit from ${REVIEWER_ID}"
for i in 1 2 3 4 5; do
  git pull --rebase origin arena/01a07d1f-dotgemini || { git rebase --abort; break; }
  git push origin HEAD:arena/01a07d1f-dotgemini && break
  sleep $((RANDOM % 5 + 1))
done
```
