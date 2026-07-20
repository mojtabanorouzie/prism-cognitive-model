---
id: H000
title: "EXAMPLE — checking my phone the instant a notification arrives is about resolving uncertainty, not the content of the message"
status: active
confidence: 0.60
created: 2026-07-20
updated: 2026-07-20
tags: [example, attention, uncertainty]
related: []
supersedes: []
---

> **This is a worked example, not real data.** It's here to show you what a finished hypothesis looks like — the frontmatter, the mechanism written as a loop, evidence on *both* sides, and a confidence log — before you write your own. Once you've run your first real investigation, delete this file (and its row in [`../MODEL.md`](../MODEL.md)). See the quick start in the top-level [README](../../README.md) ([فارسی](../../README.fa.md)).

## Statement
The urge to check a phone the moment it buzzes is driven more by an intolerance of the "unknown" state than by wanting to see the actual message.

## Mechanism
Trigger (buzz/notification) -> Thought ("what if it's important / what if I'm missing something") -> Emotion (mild anxiety, itch) -> Prediction ("checking now will remove the itch") -> Behavior (check within seconds) -> Immediate reward (uncertainty resolved, itch gone) -> Long-term cost (attention fragmentation, reinforces the loop, never builds tolerance for the unknown) -> Why the system keeps running it: the reward is immediate and the cost is diffuse/delayed, so the loop out-competes any "I'll check it later" intention.

## Evidence for
- [2026-07-18, session S-2026-07-18-notification-check] Checked phone within 10 seconds for 8/10 notifications during a single evening, regardless of sender.
- [2026-07-19, session S-2026-07-18-notification-check] Reported the urge felt identical for a work message and a spam text — supports "uncertainty" over "content" as the driver.

## Evidence against
- [2026-07-19, session S-2026-07-18-notification-check] Notifications from one specific person were checked faster than the average every time — suggests sender/content *does* matter at least sometimes, not pure uncertainty-resolution.

## Predictions
If true, we should also observe: checking speed stays roughly constant across trivial and important senders (with the one known exception above). If false, we'd expect: checking speed to track the predicted importance of the sender/app.

## Experiments
- (none yet — this is the example; a real E001 would go in `../experiments/` once you design one, e.g. "silence notifications for one evening and log the urge without acting on it")

## Confidence log
- 2026-07-20: seeded at 0.60 — example only, not real evidence.
