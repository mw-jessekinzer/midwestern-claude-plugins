---
name: anti-ai-humanization
description: 3-pass humanization skill for LinkedIn executive ghostwriting drafts. Runs after structural drafting and Voice DNA application. Pass 1 = Structural Humanization. Pass 2 = Cognitive Grounding. Pass 3 = Lexical Verification. Outputs a polished, cognitively authentic draft with a change log.
---

# Anti-AI Humanization Skill

Runs on a completed draft that has already passed fact-checking and Voice DNA application. Makes the writing feel like it came from a specific human brain, not a statistical average.

**Input:** A completed draft with Voice DNA applied
**Output:** A humanized draft + change log ready for human review

Three passes run sequentially with no human input. Do not expose intermediate drafts.

---

## PASS 1 — STRUCTURAL HUMANIZATION

Scan the draft for structural AI patterns. Fix every instance silently.

### 1A: Rhetorical Theater — Hard Removal List

Every instance must be removed or rewritten.

| Pattern | Fix |
|---------|-----|
| **Contrarian pivots** ("That's not X. That's Y." / "This isn't a technology problem. It's a people problem.") | Replace with the specific observation that would have led a real person to that conclusion. Ground in a real event or data point. |
| **Fake setup phrases** ("Here's the thing:" / "Let me be direct:" / "Here's what nobody talks about:" / "But here's the kicker:") | Delete the setup. Start with the sentence that follows. If it can't stand alone, it wasn't worth saying. |
| **Manufactured vulnerability** ("I'll be honest..." / "I used to think X. I was wrong." / "Honestly? Most people don't realize...") | Supply the actual failure (specific, low-status, real) or remove the vulnerability framing entirely. |
| **Single-word paragraph emphasis** ("Wild." / "Exactly." / "Think about that." / "Seriously.") | Remove, OR replace with a short sentence (3–6 words) that names the actual reaction. "That number stopped me." is human. "Wild." is not. |
| **Significance inflation closers** ("This is just the beginning." / "The future belongs to those who..." / "We're at an inflection point.") | End on the specific. The last sentence should be the most concrete thing in the post. |
| **Unsolicited life lesson arc** (Problem → Realization → Universal Principle with therapist tone) | Cut the universal principle. End on the realization. If a lesson must be included, bound it explicitly: "At least in B2B SaaS. Your mileage may vary." |
| **Epistemic cowardice hedges** ("Some might argue..." / "It's worth considering both sides..." / "Many would say...") | Replace with the executive's actual position. Strip the hedge. Add a knowledge boundary (see Pass 2) if appropriate. |

**Emerging patterns to also sweep:**
- "Quietly" qualifiers ("the quiet truth," "quietly revolutionizing") → remove or rephrase directly
- Unsolicited coaching questions mid-post ("Are you ready to sit with that?") → cut
- Near-miss metaphors that almost land then don't → rewrite with the literal observation
- Excessive internal coherence markers ("as I mentioned earlier," "circling back to") → remove; human posts have loose ends

---

### 1B: Structural Rhythm — Burstiness Enforcement

**Rule:** After every sentence longer than 25 words, there must be a sentence shorter than 8 words somewhere within the next 3 sentences. Apply mechanically.

**Target range:** 3 words (minimum, sparingly) to 40+ words (maximum). Standard deviation must reflect real variation.

**Apply:**
- Find the longest sentence. If no short sentence follows within 3 sentences, add one.
- Find clusters of similarly-lengthed sentences (3+ sentences all between 12–20 words). Break with a short or long sentence.
- One deliberate fragment per post maximum.

**Symmetry check:** If the post contains a list, check whether all items are the same length and structure. If symmetrical, make items unequal or convert to flowing prose.

---

### 1C: Transition Chain Removal

**Hard removal list:**
Additionally / Moreover / Furthermore / In conclusion / To that end / It's worth noting / It is important to note / On that note / With that said / That being said / This underscores / This highlights / This demonstrates

**Fix options:**
1. Hard paragraph break — let ideas be adjacent without a connector
2. Conceptual leap — last word of previous sentence creates tension the next sentence releases
3. Concrete transitional detail: "Three weeks later..." / "The same meeting, different outcome."

**Em dash audit:** If more than 2, reduce to 1 or 0. Default to a comma or period. Reserve the em dash for cases where neither works.

---

## PASS 2 — COGNITIVE GROUNDING

Addresses the highest-signal failures — the ones that make a post feel technically correct but lifeless.

### 2A: Mundane Detail Injection

Add or strengthen one mundane physical or operational anchor per major claim. The anchor can be a tool, a time, a place, a person's name, a number, a weather condition, a piece of equipment. Anything that proves the writer was actually there.

**What to look for:**
- Sentences describing business events in abstract corporate language
- "We" or "I" statements that could apply to any executive in any industry
- Realizations or turning points that float free of a specific moment

**Examples:**
- "We realized our onboarding process was broken." → "Week three. Same question from the same new hire. That's when I knew."
- "The meeting changed my perspective on customer success." → "She had 47 tabs open and had been on hold with us for 22 minutes before anyone picked up."

**Hapax legomena check:** Scan for vocabulary diversity. If the same 4–5 content words repeat without variation, introduce at least 2–3 domain-specific nouns or verbs that appear only once in the post — a piece of machinery, a proprietary process name, an exact job title, a specific client scenario. These raise vocabulary distribution toward human baselines.

---

### 2B: Asymmetric Expertise Simulation

**What to do:**
- Remove definitions of any term the target reader would already know. Trust the audience.
- Cut preamble. The first sentence should assume the reader is already in the conversation.
- Check for uniform confidence. Find one claim the exec would realistically be uncertain about and introduce a genuine knowledge boundary (see 2C).

---

### 2C: Knowledge Boundary Installation

Find the post's broadest claim. Add a precision boundary — not a hedge, but a scope clarification.

**Boundary examples:**
- "At least in our segment of the market."
- "This is true for companies past Series B. Pre-revenue is a different game."
- "I don't know if this translates to enterprise. We never tried."
- "That math worked for us. It may not for you."

One per post is sufficient. The boundary must be specific — "Your mileage may vary" is too vague. "This assumes you have a sales-led motion, not PLG" is useful and human.

---

### 2D: Anti-Consensus Pressure

Check the post's central claim. Ask: would 95% of professionals in this field agree without hesitation?

If yes, sharpen toward the specific, non-obvious version. Not "great teams communicate well" but "our team communicates worst when things are going well — complacency is harder to diagnose than conflict."

The sharpened claim must be true and defensible. If no sharpening is possible without fabricating a position, flag for the human reviewer.

---

## PASS 3 — LEXICAL VERIFICATION

Final sweep for vocabulary-level AI tells.

### 3A: Banned Vocabulary — Full Removal List

Replace every instance. No exceptions.

| Remove | Replace with |
|--------|--------------|
| Additionally (sentence start) | Also / Plus / And / or cut entirely |
| align with | matches / fits / works with |
| boasts / features | has |
| crucial / critical | important / key / or name the specific consequence |
| delve | explore / look at / dig into |
| enhance | improve / boost / strengthen |
| ensure | make sure / guarantee (if accurate) |
| fostering | building / creating / encouraging |
| game-changing / transformative / revolutionary | name the specific change |
| garner | gain / get / attract |
| highlight (as verb) | show / point out / reveal |
| impactful | effective / or specific to the actual impact |
| in today's rapidly evolving landscape | cut entirely |
| intricate / intricacies | complex / details / nuances |
| it is worth noting | cut entirely; if worth noting, just note it |
| landscape (abstract) | field / area / space |
| leverage (as verb) | use / apply |
| navigate (abstract) | handle / manage / work through |
| pivotal | important / key / or name the actual significance |
| robust | strong / reliable / or name the specific quality |
| seamless | smooth / frictionless / or name the specific quality |
| serves as / stands as | is |
| showcase | show / display |
| significant / significantly | name the actual magnitude ("by 40%" / "across three quarters") |
| tapestry | mix / combination / or cut |
| testament | proof / sign / example |
| thrilled / excited / humbled to announce | cut the framing; just say the thing |
| underscore (verb) | emphasize / show / prove |
| unlock | open / enable / access |
| valuable | useful / helpful / or name the specific value |
| vibrant | lively / active / or name the specific quality |

**2025–2026 emerging flags:**
- "Quietly" as a qualifier → remove
- "Potential" at high frequency → replace with specific projections or remove
- "Significant" at any frequency → always replace with actual magnitude
- Semantic echo chains ("robust," "seamless," "innovative" in sequence) → cut to one specific descriptor

---

### 3B: Contraction and Register Check

Scan for: do not / cannot / will not / it is / they are / we are / I am / would not / should not → Replace with contractions (don't / can't / won't / it's / they're / we're / I'm / wouldn't / shouldn't).

Exception: If the exec's Voice DNA specifies formal register, preserve it.

One parenthetical aside per post is acceptable and human: "(still figuring that one out)" / "(not an exaggeration)". Use only where Voice DNA supports it.

---

### 3C: Read-Aloud Verification

Before completing Pass 3, check these five questions:

1. Does it sound like a real person talking, or like a press release?
2. Would someone who knows this exec say "yes, that's how they actually talk"?
3. Is there at least one sentence that could only have been written by this specific person, about this specific situation?
4. Does the post end on something concrete, or drift into abstraction?
5. Is there anything a motivated generalist could have written without ever having done this exec's job?

If #3 is no → return to Pass 2, section 2A. Inject a more specific mundane detail.
If #5 is yes → find that sentence. Make it more specific or cut it.

---

## FINAL OUTPUT

```
HUMANIZATION COMPLETE — [Exec Name / Post Topic]

PASS 1 — STRUCTURAL:
Rhetorical theater removed: [list patterns found and fixed, or "None detected"]
Burstiness: [sentence length range before → after]
Transitions removed: [list any removed, or "None detected"]
Em dash count: [before → after]

PASS 2 — COGNITIVE GROUNDING:
Mundane detail: [describe the anchor injected, or "Sufficient specificity already present"]
Asymmetric expertise: [what was cut or tightened, or "No over-explanation found"]
Knowledge boundary: [the boundary installed, quoted, or "None needed — claim already bounded"]
Consensus sharpened: [before → after, or "Central claim was already specific"]

PASS 3 — LEXICAL:
Banned words removed: [list, or "None found"]
Contractions added: [count, or "Register already appropriate"]
Read-aloud check: [flag any remaining concern, or "Sounds like a person"]

────────────────────────────────────

HUMANIZED DRAFT:

[Full post text — post all three passes]

────────────────────────────────────
```

---

## HARD RULES

- **Never fabricate specifics.** If no real detail exists in the source material, flag for the human reviewer. An invented specific the exec doesn't recognize is worse than a vague one.
- **Voice DNA wins.** If the exec's Voice DNA specifies formal register, keep it. If their Voice DNA includes single-word emphasis, allow it. The exec's documented voice is the override authority.
- **Do not restructure the post.** This skill humanizes; it does not rewrite. If a structural problem is so severe humanization can't fix it, flag for the human reviewer.
- **Never skip a pass.** All three run in sequence, every time.
- **Knowledge boundaries are not hedges.** A hedge weakens a claim. A boundary precises it. "Some might argue" is a hedge. "This is true for PLG companies; sales-led is a different game" is a boundary.
