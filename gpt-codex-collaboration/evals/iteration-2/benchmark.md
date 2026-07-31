# Static Instruction Cost Proxy

This is a character/line proxy, not a claim about billed model tokens.

| Artifact | v1.0.0 baseline | v1.1.0 Round 1 | v1.1.0 final beta | Meaning |
|---|---:|---:|---:|---|
| SKILL.md | 208 lines / 5,309 chars | 93 lines / 2,183 chars | 117 lines / 3,017 chars | Always-loaded body; final chars reduced 43.2% |
| collaboration-brief.md | 83 lines / 1,268 chars | 36 lines / 524 chars | 36 lines / 589 chars | Loaded only after positive ROI Gate; final chars reduced 53.5% |

The goal is to reduce always-loaded instructions and prevent loading the brief when collaboration is not
worthwhile. Safety controls added after Round 1 increased the final body. Actual task-level token savings
require behavioral A/B evidence and are not claimed here.
