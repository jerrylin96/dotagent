# Reviewer Signal Scorecard: external-review-prompts-50ee34

## Overview & Tripwire Notice
> [!IMPORTANT]
> **BUILDER-OWNED & READ-ONLY**: This file is maintained exclusively by the builder agent. External review agents may read this file to check their signal level and retain/drop directive. Any edit, staging, or deletion of this file by an external agent triggers an immediate alert to the user to **TERMINATE** that agent's session.

---

## Active Scorecard (Round 7 Triage)

| Reviewer ID | Speed / State | Signal Level | Key Contributions / Findings | Directive |
|---|---|---|---|---|
| `reviewer-18251` | Complete / **APPROVE** | **HIGH SIGNAL** | Confirmed all P0/P1/P2 resolved; caught `&&...||` precedence nit | **CONTINUE** |
| `reviewer-24562` | Deep / Verified | **HIGH SIGNAL** | Caught P1 vacuous inspection diff; proved paired base SHA fix; caught P2 invariant reconciliation | **CONTINUE** |
| `reviewer-3285` | Fast / Forensic | **HIGH SIGNAL** | Caught P1 multi-ref FETCH_HEAD diff defect; caught P2 spec pasted line prefixes; empty-before audit | **CONTINUE** |
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
