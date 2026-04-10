---
name: post-generator
description: Produces clean structural LinkedIn post drafts for executives. Voice-agnostic — focuses on content structure, argument, and formatting only. Voice DNA is NOT applied here. Receives exec name, role, and audience summary; a validated angle (topic, hook, key claims); and research context. Output: a clean structural draft ready for QA.
---

# Post Generator

**Inputs:** Exec name, role, and audience summary + a validated angle (topic, hook, key claims) + research context.
**Output:** A clean structural draft ready for the QA pipeline.

**Context discipline:** This skill does not receive or apply Voice DNA. No voice-matching here. The draft should be structurally sound and content-accurate — the qa-pipeline handles voice rewriting.

---

## Post Structure (Always Follow This)

```
[HOOK — 1-2 sentences]
Question, surprising stat, contrarian claim, or micro-story opening.
No "I'm excited to share..." No "In today's fast-paced world..."
No hollow openers. Get to the point immediately.

[INSIGHT BLOCK — 2-3 short paragraphs]
The actual idea. Support it with:
  → A specific data point or claim (use the provided research context)
  → A concrete example, scenario, or real situation
  → A takeaway the reader can use

[CLOSE — 1 paragraph]
Value summary + one direct engagement question.
Not "Let me know your thoughts!" — one specific question that invites a real answer.

[HASHTAGS — 3-5, always last line]
#RelevantHashtag #IndustryTerm #TopicTag
```

---

## Post Type Classification

Classify the post before drafting. This determines the hook style and CTA approach.

```
What is the primary purpose of this post?

├── Share an opinion on industry news or trends
│   └── TYPE: Commentary
│       Hook style: Contrarian take, dry humor, or bold disagreement
│       CTA: "What's your read on this?"
│       Tone target: Direct, opinionated, conversational

├── Share a lesson, story, or hard-won insight
│   └── TYPE: Thought Leadership
│       Hook style: Framework reveal, counterintuitive claim, or strong metaphor
│       CTA: "What's worked for your team?" or specific practitioner question
│       Tone target: Smart but accessible, proof-backed, no hype

├── Celebrate a milestone, launch, or win
│   └── TYPE: Announcement
│       Hook style: Lead with the result, not the excitement
│       CTA: Soft invite to learn more or quiet credit to the team
│       Tone target: Grounded, honest framing, let the number do the work

└── Share something personal: failure, pivot, behind-the-scenes
    └── TYPE: Personal Story
        Hook style: Vulnerable or self-deprecating moment, specific detail
        CTA: Question that invites readers to share their own experience
        Tone target: Human, slightly imperfect, never self-congratulatory
```

---

## Formatting Rules

- 3–5 paragraphs max. No walls of text.
- Short paragraphs. 1–4 sentences each.
- Line breaks between paragraphs (LinkedIn-style spacing).
- 1–3 emojis max. Use them sparingly — only where they add warmth, not noise.
- Contractions: yes. Passive voice: minimize.
- No headers inside the post. No bullet lists inside the post.
- Target: under 250 words. Hard ceiling: 350 words. Flag if over.
- End with exactly one question. Not two. One.

---

## Hook Library

A quick-reference set of hook patterns by post type. Use these as starting points — always ground the actual hook in the provided research context.

**Thought Leadership**
- Contrarian: "Most [industry] leaders get [topic] wrong. Here's what the data shows."
- Metaphor hook: "[Thing A] isn't [what people think]. It's [unexpected reframe]."
- Counterintuitive: "The best [outcome] I've seen didn't come from [obvious source]. It came from [surprise]."

**Commentary**
- Dry undercut: "[Big scary claim]. [One-word or two-word defuse]."
- Industry frustration: "[Specific problem]. That's all I'm asking. [Build the case]."
- Prediction: "In 12 months, [claim]. Here's why I'm already positioning for it."

**Announcement**
- Result-first: "We just [did X]. It took [timeframe] and one decision I wasn't sure about."
- Team-first: "[Person] said something in our planning session that changed how we built [thing]."
- Honest numbers: "[Metric]. Here's what worked, what didn't, and what we'd do differently."

**Personal Story**
- Self-deprecating specific: "You think you're [competent thing]… then [humbling specific moment]."
- Vulnerable opener: "I made a call last quarter I wasn't sure about. Still not sure it was right."
- Behind-the-scenes: "What you don't see in [visible success] is the [unglamorous truth]."

---

## Voice Placeholder Rules

Since Voice DNA does not apply here, write in a clean, clear, professional tone:
- Active voice throughout
- Plain language — no jargon unless it's the exec's established domain
- No hollow amplifiers: not "incredible," "amazing," "powerful"
- No AI red-flag vocabulary (see qa-pipeline for the full list — avoid these preemptively)
- Sentences should vary in length — not all 15–20 words

The voice-agnostic draft will be transformed into the exec's authentic voice by the qa-pipeline's Pass 2 (Voice DNA rewrite). Prioritize structural clarity and content accuracy here.

---

## Posts to Emulate (Style Reference)

These examples illustrate what good structure and voice look like. Study these before drafting — they show the target quality for hooks, paragraph breaks, and CTAs.

**EXAMPLE 1 — Allison Rossi style**
*Funny, contrarian, earns the laugh without trying*

```
"AI is going to take your job."

beep — boop — nope

If multiple AI LLMs can confidently explain that an upside-down cup is
"not functional," I think we will all be just fine...
```

**What makes it work:** Hook weaponizes the reader's fear, then defuses it with dry humor in two words. Zero jargon. Sharp observation, genuine recommendation.

**Pattern to steal:** State the fear → subvert it immediately → add one specific, real example that proves the point.

---

**EXAMPLE 2 — Colby Kultgen style**
*Short, reframing, no wasted words*

```
Productivity isn't just about "getting more done".

It's about spending the short amount of time you have on this planet
intentionally, rather than losing it to distraction.
```

**What makes it work:** Reframes a familiar concept in 27 words. The line break creates a micro-pause that makes the second sentence land harder.

**Pattern to steal:** Familiar phrase in quotes → pivot word ("It's about") → bigger, more human truth → clean CTA.

---

**EXAMPLE 3 — Nils Smed style**
*Strong paragraph separation, builds frustration deliberately, payoff lands hard*

```
An API-first consumer bank.

That's all I'm asking.

Money is already software. Let me get real access to my financial data.
Not through the bank's UI. Not through a download button and excel sheets.

Let me build my own financial solutions.
```

**What makes it work:** Every paragraph does one job. Fragments act as punctuation marks. Repetition ("Not through...", "Let me...") is rhythm, not laziness. Frustration escalates in a controlled way.

**Pattern to steal:** Short declarative statement → expand with specifics → build frustration through repetition → one-word gut-punch → direct close.

---

**EXAMPLE 4 — Jonathan Gonzalez style**
*Strong metaphor hook, earned soft close*

```
The first 10 hires aren't just employees - they're a transplant of your own DNA.

They set the "standard" for the next 50 people who join.

When every seat is this high-stakes, the pressure to find a "perfect"
person becomes a bottleneck.
```

**What makes it work:** "DNA transplant" stops scrolling. Builds the case methodically. Soft sell lands last, framed as caring about the problem, not closing a deal.

**Pattern to steal:** Provocative metaphor hook → stakes (why this matters) → the tension/problem → what good looks like → personal close.

---

## Posts to Avoid

**NEVER produce:**
- "I'm thrilled / excited / humbled to announce..."
- "In today's rapidly evolving landscape..."
- "Game-changing" / "disruptive" / "revolutionary"
- Bullet lists inside a LinkedIn post
- More than 3 hashtags at the end
- Multiple questions at the end — pick one
- Vague CTAs like "Let me know your thoughts!"
- A post that's really a sales pitch in disguise
- More than 3 emojis
- Self-aggrandizing closers ("just warming up," "watch this space")

---

## LinkedIn Best Practices (2026 Algorithm)

**Value-first ranking:** LinkedIn's 2026 algo heavily favors posts where readers spend time and scroll slowly. Front-load the insight. Don't bury the value.

**Dwell time > reach bait:** A post that makes someone stop and think outperforms a post with a controversial hook but no substance. Aim for both.

**Conversation signals:** Comments (especially back-and-forth replies by the original poster) are the strongest engagement signal. Design the CTA to invite genuine responses, not just reactions.

**Under 2 minutes:** Posts over 350 words see a measurable drop in completion rate.

**No links in the post:** LinkedIn suppresses posts with external URLs. If a link is needed, put it in the first comment and reference it in the post ("link in comments").

**Best posting windows (2026):**
- Tuesday–Thursday: 7–9 AM or 12–1 PM local exec time
- Avoid Monday mornings and Friday afternoons
- For global audiences: 9 AM GMT often performs well

**Native content only:** Single-image or text-only posts perform best for thought leadership.

---

## Output Format

```
📝 STRUCTURAL DRAFT — [Exec Name] — [Angle Title] — [Post Type]

---

[Full post text here]

---

Word count: [X]
Post type: [Type]
Hook pattern used: [Pattern name — e.g., "Contrarian undercut"]
Hashtags: [list]
```

---

## Error Handling

| Situation | Response |
|---|---|
| Research context is thin | Draft with what's available. Flag in output: "Limited research context — QA pipeline fact-check pass will be important here." |
| Post exceeds 350 words | Flag it in the output. Do not cut arbitrarily — the qa-pipeline's Pass 3 will tighten. If still over after noting, cut the least essential insight paragraph. |
| Topic too vague to structure | Output one clarifying question before drafting. |
| Post type is ambiguous | Default to Thought Leadership and note the assumption in the output. |
