# Stage279: VEP Externalized Decision Reproduction

## Overview

Stage279 makes the final VEP-style decision externally reproducible.

This is the first stage where a third party can independently reproduce:

- the input evidence summary
- the gate calculation
- the final decision: `accept`, `pending`, or `reject`

In other words, this stage turns the decision itself into a verifiable artifact.

---

## What This Stage Adds

Stage279 adds:

- `evidence_summary.json` as the normalized input
- deterministic decision generation
- `decision.json` as the final fixed output
- `decision.sha256` for integrity checking
- an external verification script that recomputes the decision independently

This transforms:

- score model → reproducible decision artifact

---

## Decision Model

Immediate Gate:

`integrity × execution × identity`

Settlement Gate:

`time`

Decision rules:

- `accept`
  - integrity = 1.0
  - execution = 1.0
  - identity = 1.0
  - time_trust = 1.0

- `pending`
  - immediate gate fully satisfied
  - time trust exists but is not fully settled

- `reject`
  - one or more gate conditions failed

---

## Files

- `out/stage279/evidence_summary.json`
- `out/stage279/decision.json`
- `out/stage279/decision.sha256`
- `tools/build_stage279_decision.py`
- `tools/verify_stage279_decision.py`

---

## How to Run

Generate the decision artifact:

```bash
python3 tools/build_stage279_decision.py

Verify the decision independently:

python3 tools/verify_stage279_decision.py
What This Stage Proves
The final decision is no longer just a visual interpretation
The decision can be reproduced mechanically from normalized evidence
A third party can recompute the same result independently
The result can be integrity-checked using SHA-256
Important Accuracy

This stage does not prove that the underlying evidence is universally true.

It proves something narrower and more important for external review:

given the same normalized evidence input,
the same decision is reproduced deterministically,
and the published decision artifact has not been altered.
License

MIT License

Copyright (c) 2025 Motohiro Suzuki
