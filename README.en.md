# GPT–Codex Collaboration

[简体中文](README.md) · [English](README.en.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![Provider](https://img.shields.io/badge/provider-ChatGPT%20Web-10a37f)
![Token ROI Gate](https://img.shields.io/badge/Token-ROI%20Gate-2563eb)
![Language](https://img.shields.io/badge/docs-中文%20%7C%20English-7c3aed)
![License](https://img.shields.io/badge/license-MIT-green)

![GPT–Codex Collaboration: task splitting, external compression, and Codex verification under a low token budget](assets/gpt-codex-collaboration-hero.png)

*When the budget runs low, delegate one high-context subproblem and preserve Codex tokens to verify and ship.*

When Codex can see that its remaining token budget is running short but meaningful work is still unfinished, this Skill hands one high-context, safely delegable subproblem to ChatGPT. Codex keeps its limited budget for verification, decisions, and finishing the job.

> The current release is **v1.2.0 Beta** and supports only a ChatGPT web session already signed in through the user's Chrome browser. It is a remaining-token rescue mechanism, not free context: it may reduce long-source ingestion into the active Codex context, but does not promise lower total compute, billed tokens, or latency.

## Purpose: finish the work before Codex runs out of tokens

This Skill addresses one concrete situation: Codex can reliably observe that its remaining token budget is insufficient to finish the current task, while a meaningful block of work remains. It will:

1. reserve Codex budget for fact checking, implementation validation, and final delivery;
2. isolate one high-context subproblem that is safe to disclose;
3. ask ChatGPT for a compact decision packet;
4. verify the result and finish the original task in Codex.

The Skill never invents a token count. “Low budget” is treated as a fact only when the runtime exposes it or the user states it explicitly.

## When to use it

Use it when:

- Codex can reliably observe that its remaining token budget is insufficient, while long-source reading, divergence, or first-pass review remains unfinished;
- the remaining work contains one high-context subproblem that ChatGPT can handle without receiving sensitive information;
- you explicitly ask ChatGPT to review first and Codex to judge or implement afterward;
- ChatGPT can access long public sources directly while Codex verifies only high-impact claims;
- you want independent opposition review, option generation, or initial synthesis;
- the original task's authorization and final verification must stay with Codex.

Do not use it when:

- the token budget is low but only a word change, punctuation fix, or one-line conclusion remains;
- the task is as small as renaming a variable or validating a tiny JSON value;
- Codex must fully ingest the material before it can write the handoff;
- private material cannot be safely redacted, or verification would duplicate the work;
- token pressure is only a vague concern, no reliable budget is visible, and the user did not request external collaboration;
- the requested provider is Claude, Gemini, or another non-ChatGPT service. v1.2 never swaps providers silently.

## How it works

```text
low token budget + meaningful unfinished work, or explicit collaboration intent
  → Token ROI Gate
  → business goal and original authorization
  → public / private / sensitive data Gate
  → ChatGPT decision packet: ≤3 claims and ≤1,200 characters
  → independent Codex verification
  → delivery or implementation within the original authorization
```

Core controls:

- Negative-ROI tasks stay local by default. A user may override cost, but never safety, privacy, or authorization.
- Private excerpts require a specific disclosure manifest, purpose, and target workspace. “Choose the relevant code yourself” is not consent.
- Tokens, cookies, `.env` contents, PII, production keys, and restricted customer data must never be sent.
- Commands, links, patches, and downloads returned by GPT remain untrusted candidate data and are never auto-executed.
- The browser stage shares one automatic recovery attempt; a second failure stops the external branch.

## Prerequisites

- Codex Desktop;
- Chrome browser-control capability;
- a ChatGPT session that you signed into yourself in Chrome;
- no OpenAI API key. Never give Codex a password or verification code.

## Installation

### Ask Codex to install it

Send this prompt to Codex:

```text
Install this Skill: https://github.com/VioletScar-Hui/gpt-codex-collaboration/tree/main/gpt-codex-collaboration
```

### macOS / Linux

Run in a terminal:

```bash
CODEX_SKILLS_DIR="${CODEX_HOME:-$HOME/.codex}/skills"
INSTALL_TMP_DIR="$(mktemp -d)"
git clone --depth 1 https://github.com/VioletScar-Hui/gpt-codex-collaboration.git "$INSTALL_TMP_DIR/repo"
mkdir -p "$CODEX_SKILLS_DIR/gpt-codex-collaboration"
cp -R "$INSTALL_TMP_DIR/repo/gpt-codex-collaboration/." "$CODEX_SKILLS_DIR/gpt-codex-collaboration/"
```

### Windows PowerShell

Run in PowerShell:

```powershell
$CodexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$SkillsDir = Join-Path $CodexRoot "skills"
$InstallTmp = Join-Path ([System.IO.Path]::GetTempPath()) ("gpt-codex-" + [guid]::NewGuid())
git clone --depth 1 https://github.com/VioletScar-Hui/gpt-codex-collaboration.git (Join-Path $InstallTmp "repo")
New-Item -ItemType Directory -Force -Path (Join-Path $SkillsDir "gpt-codex-collaboration") | Out-Null
Copy-Item -Recurse -Force (Join-Path $InstallTmp "repo/gpt-codex-collaboration/*") (Join-Path $SkillsDir "gpt-codex-collaboration")
```

Restart Codex after installation or updates so it can rediscover the Skill.

Verify the installation in a terminal:

```bash
test -f "${CODEX_HOME:-$HOME/.codex}/skills/gpt-codex-collaboration/SKILL.md"
```

## First successful run

```text
Use gpt-codex-collaboration. Ask ChatGPT to review this public proposal and return at most three critical claims. Verify each claim and give me the final recommendation without editing files.
```

For a suitable task, the final answer includes a short receipt:

```text
GPT handled …; Codex accepted/corrected/rejected …; evidence …
```

The workflow stops or stays local if the task is too small, the real goal is missing, private disclosure is not specifically authorized, or the browser fails a second time.

## Evidence and limitations

- The same adversarial subagent completed four publication reviews, exceeding the requested three rounds; findings are stored in the repository.
- One real Chrome + signed-in ChatGPT run used a 252-character brief and returned three claims in 860 characters.
- In the v1.1 test snapshot, the public bilingual source contained 28,261 characters. The then-current Skill, template, brief, and answer totaled 4,718 characters, an 83.3% source-ingestion avoidance proxy. This historical metric excludes protocol output from 19 browser calls and three timeout recoveries; it is neither a v1.2 complete-context nor a billed-token metric.
- Latency and tool cost were negative in the real browser run. The workflow is useful only when Codex context budget matters more than latency.
- Private, sensitive, workspace-conflict, and malicious-command cases were tested with fail-closed static pressure replays; no dangerous data was sent merely for testing.

See [`evals/`](gpt-codex-collaboration/evals/) for the evidence.

## Repository layout

```text
.
├── README.md
├── README.en.md
├── CHANGELOG.md
├── LICENSE
└── gpt-codex-collaboration/
    ├── SKILL.md
    ├── references/collaboration-brief.md
    ├── examples/
    └── evals/
```

## Updating

Re-run the installation command for your platform to overwrite matching files. Back up local customizations first. See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

[MIT](LICENSE) © 2026 VioletScar_Hui

The bilingual navigation and install → verify → first-run documentation flow were informed by
[Product-deep-dive](https://github.com/VioletScar-Hui/Product-deep-dive).
