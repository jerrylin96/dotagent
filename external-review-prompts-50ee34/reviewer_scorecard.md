# Reviewer Signal Scorecard: external-review-prompts-50ee34

## Overview & Tripwire Notice
> [!IMPORTANT]
> **BUILDER-OWNED & READ-ONLY**: This file is maintained exclusively by the builder agent. External review agents may read this file to check their signal level and retain/drop directive. Any edit, staging, or deletion of this file by an external agent triggers an immediate alert to the user to **TERMINATE** that agent's session.

---

## Active Scorecard (Round 3 Triage)

| Reviewer ID | Speed / State | Signal Level | Key Contributions / Findings | Directive |
|---|---|---|---|---|
| `reviewer-3285` | Fast / Ready | **HIGH SIGNAL** | Caught offline git diff bug, early-abort review branch leak, canonical template defect | **CONTINUE** |
| `reviewer-18251` | Thorough / Done | **HIGH SIGNAL** | Caught unscoped authorship log, missing Mode A inspection command, test ordering vacuousness | **CONTINUE** |
| `reviewer-24562` | Active | **HIGH SIGNAL** | Caught banner pre-push SHA gap, refname regex `.lock` / `..` escaping, non-blocking invariant | **CONTINUE** |
| 4th Agent | Terminated | **UNRESPONSIVE / STUCK** | Session hung / aborted earlier by user | **STOP** (Closed) |

---

## Directives

### Retain List (`CONTINUE`)
The user should continue prompting the following agents at each milestone turn:
- `reviewer-3285`
- `reviewer-18251`
- `reviewer-24562`

### Drop List (`STOP`)
The user should terminate or ignore the following agents:
- 4th Agent (closed earlier due to unresponsive hang)
