# Feature Specification: Post-Push External Review Prompts & Autonomous Triage Protocol

## 1. Problem Statement & Motivation
During the `/make-feature` lifecycle (and standalone adversarial reviews), the agent commits and pushes code milestones (spec, plan, failing RED tests, GREEN implementation) to an isolated remote branch (`origin/gemini/<feature-name>-<hash>`).

Currently:
1. **Context Bloat**: Users running independent external review agents (e.g., Claude, ChatGPT, Cursor, GitHub bots) paste multi-page conversational reviews into the active chat session. Once addressed, this verbose text persists indefinitely, diluting context window quality.
2. **Review Noise & YAGNI Violations**: External agents lack full project history and frequently propose speculative abstractions, extraneous wrappers, or stylistic bikeshedding that violate the **Ponytail (Lazy Senior Dev Mode)** principle.
3. **Manual Overhead**: Users must manually formulate prompts containing branch names, diff ranges, review lenses, and output formatting rules after every push.

## 2. Goals & Non-Negotiables
- **Post-Push Prompt Generation**: Automatically emit a tailored, copy-pasteable review prompt immediately after every milestone push (`spec`, `plan`, `test: add RED test suite`, `feat: implement GREEN code`, and per-slice pushes in Heavy Mode).
- **Strict Low-Token Output Contract**: Prompts must instruct external agents to output strictly structured, token-dense summaries (compact Markdown table or JSON array) with zero conversational filler or greetings.
- **Dual Ingestion Channels**: Support both direct chat paste (compact table only) and zero-bloat file ingestion via `<brain>/scratch/external_reviews.md` read via `view_file`.
- **Autonomous Agent Triage (Ponytail Defense)**: The builder agent has explicit authority to triage external findings against the Ponytail Senior Dev ladder—accepting valid defects/security bugs while rejecting unrequested abstractions or hallucinations with a terse 1-line rationale.

## 3. Detailed Workflow & Prompt Templates

### 3.1 Push-Cadence Triggers
After every successful push to `origin/${BRANCH_NAME}`:
1. Phase 1a Step 2 / 2b: Spec push (`spec: add initial feature spec...`).
2. Phase 1b Step 3 / 3b: Plan push (`plan: add implementation plan...`).
3. Phase 2 Step 4d: RED test push (`test: add RED test suite (failing)`).
4. Phase 2 Step 5 / Phase 3 Step 6: GREEN code push (`feat: implement feature...`).
5. Heavy Mode: After each slice RED test push and each slice GREEN code push.

### 3.2 Standard Prompt Template Structure
Every emitted prompt must specify:
1. **Target**: `origin/${BRANCH_NAME}`
2. **Base**: `origin/${BASE_BRANCH}`
3. **Diff / File Target**: Link to specific file or GitHub compare URL.
4. **Targeted Review Lens**: Milestone-specific audit focus (scope/assumptions for spec; atomic ordering for plan; assertion rigor/failure reason for RED tests; logic/security/YAGNI for GREEN code).
5. **Strict Output Format Contract**:
   ```markdown
   Output ONLY a markdown table with columns:
   | Severity (P0/P1/P2/Nit) | File:Line | Defect / Risk | Concrete Minimal Fix |
   Do NOT include introductions, summaries, or conversational text. If clean, output only: "VERDICT: CLEAN"
   ```

### 3.3 Autonomous Triage Protocol
When the user supplies external review feedback:
1. The builder agent reads feedback from chat or `scratch/external_reviews.md`.
2. The agent outputs an **External Review Triage Matrix**:
   - `ACCEPT`: Real defect, logic bug, security issue, boundary error, or broken spec invariant -> execute fix.
   - `REJECT`: Speculative abstraction, unneeded interface/factory, style preference, hallucinated API, or contradiction of agreed spec -> reject with 1-line Ponytail rationale.
3. If changes were made, stage, commit, and push per lifecycle rules.

## 4. Documentation & Skill Integration Points
- Update `~/.gemini/skills/make-feature/SKILL.md`: Add step requirements to emit external review prompt blocks after each `git push`, document dual ingestion paths, and formalize the Ponytail triage matrix.
- Update `~/.gemini/skills/adversarial-review/SKILL.md`: Standardize external review prompt output contracts and triage guidelines.
- Update `~/.gemini/AGENTS.md` (and `GEMINI.md`): Update §3 Mandatory Execution Pipeline to mention post-push review prompt emission and autonomous triage gate.
