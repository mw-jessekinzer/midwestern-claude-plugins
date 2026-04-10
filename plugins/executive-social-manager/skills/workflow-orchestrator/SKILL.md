---
name: workflow-orchestrator
description: Orchestrates all LinkedIn ghostwriting entry points. Triggered by /draft, /draft-for, /react, /qa, /batch-ideas, or natural-language equivalents. Calls post-generator and qa-pipeline skills. Enforces a single human review gate. No external integrations.
---

# Executive Social Manager — Workflow Orchestrator

**Version:** 3.0 | **Author:** Jesse Kinzer

---

## Core Rules (Never Break These)

1. **Always load the exec profile before doing anything else.** If no profile exists, stop and ask the user to add one.
2. **Never skip the human gate.** Do not proceed past the review gate without an explicit "approved" from the user.
3. **Voice DNA loads late — except in MODE 4.** In Modes 1, 2, and 3, do not load or reference Voice DNA until the QA pipeline step. This is intentional — it keeps early context lean. In MODE 4 (`/qa`), Voice DNA loads upfront because the draft already exists.
4. **Single post preferred.** Batch mode = ideation only. Do not draft full posts in batch mode unless the user explicitly selects angles to develop.
5. **Voice DNA wins.** When an exec's voice profile conflicts with any skill's structural default, the exec's voice wins every time.

---

## Exec Profile System

### How to Find the Right Profile

Executive profiles live in `resources/exec-profiles/`. Files may use any capitalization or hyphen style (e.g., `Austin-Daniel.md`, `jesse-kinzer.md`, `Kahlie-Huff.md`). **Always match case-insensitively.**

**Name matching rules (in priority order):**

1. **Exact first name match in the profile header.** The header line of every profile reads `# VOICE PROFILE: [Full Name] — [Title]`. Extract the first name from that line and match case-insensitively against what the user typed.

2. **First name match in the filename.** If the header match fails, check filenames case-insensitively for the first name the user typed.

3. **If multiple profiles match the same first name** (e.g., Austin Daniel and Austin Edison): do not guess. Stop and ask:

```
⚠️ I found two executives named Austin:
  → Austin Daniel — Chief Growth Officer
  → Austin Edison — Design Director

Which Austin would you like me to draft for?
```

4. **If no profile matches at all:**

```
⚠️ No profile found for "[Name]".

To add this executive, copy the template from:
resources/exec-profiles/ADD-YOUR-OTHER-9-HERE.md

Fill in their profile and save it as:
resources/exec-profiles/[Firstname-Lastname].md

Then run the command again.
```

**Always skip `ADD-YOUR-OTHER-9-HERE.md` — it is a template, not an active exec profile.**

### Active Execs for Batch Mode

In batch-ideas mode, load all `.md` files in `resources/exec-profiles/` that are not the `ADD-YOUR-OTHER-9-HERE.md` template. These are your active exec profiles.

---

## MODE 1 — Draft (`/draft`, `/draft-for`)

**When to use:** User has a topic or angle in mind. No external source, no research pipeline.

Run all steps in sequence. Label each step output clearly. Do not skip steps.

**Honesty label:** No research pipeline ran. Post is grounded in the exec's profile and stated topic. If any supporting data is included in the draft, label it `[ESTIMATED — not validated]` in the QA summary.

---

### STEP 1 — Load Minimal Profile

Load the exec's profile file from `resources/exec-profiles/`.

**Only extract and display:**
- Full name and title
- Audience summary (who follows them, what they care about)
- Content pillars (3–5 topics they own)
- Topic provided by user

**Do NOT load or display Voice DNA at this step.** Voice DNA loads during the QA pipeline.

Output:
```
✅ EXECUTIVE LOADED — STEP 1

Name: [Full Name, Title]
Audience: [Audience summary from profile]
Content pillars: [Pillars from profile]
Topic: [Topic provided by user]
```

---

### STEP 2 — Generate Quick Angles

Using the exec's minimal profile (name, role, audience, content pillars) and the provided topic, generate 2–3 distinct angle options. Draw on the exec's profile and current knowledge only.

Each angle should differ meaningfully in framing or POV — not just the hook wording.

Output:
```
💡 ANGLE OPTIONS — [Exec Name] — STEP 2

Angle A: [Hook / framing]
Why it works: [1 sentence — why this angle fits this exec's audience and pillars]

Angle B: [Hook / framing]
Why it works: [1 sentence]

Angle C: [Hook / framing]  ← optional third option if topic supports it
Why it works: [1 sentence]

→ Reply with "A", "B", or "C" to develop that angle.
→ Or reply "just go" to use Angle A.
→ Or describe your own angle.
```

---

### STEP 3 — User Selects Angle

**GATE: Wait for user input before proceeding.**

If the user replies with a letter, a number, "just go", or their own framing — accept it and proceed to Step 4.

---

### STEP 4 — Draft Post

Call the `post-generator` skill. Pass it:
- The exec's name, role, and audience summary
- The selected angle from Step 3
- Instruction: produce a structural draft — content-focused only, no voice application yet

**Do not pass Voice DNA to post-generator.** That loads at Step 5.

Output:
```
📝 DRAFT COMPLETE — STEP 4

Structural draft ready for [Exec Name] on: [angle]
Running QA pipeline now...
```

---

### STEP 5 — Automated QA Pipeline

Call the `qa-pipeline` skill. Pass it:
- The structural draft from Step 4
- The exec's **full profile** (all sections, including Voice DNA — this is the first time Voice DNA is loaded)
- The exec's name and topic for the summary header
- Flag: **No research pipeline ran** — any supporting data injected by the model should be labeled `[ESTIMATED — not validated]` in the QA summary

The qa-pipeline runs three sequential passes automatically:
1. Fact check — verify all claims, auto-fix or reframe; label any model-generated data as `[ESTIMATED — not validated]`
2. Voice DNA rewrite — apply exec's authentic voice
3. Anti-AI humanization — strip AI vocabulary, vary sentence rhythm, add human imperfection

No human input between passes. Display the full QA summary and polished draft from the pipeline output.

---

### STEP 6 — Human Review Gate

**HARD STOP. Do not proceed until the user explicitly approves.**

Display the polished draft from Step 5 with the comment syntax guide:

```
🛑 READY FOR YOUR REVIEW — STEP 6

[Full polished post — post all three QA passes]

────────────────────────────────────

HOW TO GIVE FEEDBACK:

[speech]    → change how this sounds/reads aloud
[sentence]  → fix a run-on or awkward sentence
[section]   → rewrite or remove an entire paragraph
[data]      → change a statistic or claim

EXAMPLES:
→ "approved"
→ "[speech] the opening line feels too formal for Austin"
→ "[data] the 83% stat should be updated to our Q4 number"
→ "[section] the second paragraph buries the point — tighten it"

────────────────────────────────────

Reply "approved" to generate the final output block.
Reply with [tag] + note for any changes.
```

**On changes:** Re-run only the affected pass(es) in the qa-pipeline, not the full pipeline.
- [speech] or [sentence] → re-run Pass 3 (anti-AI) only
- [section] → re-run Passes 2 and 3 (voice + anti-AI)
- [data] → re-run Passes 1 and 3 (fact-check + anti-AI)

Do not proceed until the user replies "approved."

---

### STEP 7 — Formatted Output Block

Generate and display the final formatted output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
READY TO POST — [Exec Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Full post text]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Word count: X  |  Emojis: X  |  Est. read time: Xs
Hashtags: #tag1 #tag2 #tag3
Best window: Tue–Thu, 7–9am or 12–1pm
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After the output block:
```
✨ WORKFLOW COMPLETE

The post is ready to copy and schedule.

Want to draft another post? Type /draft [exec] [topic]
Want to react to an article? Type /react [exec] [URL or text]
Want to QA a draft you wrote? Type /qa [exec]
Want to generate a week of ideas? Type /batch-ideas
```

---

## MODE 2 — Batch Ideas (`/batch-ideas`)

Run Steps 1–3 of Mode 1 only for all active executives (or a specified exec). No drafting unless user selects angles.

1. Load minimal profile for each active exec (name, role, audience, content pillars — no Voice DNA)
2. For each exec, generate 5–8 angle ideas from exec profile and current knowledge
3. Output the master idea bank:

```
📅 BATCH IDEAS — [Date]

[EXEC NAME 1]
  → Topic #1: [Angle title] | Why now: [1 sentence] | Fit: [why it works for this exec]
  → Topic #2: [...]
  → Topic #3: [...]

[EXEC NAME 2]
  → [...]

Which ones should I develop into full drafts?
Reply: "[Exec name] / [Topic number]" for each one you want developed.
```

For any selected angles, run the full Mode 1 Steps 4–7 workflow for that exec/angle pair.

---

## MODE 3 — React (`/react`)

**When to use:** User found an article, tweet, or news item and wants the exec's take on it.

Run all steps in sequence. Label each step output clearly.

**Key constraint:** The source is not validated. The system reads it for context — the exec's angle is primary, not a summary of the source. QA Pass 1 checks claims the *draft makes*, not whether the source is correct.

---

### STEP 1 — Load Minimal Profile

Load the exec's profile file from `resources/exec-profiles/`.

**Only extract and display:**
- Full name and title
- Audience summary
- Content pillars

**Do NOT load or display Voice DNA at this step.**

Output:
```
✅ EXECUTIVE LOADED — STEP 1

Name: [Full Name, Title]
Audience: [Audience summary from profile]
Content pillars: [Pillars from profile]
Source: [URL or first 100 chars of pasted text]
```

---

### STEP 2 — Ingest Source

If a URL was provided: fetch the page and extract the core claim or premise (1–3 sentences). Do not summarize the full article — extract only the central idea the exec will react to.

If pasted text was provided: parse it and extract the core claim or premise (1–3 sentences).

Output:
```
📰 SOURCE INGESTED — STEP 2

Core premise: [1–3 sentence extraction of the central claim]

Note: This source has not been validated. QA will check claims the draft makes, not the source itself.
```

---

### STEP 3 — Draft Exec's Reaction

Call the `post-generator` skill. Pass it:
- The exec's name, role, audience summary, and content pillars
- The core premise extracted in Step 2 as context/launching pad
- Instruction: draft the exec's reaction — the source is inspiration, not content to summarize. The exec's angle, opinion, or insight is the post. The source is mentioned briefly (if at all) to frame what the exec is reacting to.

**Do not pass Voice DNA to post-generator.** That loads at Step 4.

Output:
```
📝 REACTION DRAFT COMPLETE — STEP 3

Structural draft ready for [Exec Name] reacting to: [source title or URL]
Running QA pipeline now...
```

---

### STEP 4 — Automated QA Pipeline

Call the `qa-pipeline` skill. Pass it:
- The structural draft from Step 3
- The exec's **full profile** (all sections, including Voice DNA — this is the first time Voice DNA is loaded)
- The exec's name and source context for the summary header
- Flag: **Fact-check the draft's claims only** — do not evaluate the source for accuracy

The qa-pipeline runs three sequential passes automatically:
1. Fact check — verify claims the *draft* makes (not the source); auto-fix or reframe
2. Voice DNA rewrite — apply exec's authentic voice
3. Anti-AI humanization — strip AI vocabulary, vary sentence rhythm, add human imperfection

No human input between passes. Display the full QA summary and polished draft.

---

### STEP 5 — Human Review Gate

**HARD STOP. Do not proceed until the user explicitly approves.**

Display the polished draft with the comment syntax guide (same format as Mode 1 Step 6).

On changes: Re-run only the affected pass(es) — same rules as Mode 1.

Do not proceed until the user replies "approved."

---

### STEP 6 — Formatted Output Block

Generate and display the final formatted output (same format as Mode 1 Step 7).

After the output block:
```
✨ WORKFLOW COMPLETE

The post is ready to copy and schedule.

Want to react to another article? Type /react [exec] [URL or text]
Want to draft from a topic? Type /draft [exec] [topic]
Want to generate a week of ideas? Type /batch-ideas
```

---

## MODE 4 — QA (`/qa`)

**When to use:** User already wrote a draft. They want voice matching, fact-checking, and humanization only.

**This is the only mode that loads Voice DNA before any drafting.** The draft already exists — Voice DNA is needed immediately.

---

### STEP 1 — Load Full Profile

Load the exec's **full profile** from `resources/exec-profiles/` — all sections including Voice DNA.

Output:
```
✅ EXECUTIVE LOADED — STEP 1

Name: [Full Name, Title]
Voice DNA: loaded
```

---

### STEP 2 — Request Draft

**GATE: Wait for user to paste their draft.**

Output:
```
📋 PASTE YOUR DRAFT — STEP 2

Please paste the draft you'd like me to QA for [Exec Name].
```

Wait for the user to paste their draft. Accept it as-is and proceed to Step 3. Do not edit, critique, or comment on the draft before running QA.

---

### STEP 3 — Automated QA Pipeline

Call the `qa-pipeline` skill. Pass it:
- The draft pasted by the user
- The exec's full profile (Voice DNA already loaded in Step 1)
- The exec's name for the summary header

The qa-pipeline runs three sequential passes automatically:
1. Fact check — verify all claims, auto-fix or reframe
2. Voice DNA rewrite — apply exec's authentic voice
3. Anti-AI humanization — strip AI vocabulary, vary sentence rhythm, add human imperfection

No human input between passes. Display the full QA summary and polished draft.

---

### STEP 4 — Human Review Gate

**HARD STOP. Do not proceed until the user explicitly approves.**

Display the polished draft with the comment syntax guide (same format as Mode 1 Step 6).

On changes: Re-run only the affected pass(es) — same rules as Mode 1.

Do not proceed until the user replies "approved."

---

### STEP 5 — Formatted Output Block

Generate and display the final formatted output (same format as Mode 1 Step 7).

After the output block:
```
✨ WORKFLOW COMPLETE

The post is ready to copy and schedule.

Want to QA another draft? Type /qa [exec]
Want to draft from a topic? Type /draft [exec] [topic]
Want to react to an article? Type /react [exec] [URL or text]
```

---

## Error Handling

| Situation | Response |
|---|---|
| Exec profile not found | Stop. Show exact instructions to add profile. Do not guess or draft without a profile. |
| URL fetch fails in MODE 3 | Ask user to paste the article text directly. |
| User tries to skip the human gate | "This step requires your review before I can continue. Please reply 'approved' or use [tag] comment syntax." |
| Fact-check flags a claim the user wants to keep | "Noted — I'll reframe it as your personal observation. Updating now." |
| Post is over 350 words after drafting | Note it in the QA pipeline summary. QA pipeline should tighten. If still over 350 after passes, flag at the review gate. |
| User provides a topic in a different language | Produce the post in that language. Confirm language with user first. |
| QA pipeline re-run requested at review gate | Re-run only the affected pass(es). Show updated draft and reset the approval gate. |
| [ESTIMATED] data in MODE 1 or MODE 3 draft | Flag in QA summary: "Supporting data included is model-estimated and not research-validated. Labeled [ESTIMATED — not validated]." |
