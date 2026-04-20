# Stage279: VEP Externalized Decision Reproduction

## Overview

Stage279 introduces externally reproducible decision verification.

This stage enables third parties to independently reproduce:

- evidence input
- trust scoring
- final decision (accept / pending / reject)

The decision is no longer an interpretation — it is a deterministic artifact.

---

## Key Concept

From evidence → score → decision

All steps are:

- deterministic
- reproducible
- verifiable outside the original environment

---

## Decision Model

Immediate Gate:

integrity × execution × identity

Settlement Gate:

time

---

## Decision Rules

- **accept**
  - integrity = 1.0
  - execution = 1.0
  - identity = 1.0
  - time_trust = 1.0

- **pending**
  - immediate gate satisfied
  - time trust exists but not fully settled

- **reject**
  - one or more conditions failed

---

## Files

- `out/stage279/evidence_summary.json`
- `out/stage279/decision.json`
- `out/stage279/decision.sha256`
- `tools/build_stage279_decision.py`
- `tools/verify_stage279_decision.py`

---

## How to Reproduce

### Generate decision

```bash
python3 tools/build_stage279_decision.py
Verify decision
python3 tools/verify_stage279_decision.py
Quick External Verification (One Command)

You can reproduce the decision externally using GitHub Actions artifacts.

git clone https://github.com/mokkunsuzuki-code/stage279.git && \
cd stage279 && \
RUN_ID=$(gh run list --workflow stage279-decision --limit 1 --json databaseId -q '.[0].databaseId') && \
rm -rf downloaded_stage279_decision && \
mkdir -p downloaded_stage279_decision && \
gh run download $RUN_ID --dir downloaded_stage279_decision && \
python3 tools/verify_stage279_decision.py \
  --summary downloaded_stage279_decision/stage279-decision-artifacts/evidence_summary.json \
  --decision downloaded_stage279_decision/stage279-decision-artifacts/decision.json \
  --sha256 downloaded_stage279_decision/stage279-decision-artifacts/decision.sha256

This performs:

artifact download from GitHub Actions
independent recomputation of the decision
integrity verification via SHA256

No manual steps required.

What This Stage Proves
The final decision is deterministic
Anyone can reproduce the same result
The result is integrity-protected via SHA-256
No manual interpretation is required
Important Note

This stage does NOT prove that:

the evidence itself is universally correct

It proves that:

given the same input,
the same decision is always produced,
and the published result has not been altered
License

MIT License

Copyright (c) 2025 Motohiro Suzuki
