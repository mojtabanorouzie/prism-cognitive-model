# Schemas

The exact file shapes every Prism agent reads and writes, so the store stays machine-parseable. These are contracts, not suggestions: copy a block verbatim and fill it in — don't rename fields or improvise the structure, or the other agents won't be able to read what you wrote.

Confidence is always a float `0.00`-`1.00`. Anchors: `<0.20` unlikely · `~0.50` genuinely uncertain, test it · `>0.80` well-supported. Never `1.00`.

---

## Hypothesis file — `hypotheses/H<NNN>-<slug>.md`

Written/updated only by the **historian**.

```markdown
---
id: H001
title: Short falsifiable claim
status: active            # active | confirmed | rejected | merged
confidence: 0.80          # 0.00–1.00
created: 2026-07-16
updated: 2026-07-16
tags: [self-worth, defense]
related: [H005]           # coupled hypotheses
supersedes: []            # ids this replaces (for merged/rejected)
---

## Statement
One sentence. Must be disconfirmable.

## Mechanism
Trigger -> Thought -> Emotion -> Prediction -> Behavior -> Immediate reward -> Long-term cost -> Why the system keeps running it.

## Evidence for
- [YYYY-MM-DD, source] observation

## Evidence against
- [YYYY-MM-DD, source] observation   (empty is a warning sign — go find some)

## Predictions
If true, we should also observe ___. If false, we'd expect ___.

## Experiments
- E003 (running) — links to tests probing this

## Confidence log
- 2026-07-16: seeded at 0.80 — from initial profile
```

---

## Experiment file — `experiments/E<NNN>-<slug>.md`

Written by the **experiment-designer**.

```markdown
---
id: E001
tests: [H002, H003]
status: proposed          # proposed | running | done | aborted
created: 2026-07-16
---

## Context
Where and when it runs.

## Action
The single concrete, reversible thing to do.

## Measure
Exactly what will be observed and how it's scored.

## Prediction (before)
What the hypothesis predicts + the user's stated confidence. Registered before running.

## Result (after)
Raw observation only. No interpretation.

## Interpretation
What the result implies for the hypothesis.

## Proposed confidence delta
- H003: 0.65 -> 0.55 — result disconfirmed the prediction
```

---

## Session file — `sessions/S-YYYY-MM-DD-<slug>.md`

Written by the **analyst**.

```markdown
---
id: S-2026-07-16-intro
date: 2026-07-16
agent: cognitive-systems-analyst
touches: [H004, H005]
---

## Observation (raw log)
The reported behavior, un-explained.

## Hypotheses considered
- H004 (existing): …
- New candidate: …

## Evidence gathered
Answers, counterexamples, contradictions surfaced.

## Proposed confidence changeset
- H004: 0.80 -> 0.82 — reason
- H007: 0.50 -> 0.45 — reason

## Open questions
- …

## Next investigation candidate
- …
```
