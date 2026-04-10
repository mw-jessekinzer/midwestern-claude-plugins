---
name: self-improver
description: Voice calibration skill for exec profiles. Triggered by /improve-workflow. User pastes exec feedback or edits from a recent draft — skill analyzes the pattern, proposes specific Voice DNA changes with before/after diffs, and waits for "apply" before it writes the change directly to the exec profile file. Nothing changes automatically.
---

# Self Improver

Triggered by `/improve-workflow`. Focused on one thing: calibrating an exec's Voice DNA based on real feedback from drafts.

Interactive — nothing changes automatically. Every proposed change requires explicit "apply" from the user.

---

## How It Works

1. Ask the user what felt off — or ask them to paste edits they made to a recent draft
2. Identify the Voice DNA pattern the feedback points to
3. Propose a specific before/after change to the exec's Voice DNA section
4. Wait for "apply" before providing the updated profile content

---

## Step 1 — Gather Feedback

Open with:

```
What would you like to calibrate?

You can share:
- Edits you made to a recent draft (paste them — I'll spot the voice pattern)
- A specific exec whose voice isn't landing right
- Something that keeps getting changed every time (that's a Voice DNA gap)

Or just describe what happened and what you wish had happened instead.
```

Ask one question at a time. Do not present a form.

---

## Step 2 — Identify the Voice DNA Pattern

Once the user shares feedback, analyze what it reveals about the exec's voice:

**Read the edits as signals:**
- If the user always makes sentences shorter → the exec's voice is punchier than documented
- If the user removes hedging language → the exec's voice is more direct than documented
- If the user adds specific tool names or numbers → the exec's voice is more concrete than documented
- If the user changes the CTA every time → the CTA style in the profile needs updating
- If the user removes certain words consistently → add them to the exec's avoid list

**Name the gap before proposing a fix:**

```
Based on what you shared, it looks like [exec name]'s Voice DNA is missing [specific thing].

Here's what I see in the current profile:
"[Current Voice DNA text]"

Is that the section you'd like to update?
```

---

## Step 3 — Propose the Change

Once confirmed, output a clean before/after diff:

```
PROPOSED VOICE DNA CHANGE — resources/exec-profiles/[filename].md

BEFORE:
"[Current Voice DNA text — exact quote from profile]"

AFTER:
"[Updated Voice DNA text]"

REASON:
[One sentence — what feedback triggered this and what it fixes.]

────────────────────────────────────
Reply "apply" to write the change directly to the file.
Reply "tweak this" to adjust the proposal first.
Reply "skip" to move on.
```

**One change at a time.** If the feedback surfaces multiple gaps, address them sequentially — not all at once. This keeps each change deliberate and easy to review.

---

## Step 4 — Apply (on user request)

When user says "apply":

Use the Edit tool to update the Voice DNA section directly in `resources/exec-profiles/[filename].md`.
Replace only the Voice DNA section text — do not modify any other sections.

After writing:

```
✅ Applied. Voice DNA updated in resources/exec-profiles/[filename].md

The change takes effect in your next session.

Want to calibrate another exec? Paste their feedback and I'll run through the same process.
```

---

## Hard Rules

- **Never apply changes without explicit "apply" from the user.** Proposals only.
- **Never change an exec's voice without the user's review.** Voice is the exec's reputation.
- **One change at a time.** Multiple proposed changes in one block make it hard to review. Sequence them.
- **Only update Voice DNA.** This skill does not modify other profile sections (Audience, Content Pillars, Pain Points). If those need updating, the user edits those directly.
- **Verify the profile exists before proposing changes.** Read the current profile content before drafting a proposal — never propose a change without seeing what's already there.

---

## When to Use Automatic Memories Instead

For lightweight or transient preferences — things that don't warrant a formal Voice DNA update — just say it in conversation. Claude's automatic memories capture preferences across sessions without any command.

**Use automatic memories for:**
- Words an exec wants to avoid in a specific context ("Ryan never references Salesforce")
- Temporary tone notes ("Austin's audience is in Q4 budget mode right now — lean practical")
- Post-level feedback that doesn't reveal a Voice DNA gap

**Use `/improve-workflow` for:**
- Patterns that repeat across multiple drafts (that's a Voice DNA gap)
- Structural voice changes (sentence length, CTA style, hedge language)
- Anything that should be permanent and apply to every future draft for this exec

**Why the distinction matters:** Automatic memories are project-scoped — all execs share the same pool. Voice DNA in exec profiles is exec-scoped and loaded by the qa-pipeline as structured instructions. Use the right tool for the job.
