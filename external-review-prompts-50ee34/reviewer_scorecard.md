# Reviewer Signal Scorecard: external-review-prompts-50ee34

## Overview & Tripwire Notice
> [!IMPORTANT]
> **BUILDER-OWNED & READ-ONLY**: This file is maintained exclusively by the builder agent. External review agents may read this file to check their signal level and retain/drop directive. Any edit, staging, or deletion of this file by an external agent triggers an immediate alert to the user to **TERMINATE** that agent's session.

---

## Active Scorecard (Round 8 Triage)

| Reviewer ID | Speed / State | Signal Level | Key Contributions / Findings | Directive |
|---|---|---|---|---|
| `reviewer-18251` | Deep / Active | **HIGH SIGNAL** | Caught gate announcement & stall bound gap; required fresh AUDITED_SHA handshake | **CONTINUE** |
| `reviewer-24562` | Deep / Verified | **HIGH SIGNAL** | Proved 3-dot offline diff failure in shallow clones; caught authorship audit ref check | **CONTINUE** |
| `reviewer-3285` | Fast / Forensic | **HIGH SIGNAL** | Caught gate termination bound; reconciled content precedence vs process gate; empty-before audit | **CONTINUE** |
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
