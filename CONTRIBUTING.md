# Contributing to Prism

Thanks for wanting to help! Prism is a small, friendly project, and contributions of every size are welcome — a typo fix counts.

**English** · فارسی (نسخه‌ی فارسی در پایین)

## Ways to contribute

- **Fix or improve the docs.** Clearer wording, better examples, fixed typos.
- **Translate.** Docs in more languages are hugely valuable (English and Persian are here already).
- **Improve an agent.** Sharpen the instructions in `.claude/agents/` — but keep the core invariants below.
- **Add a worked example.** A clear, *fictional* example hypothesis or experiment that helps newcomers.
- **Report a bug or suggest an idea.** Open an issue — templates are provided.

## The one hard rule: never commit personal data

Prism is a tool for private self-reflection. **Do not put real personal data in a pull request** — no real hypotheses, sessions, or experiments about yourself or anyone else. The repo ships only a clearly-labeled fictional example (`H000`). Your own real model stays on your machine; it's git-ignored by default.

## Keep the architecture invariants

If you touch the agents, preserve what makes the system trustworthy:

- **Propose vs. commit.** The analyst and experiment-designer only *propose* changes; only the historian *writes* `hypotheses/` and `MODEL.md`. Don't give the other agents write access to the canonical model.
- **Truth over comfort.** The agents are honest, not flattering. They don't diagnose and they don't motivate.
- **Falsifiable and sourced.** Confidence never hits `1.00`, and every change cites a source.

## How to submit a change

1. Fork the repo and create a branch.
2. Make a focused change.
3. If you edited docs, check that links and headings still resolve.
4. Open a pull request describing what changed and why.

Be kind, be honest — the same rule the tool lives by.

---

<div dir="rtl">

## مشارکت در پریزم

ممنون که می‌خواهی کمک کنی! پریزم پروژه‌ای کوچک و صمیمی است و هر مشارکتی — حتی اصلاحِ یک غلطِ تایپی — ارزشمند است.

### راه‌های مشارکت

- **بهبودِ مستندات:** واضح‌ترکردنِ متن، مثال‌های بهتر، رفعِ غلط‌های تایپی.
- **ترجمه:** مستندات به زبان‌های بیشتر بسیار ارزشمند است.
- **بهبودِ عامل‌ها:** دستورالعمل‌های داخلِ `.claude/agents/` را دقیق‌تر کن — اما اصولِ اصلیِ پایین را حفظ کن.
- **افزودنِ نمونه:** یک نمونه‌ی *خیالیِ* واضح که به تازه‌واردها کمک کند.
- **گزارشِ باگ یا پیشنهاد:** یک issue باز کن؛ قالب‌ها آماده‌اند.

### تنها قانونِ سختگیرانه: هیچ‌وقت داده‌ی شخصی را commit نکن

پریزم ابزاری برای خودکاویِ خصوصی است. **هیچ داده‌ی شخصیِ واقعی‌ای را در pull request نگذار** — نه فرضیه، نه session، و نه آزمایشِ واقعی درباره‌ی خودت یا هرکسِ دیگر. مدلِ واقعیِ تو روی دستگاهِ خودت می‌ماند و به‌طورِ پیش‌فرض در git نادیده گرفته می‌شود.

### حفظِ اصولِ معماری

اگر عامل‌ها را تغییر می‌دهی، چیزی را که سیستم را قابل‌اعتماد می‌کند حفظ کن: قانونِ «پیشنهاد در برابر ثبت» (فقط historian در مدلِ اصلی می‌نویسد)، «حقیقت مهم‌تر از دلداری»، و اینکه ضریبِ اطمینان هیچ‌وقت به ۱٫۰۰ نمی‌رسد و هر تغییر منبع دارد.

</div>
