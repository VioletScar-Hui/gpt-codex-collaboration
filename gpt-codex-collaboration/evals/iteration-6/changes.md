# Iteration 6 changes

## Reason

The user clarified the primary product purpose: use the Skill when Codex can see that its remaining token
budget is insufficient but meaningful work is still unfinished.

## Changes

- Added observable low-budget plus unfinished-work as an independent positive routing condition.
- Required a high-context, safely delegable subproblem and reserved Codex budget for verification and delivery.
- Kept vague token concern and low-budget micro-tasks out of the external workflow.
- Updated the Chinese and English positioning while preserving privacy and authorization gates.
- Bumped SKILL metadata and eval-set versions from 1.1.0 to 1.2.0.

## Regression scope

Static walk-through: new `core-9` and `edge-11`, plus adjacent `core-7`, `core-8`, `edge-5`, and `edge-7`.
Independent model routing remains not run and must not be inferred from structural validation.

## Rollback

Revert the v1.2 commit to restore the explicit-collaboration-only v1.1 routing boundary.
