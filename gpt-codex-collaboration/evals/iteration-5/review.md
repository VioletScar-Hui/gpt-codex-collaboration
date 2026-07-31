# Adversarial review — Round 4 final closure audit

Date: 2026-07-31

Reviewer: the same adversarial subagent used in Rounds 1–3

## Closure result

All Round-3 P0 and P1 findings are closed for a beta release:

| Finding | Status | Evidence |
|---|---|---|
| Cost metric overstated | Closed | Renamed to source-ingestion avoidance proxy, reduced to 83.3%, exclusions stated, `core-7` is partial. |
| Safety behavior lacked evidence | Beta gate closed | Six static fail-closed replays recorded; broader runtime coverage is still required for stable. |
| Missing business goal | Closed | Ask one short question; do not open the browser or invent a brief. |
| Retry budget ambiguity | Closed | One automatic recovery is shared by the entire external-review stage. |
| Trigger ambiguity | Closed | Any one explicit collaboration intent can trigger the Skill. |
| No auditable send checkpoint | Closed | All three pre-send records are mandatory. |
| Ambiguous reply node | Closed | Use the last completed, stable assistant message after the latest submitted prompt. |

## Pressure replay after fixes

All four exact pressure prompts passed. Workspace ambiguity, broad private consent, sensitive material,
and untrusted external commands all fail closed without disclosing test data.

## Publication verdict

**Beta: publishable. Stable: not yet supported.**

Remaining limitations are non-blocking for beta: only one real public-material Chrome success run; the new
stage-wide recovery rule and authentication/CAPTCHA/rate-limit branches lack end-to-end tests; most
cross-domain cases remain static; the measured benefit is source-ingestion avoidance rather than total token,
latency, or tool cost.
