# Prism

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Works with Claude Code](https://img.shields.io/badge/works%20with-Claude%20Code-8A2BE2.svg)](https://docs.claude.com/en/docs/claude-code)
[![Language: English | فارسی](https://img.shields.io/badge/docs-English%20%7C%20%D9%81%D8%A7%D8%B1%D8%B3%DB%8C-informational.svg)](README.fa.md)

**English** | [فارسی](README.fa.md)

> **Debug your own mind the way a senior engineer debugs an unfamiliar production system.**
>
> Prism is engineered to *reverse-engineer* your behavior down to its **root cause** — not to manage the symptom on the surface, but to trace a pattern back to the real driver underneath and prove which explanation actually holds.

Prism is a small toolkit that helps you figure out *why you do the things you do*. You bring one puzzling behavior — "I don't know why I keep checking my phone," "why do I rewrite the same thing five times before I'm happy with it" — and Prism helps you break it apart into the different explanations that could be true, then test them against real life instead of just guessing.

Like a glass prism splits one beam of white light into its full spectrum of colors, Prism takes a single behavior and refracts it into the full spectrum of hypotheses that might explain it — then helps you find out which ones actually hold up.

Under the hood it's three cooperating **Claude Code sub-agents** (small, specialized AI assistants, each with one job) that turn vague self-observations into falsifiable hypotheses, small real-world experiments, and an evidence-backed model of your own behavior — instead of vibes, journaling, or generic advice.

---

## Table of contents

- [What it is / who it's for](#what-it-is--who-its-for)
- [A quick vocabulary](#a-quick-vocabulary)
- [Why this exists](#why-this-exists)
- [How it works — three agents, one rule](#how-it-works--three-agents-one-rule-propose-vs-commit)
- [Quick start](#quick-start)
  - [Option A — Claude Code (recommended, zero setup)](#option-a--claude-code-recommended-zero-setup)
  - [Option B — Any other model (ChatGPT, Gemini, Claude.ai, local)](#option-b--any-other-model-chatgpt-gemini-claudeai-a-local-model-anything-with-a-chat-box)
- [A full loop, start to finish](#a-full-loop-start-to-finish)
- [Repo layout](#repo-layout)
- [FAQ](#faq)
- [Not therapy](#not-therapy)
- [Privacy note](#privacy-note)
- [License](#license)

---

## What it is / who it's for

Prism is a structured self-reflection tool. It's for anyone who has noticed a pattern in themselves and genuinely wants to understand it — not to be reassured about it, and not to be handed generic advice, but to actually find out what's going on.

You do **not** need to be a programmer, and you do **not** need to have used an AI coding tool before. If you can copy a sentence and paste it into a chat box, you can use Prism. The [Quick start](#quick-start) walks you through it step by step.

What makes it different from journaling or a chatbot pep talk: Prism is built to be *honest, not flattering*. It treats your beliefs about yourself as claims to be tested, actively hunts for evidence that you're wrong, and keeps a written record of what's been ruled out. It's a scientist for a subject of one — you.

**Private by design.** Everything Prism creates is plain text saved in this folder on your own computer. Nothing is uploaded to any server, and nothing reaches git or GitHub unless you deliberately push it — your personal hypotheses, sessions, and experiments are git-ignored by default, so your inner life never leaves your machine.

## A quick vocabulary

Three words show up constantly. Here's what they mean **inside Prism** (they're used in a specific way):

- **Hypothesis** — a guess about *why* you do something, written so it could turn out to be false. Not "I have low self-esteem" (too vague to test) but "I rewrite my work repeatedly because I expect criticism, so I try to make it un-criticizable first." A behavior usually has several competing hypotheses; that's the point.
- **Confidence** — a number from `0.00` to `1.00` attached to each hypothesis, saying how well the evidence currently supports it. It only moves when something gives a logged reason. `~0.50` means "genuinely unsure — worth testing." It never reaches `1.00`, because you can always be surprised.
- **Experiment** — a small, safe, reversible thing you actually do in real life to test a hypothesis, with a prediction written down *beforehand* so hindsight can't quietly rewrite it. Prism deals in experiments, not advice.

## Why this exists

Most self-help is untested opinion. Prism treats your own behavior like a system to be reverse-engineered:

- Every belief about yourself is a **hypothesis**, not a fact.
- Every hypothesis needs **evidence for and against** — an empty "evidence against" list is a warning sign, not a good sign.
- Every hypothesis gets a **confidence score (0.00–1.00)** that only moves when something logs a reason.
- Nothing is deleted. Rejected hypotheses are kept and marked `rejected` — proof of what's already been tested.
- Advice is not the deliverable. **Experiments** are — small, measurable, reversible, pre-registered predictions you actually go run in your life.

## How it works — three agents, one rule: propose vs. commit

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

| Agent | File | Job |
|---|---|---|
| **cognitive-systems-analyst** | `.claude/agents/cognitive-systems-analyst.md` | Investigates a behavior, generates 3–5 competing hypotheses, gathers evidence, writes a session file. Never diagnoses, never flatters. |
| **behavioral-experiment-designer** | `.claude/agents/behavioral-experiment-designer.md` | Turns a weak/contested hypothesis into one small, safe, real-world experiment with a prediction registered *before* the result. |
| **cognitive-historian** | `.claude/agents/cognitive-historian.md` | The only agent allowed to write `hypotheses/` and `MODEL.md`. Commits confidence deltas, retires rejected hypotheses, regenerates the dashboard. |

The analyst and the experiment designer only ever **propose** changes. Only the historian **commits** them. That single-writer rule is what keeps the model from turning into three agents overwriting each other — think of the historian as the only one holding the pen for your permanent record.

## Quick start

### Option A — Claude Code (recommended, zero setup)

If you have [Claude Code](https://docs.claude.com/en/docs/claude-code) installed, this is the easiest path — the three agents are detected automatically.

1. **Get the project.** Clone or download this repo into its own folder.
2. **Open the folder in Claude Code.** It auto-detects the three agents in `.claude/agents/` — nothing to configure.
3. **Investigate something.** In the main conversation, type a sentence like this (swap in whatever you keep noticing about yourself):
   ```
   use the cognitive-systems-analyst to investigate why I keep checking my phone the second it buzzes
   ```
   The analyst asks you questions, proposes a few competing explanations, and writes a **session file** into `cognitive-model/sessions/`.
4. **Commit what it found.** Read the session, then hand it to the historian to record:
   ```
   use the cognitive-historian to reconcile the latest session
   ```
5. **Read your dashboard.** Open `cognitive-model/MODEL.md` — that's your model now: your hypotheses, their confidence, and what's still open.

That's one full pass. From there you can design an experiment for a shaky hypothesis, run it in real life, and feed the result back in — see [A full loop, start to finish](#a-full-loop-start-to-finish).

> The commands above aren't slash commands or magic syntax — they're plain English. You're just telling the main conversation which agent to hand the work to. The one thing that must be exact is the **agent name** (`cognitive-systems-analyst`, `behavioral-experiment-designer`, `cognitive-historian`); everything around it you can phrase however you like.

For the full command reference, see **[CHEATSHEET.md](CHEATSHEET.md)** (or the **[Persian version](CHEATSHEET.fa.md)**).

### Option B — Any other model (ChatGPT, Gemini, Claude.ai, a local model, anything with a chat box)

You don't need Claude Code to use Prism. Each file in `.claude/agents/` is a plain Markdown file: a short header Claude Code uses for routing, followed by a plain-language system prompt any capable chat model can run.

1. **Grab an agent's brain.** Open `.claude/agents/cognitive-systems-analyst.md`. Copy everything **below** the `---` / `---` header block at the top (that header is just for Claude Code; the instructions are what matter).
2. **Paste it in as the model's instructions.**
   - **ChatGPT** — paste it into a Custom GPT's "Instructions" field, or as your first message in a normal chat.
   - **Claude.ai** — create a Project and paste it into the Project's custom instructions.
   - **Gemini** — paste it into a Gem's instructions.
   - **Local model (Ollama, LM Studio, etc.)** — use it as the system prompt.
3. **Carry the state by hand.** These tools can't read your folder on their own, so you're the courier:
   - At the **start** of a session, paste in the contents of `cognitive-model/MODEL.md` and any relevant `hypotheses/*.md` files (or upload them, if your tool supports file/knowledge attachments — ChatGPT Projects, Claude Projects, and Gemini Gems all do).
   - When the model **finishes**, copy its output into a new file under `cognitive-model/sessions/` yourself, following the schema in `cognitive-model/TEMPLATES.md`.
   - Repeat the same paste-in / copy-out pattern for the other two agents.
4. It's more manual than Claude Code, but the actual thinking — the hypotheses, the evidence discipline, the experiments — works identically on any model capable of following instructions.

## A full loop, start to finish

Here's what one complete cycle looks like, so the pieces click together. Suppose you keep rewriting the same piece of work over and over before you'll ship it.

1. **Investigate** — *analyst.*
   ```
   use the cognitive-systems-analyst to investigate why I rewrite the same module five times before shipping
   ```
   It proposes competing explanations — say `H004: "I'm a perfectionist"` and `H005: "I rewrite to pre-empt criticism I expect to receive"` — assigns each a starting confidence, and writes a session file. Nothing is committed to your model yet.
2. **Commit** — *historian.*
   ```
   use the cognitive-historian to reconcile the latest session
   ```
   The historian records those hypotheses in `hypotheses/`, logs each confidence with a reason, and regenerates `MODEL.md`.
3. **Design a test** — *experiment designer.* `H005` is sitting near `0.50` — genuinely uncertain, so it's the highest-value thing to test:
   ```
   use the behavioral-experiment-designer to design a test for H005
   ```
   It writes a small, reversible experiment to `experiments/` with a prediction registered up front — e.g. "ship the next piece after **one** pass instead of five; predict how anxious you'll feel (0–10) and whether anyone actually criticizes it."
4. **Run it in real life.** This part is yours. Do the thing; note what actually happened.
5. **Record the result** — *experiment designer.*
   ```
   use the behavioral-experiment-designer to record E001: predicted 8, actual 2, no criticism received
   ```
   It fills in the result and interpretation and **proposes** a confidence change.
6. **Commit the result** — *historian.*
   ```
   use the cognitive-historian to commit the E001 deltas
   ```
   The historian moves `H005`'s confidence, logs why, and updates the dashboard.

Each loop makes the model a little more accurate. Nothing is automatic — you drive every hop, which is what keeps it honest.

## Repo layout

```
.claude/agents/                        the three agent definitions
cognitive-model/
  README.md                            how the system works internally (architecture)
  TEMPLATES.md                         exact schema every agent follows
  MODEL.md                             the dashboard — start here once you have data
  hypotheses/H000-example-*.md         one fully worked EXAMPLE — delete once you seed your own
  experiments/                         empty — the designer writes here
  sessions/                            empty — the analyst writes here
CHEATSHEET.md / CHEATSHEET.fa.md        quick command reference (English / Persian)
README.md / README.fa.md               this file (English / Persian)
```

New here and not sure where to start? Read the worked example at [`cognitive-model/hypotheses/H000-example-notification-checking.md`](cognitive-model/hypotheses/H000-example-notification-checking.md) to see a hypothesis filled out end to end, then delete it and run:

```
use the cognitive-systems-analyst to investigate <something you actually keep noticing about yourself>
```

## FAQ

**Do I need to know how to code?**
No. If you can copy a sentence into a chat box, you can use Prism. The systems-and-engineering framing is a metaphor — the agents adapt to plain language too.

**Do I need Claude Code?**
No, but it's the smoothest experience. Without it, follow [Option B](#option-b--any-other-model-chatgpt-gemini-claudeai-a-local-model-anything-with-a-chat-box) and carry the files between your model and the folder by hand. The reasoning is identical on any capable model.

**Why three agents instead of one?**
Separation of duties. One agent that investigates *and* tests *and* keeps the permanent record tends to flatter itself and quietly rewrite history. Splitting the roles — and letting only the historian write the canonical model — keeps each step honest.

**What's a "confidence score" and why does it never hit 1.00?**
It's how strongly the current evidence backs a hypothesis, from `0.00` to `1.00`. It never reaches `1.00` on purpose: a claim you treat as 100% certain is one you've stopped testing, and that's exactly the blind spot Prism is built to find.

**Where does my data live?**
Entirely in this folder on your machine, as plain Markdown files — and git-ignored by default, so they're never committed even if you push the repo. Nothing is uploaded anywhere by Prism itself. See the [Privacy note](#privacy-note) before you share or push.

**Can I just delete the example?**
Yes — once you've run your first real investigation, delete `H000-example-notification-checking.md` and its row in `MODEL.md`. It's only there to show you the format.

## Not therapy

This is a self-reflection and journaling framework, not a medical or mental-health tool. It's built to be honest, not kind — that's the point, but it also means it's the wrong tool for a crisis. If you're dealing with trauma, a mental health condition, or you're in crisis, please talk to a licensed professional or a crisis line instead.

## Privacy note

Prism runs entirely on your own machine. Everything it produces is plain Markdown saved inside this folder — nothing is sent to any server, and **your personal data is never committed to git by default.** Once you start using it for real, `cognitive-model/hypotheses/`, `sessions/`, and `experiments/` fill up with sensitive material about you; the included [`.gitignore`](.gitignore) already excludes those from version control (keeping only the worked example and the folder READMEs), so even if you push your copy, your private entries stay local. For extra peace of mind, you can also make your fork **private**.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, change it, send it to your friends.
