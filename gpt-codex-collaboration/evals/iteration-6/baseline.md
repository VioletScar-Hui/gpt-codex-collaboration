# v1.2 positioning baseline

Date: 2026-07-31

## Snapshot

The published v1.1.0 description required an explicit request to collaborate with ChatGPT and explicitly
excluded “save tokens” as a trigger by itself.

## Target replay against v1.1.0

| Scenario | Baseline result | Evidence |
|---|---|---|
| Observable Codex token budget is insufficient, substantial public work remains, and a high-context subproblem can be delegated safely | Fail | v1.1.0 has no independent low-budget rescue trigger; it waits for explicit ChatGPT collaboration intent. |
| Token budget is low but only a one-line edit remains | Pass | The Token ROI Gate rejects collaboration whose fixed overhead exceeds the remaining work. |

The target change is a new routing scenario, so v1.2 must improve the first case without regressing the second.
