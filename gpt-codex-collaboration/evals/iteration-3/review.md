# Iteration 3 — Adversarial Review Round 2

## Verdict

Round 2: **fail / not publishable**.

## Round 1 closure

- Static loading cost, premature full ingestion, token-only misrouting, provider scope and external-command
  isolation were closed or materially improved.
- Task-level token evidence and browser behavior remained unverified.
- Private disclosure authorization was only partially closed.

## Changes after Round 2

- Replaced “one screen” with a measurable response schema: at most 3 claims, one sentence per field,
  1,200-character read cap.
- Added a pre-read node-length check and one compression retry; oversized text is not imported wholesale.
- Made private disclosure confirmation specific to file/range, data class, purpose and target session.
- Added workspace-selection rules and provider/browser failure taxonomy.
- Restored the requirement that domain Skills own safety, evidence and implementation.
- Added measurable eval fields for brief size, response size, browser calls and disclosure evidence.

## Evidence still required before Round 3

- Real Chrome success run with bounded response extraction.
- Real browser failure/recovery record.
- Positive ROI, negative ROI and forced-override cost records.
