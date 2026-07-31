# Iteration 2 — Adversarial Review Round 1

## Verdict

Round 1: **fail / not publishable**.

## Findings fixed

1. P0: 208-line Skill plus mandatory reference load created negative token ROI.
2. P0: ROI Gate allowed Codex to read all material before delegating.
3. P0: “省 token” alone could trigger external collaboration.
4. P0: local write authorization was conflated with third-party disclosure authorization.
5. P1: description claimed arbitrary providers while workflow only supported ChatGPT Chrome.
6. P1: links, commands and patches in GPT replies lacked an executable isolation rule.

## Changes

- Rewrote SKILL.md as a thin protocol and moved the Gate before reference/browser use.
- Added a hard negative-ROI branch when Codex must ingest full material first.
- Restricted 1.1 to ChatGPT web sessions.
- Added public/private/sensitive disclosure classes and a separate disclosure Gate.
- Added tool isolation for links, commands, patches and downloads from external replies.
- Added adversarial evals for routing, private data, provider mismatch and prompt injection.

## Evidence status

- Structural validation: pending rerun after this change.
- Static instruction-size comparison: recorded in `benchmark.md`.
- Behavioral, Chrome and token A/B runs: pending; remain not-run until executed.
