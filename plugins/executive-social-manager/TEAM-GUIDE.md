# Executive Social Manager — Team Guide

**Version 4.0 · March 2026**

---

## TLDR

This plugin runs inside Claude Code. You type a slash command, Claude ghostwrites a LinkedIn post for a specific executive, puts it through 3 automated QA passes (fact-check → voice match → anti-AI), then stops and asks you to approve before delivering the final output. One human review gate. Everything else is automated.

**95% of the time, you'll only use one command: `/draft`**

---

## The Commands

### `/draft [exec] [topic]`
The everyday command. Give it an exec name and a topic — it handles everything.

```
/draft Jesse AI content authenticity for solo marketers
/draft Austin how trade tariffs are affecting B2B SaaS
```

**Batch mode** (max 3 execs, same topic):
```
/draft Jesse, Austin, Kahlie attribution and marketing ROI
```

What happens:
1. Loads exec's profile
2. Generates 2–3 angle options — you pick one (or say "just go")
3. Drafts the post
4. Runs 3-pass QA automatically (fact-check → voice → anti-AI strip)
5. Stops for your review
6. On "approved" → outputs the final post block, ready to copy-paste

---

### `/react [exec] [URL or pasted text]`
Drop in an article, news item, or any text — Claude drafts the exec's take on it.

```
/react Jesse https://example.com/article-about-ai-tools
/react Austin [paste article text here]
```

The source is inspiration, not a summary. The exec's perspective is what drives the post.

---

### `/qa [exec]`
Already have a draft written by the exec or someone else? Run it through the full QA pipeline.

```
/qa Jesse
→ Claude prompts: "Paste the draft"
→ Runs all 3 QA passes → review gate → final output
```

---

### `/research [exec]` → `/validate-research [exec]` → `/generate-angles [exec]`

**The 3-phase research pipeline.** Use this for planned content calendars, not daily posts. It's slower and more thorough.

| Phase | Command | What it does |
|---|---|---|
| 1 | `/research [exec]` | Gathers news, trends, and community signals relevant to this exec |
| 2 | `/validate-research [exec]` | Fact-checks and scores the research |
| 3 | `/generate-angles [exec]` | Produces 8 typed angle tickets, each with a ready `/draft` command |

Run all three in order. Phase 3 outputs 8 angles you can hand off directly to `/draft`.

---

### `/improve-workflow [exec]`
Voice calibration. Use this when the exec reviews a draft and makes edits, or when the voice keeps drifting.

```
/improve-workflow Jesse
→ Paste the exec's edits or feedback
→ Claude analyzes what the edits reveal about voice gaps
→ Proposes specific Voice DNA changes
→ You type "apply" → get the updated profile text to copy in
```

Nothing changes automatically. You approve every update.

---

## The Review Gate

Every command (except `/research` and `/validate-research`) pauses here before final output:

```
🛑 READY FOR YOUR REVIEW

[polished post]

[speech]    → change how this sounds/reads aloud
[sentence]  → fix a run-on or awkward sentence
[section]   → rewrite or remove an entire paragraph
[data]      → change a statistic or claim

Reply "approved" or "[tag] your note here"
```

**To approve:** type `approved`

**To request a change:** use the tag syntax
```
[speech] make the opening less formal
[data] the 83% burnout stat needs a source
[section] cut the third paragraph entirely
```

Tags target only the relevant QA pass — it doesn't re-run the whole pipeline.

---

## Adding a New Executive

Each exec needs a profile file in `resources/exec-profiles/`.

1. Copy `ADD-YOUR-OTHER-9-HERE.md`
2. Fill in their profile (use `jesse-kinzer.md` as the reference)
3. Save as `Firstname-Lastname.md`
4. Reference them in commands by first name: `/draft Austin [topic]`

The profile includes: origin story, role, audience, content pillars, Voice DNA, and validated audience pain points. A thin profile still works — Voice DNA gets richer over time as you run `/improve-workflow` after exec reviews.

---

## When to Use What

| Situation | Command |
|---|---|
| Daily post for an exec | `/draft [exec] [topic]` |
| Exec wants to react to something in the news | `/react [exec] [URL]` |
| Exec wrote a draft and wants it polished | `/qa [exec]` |
| Building out a content calendar for the month | `/research` → `/validate-research` → `/generate-angles` |
| Post voice keeps missing the mark | `/improve-workflow [exec]` |
| New exec just onboarded (thin profile) | Run a few `/draft`s, then `/improve-workflow` after each exec review |

---

## What the QA Pipeline Actually Does

Every draft goes through 3 automated passes before you see it:

**Pass 1 — Fact-check:** Verifies all claims and stats. Auto-fixes or reframes anything unverified. Flags estimated data as `[ESTIMATED]` in a QA summary you can review.

**Pass 2 — Voice match:** Loads the exec's Voice DNA and rewrites the structural draft to match their authentic voice, not just their topic area.

**Pass 3 — Anti-AI strip:** Removes AI vocabulary patterns, varies sentence rhythm, adds human texture. The goal is a post that reads like the exec wrote it.

---

## Key Rules to Know

- **Don't use `/research` for quick posts.** It's for planned calendar content. `/draft` generates angles from the exec's profile and Claude's knowledge — fast and good enough for daily use.
- **Voice DNA improves over time.** The more you run `/improve-workflow` after exec reviews, the better the voice match gets.
- **Exec profiles support up to 10 executives.** Each is fully independent — different voice, audience, and content pillars.
- **The plugin lives in Claude Code.** It's not a web app. Open Claude Code, navigate to this project, and type commands directly in the chat.
