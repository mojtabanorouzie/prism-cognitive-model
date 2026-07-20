# cognitive-model

The evidence ledger at the heart of Prism — where reverse-engineering one mind actually gets stored. Not a journal: a knowledge graph of hypotheses with confidence scores, maintained by three cooperating agents.

This is the internal architecture doc — how the store and the three agents fit together. If you just want to *use* Prism (including without Claude Code), start with the top-level [README](../README.md) ([فارسی](../README.fa.md)). For the exact command list, see the [cheat sheet](../CHEATSHEET.md) ([فارسی](../CHEATSHEET.fa.md)).

## The system

```
                  reads model, investigates
   ┌──────────────────────────────────────────────┐
   │                                               ▼
[historian] ◀── proposed deltas ── [analyst]   observations
   │  (single writer)                  │
   │                                   │ open questions,
   │  commits confidence,              │ low-confidence hypotheses
   │  retires/merges,                  ▼
   │  regenerates MODEL.md      [experiment-designer]
   ▲                                   │
   └────── proposed deltas ────────────┘
              (from experiment results)
```

- **analyst** (`cognitive-systems-analyst`) — observes a behavior, generates competing hypotheses, gathers evidence, proposes confidence deltas. Writes a session file. Read + Write (sessions only).
- **experiment-designer** (`behavioral-experiment-designer`) — turns weak/contested hypotheses into small, reversible, pre-registered experiments; records results. Writes experiment files.
- **historian** (`cognitive-historian`) — the single writer for the canonical model. Commits deltas into hypothesis files, retires rejected ones, regenerates the dashboard, maintains the timeline. Owns `hypotheses/` and `MODEL.md`.

Single-writer on the canonical state is the whole point: the other two agents *propose*, the historian *commits*. No races on the model.

## Layout

```
cognitive-model/
  README.md        this file
  MODEL.md         the living dashboard — start here
  TEMPLATES.md     schemas (frontmatter + sections) every agent follows
  hypotheses/      one ADR-style file per hypothesis (H001…)  ← historian writes
  experiments/     one file per experiment (E001…)            ← designer writes
  sessions/        one file per investigation (S-DATE-…)      ← analyst writes
```

## How to run it

From the main Claude Code conversation, delegate to a subagent:

- **Investigate something:** ask for the `cognitive-systems-analyst` agent — "investigate why I keep rewriting the same module three times before shipping."
- **Design a test:** ask for the `behavioral-experiment-designer` agent — "design an experiment for H003."
- **Commit findings / update the model:** ask for the `cognitive-historian` agent — "reconcile the latest session into the model."

A full cycle: analyst → (you read the session) → historian commits it → designer creates a test → you run it in real life → you report back → designer records the result → historian commits the delta. The model gets more accurate each loop.

## The one rule

Truth over comfort. A hypothesis you can't imagine disconfirming isn't a hypothesis — it's the bug.
