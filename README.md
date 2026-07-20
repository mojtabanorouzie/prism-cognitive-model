# MindDebug

**Debug your own mind the way a senior engineer debugs an unfamiliar production system.**

MindDebug is a small framework of three cooperating AI agents that turn vague self-observations ("I don't know why I keep doing that") into falsifiable hypotheses, real-world experiments, and an evidence-backed model of your own behavior — instead of vibes, journaling, or generic advice.

> 🇮🇷 راهنمای فارسی: چیت‌شیت فارسی این پروژه اینجاست → [CHEATSHEET.fa.md](CHEATSHEET.fa.md)

## Why this exists

Most self-help advice is untested opinion. MindDebug treats your own behavior like a system to be reverse-engineered:

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

The analyst and the experiment designer only ever *propose* changes. Only the historian *commits* them. That single-writer rule is what keeps the model from turning into three agents overwriting each other.

## Quick start

### Option A — Claude Code (recommended, zero setup)

1. Clone or download this repo into its own folder.
2. Open that folder in Claude Code — it auto-detects the three agents in `.claude/agents/`.
3. In the main conversation, type:
   ```
   use the cognitive-systems-analyst to investigate <a behavior or pattern you keep noticing about yourself>
   ```
4. Read what it writes to `cognitive-model/sessions/`, then:
   ```
   use the cognitive-historian to reconcile the latest session
   ```
5. Open `cognitive-model/MODEL.md` — that's your dashboard now.

See [CHEATSHEET.md](CHEATSHEET.md) (or the [Persian version](CHEATSHEET.fa.md)) for the full command reference.

### Option B — Any other model (ChatGPT, Gemini, Claude.ai, a local model, anything with a chat box)

You don't need Claude Code to use this. Each file in `.claude/agents/` is a plain Markdown file: a short header Claude Code uses for routing, followed by a plain-language system prompt any chat model can run.

1. Open `.claude/agents/cognitive-systems-analyst.md`. Copy everything **below** the `---` / `---` header block at the top.
2. Paste it into your model of choice as the system prompt / custom instructions:
   - **ChatGPT** — paste into a Custom GPT's "Instructions" field, or as your first message in a normal chat.
   - **Claude.ai** — create a Project, paste it into the Project's custom instructions.
   - **Gemini** — paste it into a Gem's instructions.
   - **Local model (Ollama, LM Studio, etc.)** — use it as the system prompt.
3. Since these tools can't read your folder by themselves, you carry the state by hand:
   - At the start of a session, paste in the contents of `cognitive-model/MODEL.md` and any relevant `hypotheses/*.md` files (or upload them, if your tool supports file/knowledge attachments — ChatGPT Projects, Claude Projects, and Gemini Gems all do).
   - When the model finishes, copy its output into a new file under `cognitive-model/sessions/` yourself, following the schema in `cognitive-model/TEMPLATES.md`.
   - Repeat the same paste-in / copy-out pattern for the other two agents.
4. It's more manual than Claude Code, but the actual thinking — the hypotheses, the evidence discipline, the experiments — works identically on any model capable of following instructions.

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
```

## First run

Don't know where to start? Read the worked example at [`cognitive-model/hypotheses/H000-example-notification-checking.md`](cognitive-model/hypotheses/H000-example-notification-checking.md) to see a hypothesis filled out end to end, then delete it and run:

```
use the cognitive-systems-analyst to investigate <something you actually keep noticing about yourself>
```

## Not therapy

This is a self-reflection and journaling framework, not a medical or mental-health tool. It's built to be honest, not kind — that's the point, but it also means it's the wrong tool for a crisis. If you're dealing with trauma, a mental health condition, or you're in crisis, please talk to a licensed professional or a crisis line instead.

## Privacy note

Once you start using this for yourself, `cognitive-model/hypotheses/`, `sessions/`, and `experiments/` will fill up with real, potentially sensitive information about you. If you forked this repo to run your own model, consider making that fork **private**, or `.gitignore`-ing those three folders and keeping your real data local only — see the commented-out section in [`.gitignore`](.gitignore).

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, change it, send it to your friends.
