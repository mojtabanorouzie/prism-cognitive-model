# Cheat sheet

These aren't slash commands — they're natural-language delegations you type to the main Claude Code conversation, which routes them to the right subagent. Full architecture is in [README.md](README.md). Not using Claude Code? See "Option B" in the README — the same commands work as plain instructions to whatever model you paste the agent prompt into.

🇮🇷 نسخه فارسی: [CHEATSHEET.fa.md](CHEATSHEET.fa.md)

## Core loop

| Goal | What you type |
|------|---------------|
| **Investigate** a behavior | `use the cognitive-systems-analyst to investigate <behavior>` |
| **Commit** findings to the model | `use the cognitive-historian to reconcile the latest session` |
| **Design a test** | `use the behavioral-experiment-designer to design a test for H005` |
| **Log a result** | `use the behavioral-experiment-designer to record E001: predicted 6, actual 0, no negative event` |
| **Apply the result** | `use the cognitive-historian to commit the E001 deltas` |

Pipeline: **analyst → historian → designer → (you run it in real life) → designer → historian.** Nothing is automatic; you drive each hop.

## Agent 1 — Analyst (investigate)

```
use the cognitive-systems-analyst to investigate why I rewrite the same module before shipping
use the analyst to challenge H004 — find counterexamples where I shipped fast
use the analyst on this: I said "I'm just lazy" about the gym
```
Writes a session file to `sessions/` with proposed confidence deltas. Does **not** commit them.

## Agent 2 — Experiment Designer (test)

```
use the behavioral-experiment-designer to design a test for the H006/H007 coupling
use the experiment-designer to find the single highest-value experiment right now
use the experiment-designer to record E001 result: predicted 7, actual 1
```
Writes/updates files in `experiments/`. Targets hypotheses near **0.50** (max information gain).

## Agent 3 — Historian (commit + read state)

```
use the cognitive-historian to reconcile session S-2026-07-16-seed
use the cognitive-historian to reject H004 and note H005 as its parent
use the cognitive-historian to regenerate MODEL.md
use the cognitive-historian to show me the current dashboard and timeline
```
The **only** agent that writes `hypotheses/` and `MODEL.md`. Every delta gets a logged reason + source.

## Reading state directly (no agent needed)

```
show me MODEL.md                    # the dashboard — start here
show me H005                        # one hypothesis
list all rejected hypotheses        # what's been tested and killed
show me open experiments
```

## Handy compound moves

```
run a full cycle on <behavior>: analyst, then historian, then designer
what's my highest-value open question right now?
which hypothesis is least tested?          # find the empty "Evidence against" sections
recompute confidences from the evidence and tell me what moved
```

## Invariants (so you catch a misbehaving agent)

- Analyst & Designer **propose** deltas; only the **Historian commits**. If the analyst edits a hypothesis file directly, it broke protocol.
- No confidence ever hits **1.00**, and every change cites a source (`S-…` or `E…`).
- Rejected hypotheses are **retired, not deleted** — an empty `rejected` list early on means nothing's been falsified yet.

## First time here?

If `cognitive-model/hypotheses/` only has the `H000-example-*` file, you haven't seeded your own model yet. Start with:
```
use the cognitive-systems-analyst to investigate <something you keep noticing about yourself>
```
