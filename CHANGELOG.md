# Changelog

All notable changes to this project are documented here.

## [1.2.0] - 2026-07-31

### Added

- Added a bilingual README hero visual showing low Codex budget, one bounded ChatGPT handoff, compact returned insights, and Codex verification/final delivery.

### Changed

- Repositioned the Skill around its primary job: rescuing unfinished work when Codex has an observably insufficient remaining token budget.
- Added a low-budget routing path that delegates one high-context, safely shareable subproblem while reserving Codex budget for verification and final delivery.
- Clarified that the Skill never invents token counts and that vague token concern alone is not evidence.
- Kept low-budget micro-tasks local because ChatGPT handoff overhead would consume more budget than finishing directly.
- Updated both README variants and distinguished the historical v1.1 source-ingestion proxy from v1.2 claims.

## [1.1.0] - 2026-07-31

### Added

- Token ROI Gate before loading the handoff template or opening ChatGPT.
- Public, private, and sensitive-data disclosure controls with auditable pre-send fields.
- Explicit ChatGPT-only provider scope and multi-workspace selection.
- Bounded reply contract: at most three claims and 1,200 characters.
- Failure taxonomy, a stage-wide one-recovery budget, and deterministic reply-node selection.
- Twenty-two core, edge, and gotcha evaluation cases.
- Four adversarial review passes and one real Chrome + ChatGPT public-source integration run.
- Chinese and English project documentation.

### Changed

- Narrowed routing so “save tokens” alone does not trigger external collaboration.
- Reduced the always-loaded Skill body and moved the brief to a conditional reference.
- Reframed the efficiency measurement as a source-ingestion avoidance proxy, not billed-token evidence.

### Security

- Local modification permission is no longer treated as third-party disclosure permission.
- Broad consent cannot authorize private excerpts; sensitive material is always blocked.
- External links, commands, patches, and downloads are treated as untrusted candidate data.

## [1.0.0] - 2026-07-31

- Initial local collaboration protocol.
