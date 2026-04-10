# Executive Social Manager Plugin

Your LinkedIn ghostwriting workflow, fully autonomous except for one mandatory human review gate. Supports up to 10 executives — each with their own voice, audience, and content pillars.

---

## Quick Start — Which Command Do I Use?

| I want to... | Command | What happens |
|---|---|---|
| Draft a post from a topic I already have | `/draft [exec] [topic]` | Quick angles → draft → full QA → review gate → output |
| Draft posts for 2-3 execs on the same topic | `/draft [exec1, exec2] [topic]` | Runs the full workflow for each, sequentially |
| React to an article or news item | `/react [exec] [URL or text]` | Ingests source → drafts exec's take → QA → review gate → output |
| QA a draft I already wrote | `/qa [exec]` | Paste your draft → fact-check + voice + anti-AI → review gate → output |
| Deep-dive research for planned content | `/research [exec]` | Phase 1 of 3: gathers raw research (news, community, trends) |
| Validate that research | `/validate-research [exec]` | Phase 2 of 3: fact-checks and alignment-checks the research |
| Generate angles from validated research | `/generate-angles [exec]` | Phase 3 of 3: produces typed angle tickets ready for `/draft` |
| Calibrate an exec's voice | `/improve-workflow [exec]` | Paste feedback or edits → proposes Voice DNA updates for approval |

**For most day-to-day use, `/draft` is the command you want.** The 3-phase research pipeline (`/research` → `/validate-research` → `/generate-angles`) is for planned content calendars where you want data-backed angles — not for daily post generation.

---

## What a Real Session Looks Like

You type: `/draft Jesse AI content authenticity for solo marketers`

The orchestrator runs:

1. Loads Jesse's minimal profile (name, role, audience, content pillars — no Voice DNA yet)
2. Generates 2–3 distinct angle options from the profile and topic
3. Presents angles and asks you to pick one

You pick → the workflow continues:

4. Drafts a structural post (content-focused, no voice yet)
5. Runs the automated QA pipeline:
   - Pass 1: Fact-checks all claims, auto-fixes or reframes
   - Pass 2: Loads Voice DNA, rewrites to authentic exec voice
   - Pass 3: Strips AI vocabulary, varies sentence rhythm, adds human feel

Then it stops:

```
🛑 READY FOR YOUR REVIEW — STEP 6

[polished post — post all three QA passes]

[speech]    → change how this sounds/reads aloud
[sentence]  → fix a run-on or awkward sentence
[section]   → rewrite or remove an entire paragraph
[data]      → change a statistic or claim

Reply "approved" or "[tag] your note here"
```

You review → type **"approved"**

6. Outputs the formatted post block:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
READY TO POST — Jesse Kinzer
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[post]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Word count: 214  |  Emojis: 1  |  Est. read time: 52s
Hashtags: #AIMarketing #ContentStrategy #MarketingOps
Best window: Tue–Thu, 7–9am or 12–1pm
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

You copy → paste → done.

---

## Adding Executives

Each executive needs a profile file in `resources/exec-profiles/`.

- Jesse's file: `jesse-kinzer.md` (use as the reference format for new profiles)
- For each new exec, copy `ADD-YOUR-OTHER-9-HERE.md` and fill in their profile
- File naming: `Firstname-Lastname.md` (hyphenated)
- Reference them in commands by first name: `/draft Austin [topic]`

---

## Design Principles

- **One human gate.** The QA pipeline runs fully automated (fact-check + voice + anti-AI) before you ever see a draft.
- **Voice DNA loads late.** Voice isn't applied until the QA pipeline — keeping early ideation and drafting steps lean.
- **Targeted feedback syntax.** The review gate uses comment tags (`[speech]`, `[data]`, etc.) so re-runs target only the affected pass, not the whole pipeline.
- **Voice over template.** Each exec's Voice DNA overrides any structural default.
- **`/draft` for daily use, research pipeline for planning.** Don't burn context on the 3-phase research pipeline for a quick post.

---

Version 4.0 — Mar 2026
