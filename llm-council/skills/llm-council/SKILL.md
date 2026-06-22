---
name: llm-council
description: Run a question, idea, or decision through 3 to 5 AI advisors who analyze it from forced-divergent angles, peer-review each other anonymously, and a chairman commits to one sharp verdict. For decisions with a real but bounded cost of being wrong. TRIGGERS: "council this", "council?", "worth councilling?", "pressure-test this", "debate this", "should I X or Y", "which option", "I'm torn between". Do NOT trigger on factual lookups, single-option "should I" with no alternative, or creation/processing tasks. A 3-minute council beats a 3-day wrong turn.
---

# LLM Council

Same-model agents from one prompt rhyme. Force them apart: each gets a different forbidden move, evidence demand, and output shape, so they split on what counts as evidence, not just mood. They blind-review; a chairman commits to one verdict.

## 1. Frame
Scan fast (under 30s): CLAUDE.md, memory/, named files. Build a brief, **at most 500 tokens, framed question first, then at most 300 of context**. Overflow compresses the context, never the question. Advisors get the brief, not raw files. If too vague, ask ONE question, then proceed.

Pick advisors from the framed question:
- Always: **Contrarian + First-Principles + Executor** (the tension triangle).
- Add **Expansionist** if it contains: launch, price, positioning, market, build, new, pivot, expand.
- Add **Outsider** if it has 3+ unresolved acronyms or internal nouns.
- Cap 5.

## 2. Convene (parallel)
Spawn all at once. Each gets the brief, its spec, and the others' specs, with one rule: don't duplicate their angle; if you overlap, give your next-best honest answer.
- **Contrarian**: assume a fatal flaw; no balancing phrases; name a failure mode + its mechanism; output a numbered failure list.
- **First-Principles**: ignore the surface question; don't accept its framing; derive from one stated axiom; output a re-framed question + derivation.
- **Executor**: only what's doable Monday; no strategy or theory; concrete steps + time estimates; output a 3-step plan.
- **Expansionist**: hunt the upside others miss; no risk talk; name one adjacent opportunity; output an opportunity map.
- **Outsider**: zero context, fresh eyes; no domain term from the question; surface one curse-of-knowledge gap; output questions only.

## 3. Peer review (parallel)
Anonymize by hash(role + framed_question), label A onward (deterministic, not arrival order). One reviewer per advisor; each sees all and answers:
1. Which response, if acted on, is most likely wrong in 6 months?
2. What did every response take for granted that the question doesn't grant?
3. Bet your own time on one: which, and why?

If you answer "insufficient info," name the missing fact AND why the operator can't supply it in 30s. Else bet on the strongest.

## 4. Chairman
Gets the question, de-anonymized responses, and reviews. Commits to:
- **Shape A** (recommendation) or **Shape B** (hand off to War Room with a re-framed problem statement).
- **Confidence HIGH / MEDIUM / LOW.** LOW + Shape A is forbidden; LOW forces Shape B. MEDIUM is the rarest label: use it only when the single `Assumes:` line is the load-bearing uncertainty, else pick HIGH or LOW.
- **Recommendation: one imperative, 15 words max.** "Use TOML." "Ship now, refactor next sprint." "Hand off to War Room." Can't compress to an imperative, output LOW + Shape B.
- Always `Assumes: <load-bearing assumption>`.
- `Fact-check before acting: <claim>` only if the call rests on a checkable fact (version, API, price, date).
- **Same-model flag:** if reviewers' Q2 answers rhyme, drop confidence one notch and surface the shared assumption in `Assumes:`.
- Before emitting, confirm Shape, Confidence, Assumes (and Fact-check if used) are present. Each section 1 to 2 lines; the whole verdict fits one screen.

## 5. Present in chat
Markdown only, no HTML, no files. Lead with the answer:
```
## Council Verdict: {topic}
**Recommendation:** <imperative>  |  **Confidence:** H/M/L  |  **Shape:** A/B
Assumes: <...>
[Fact-check before acting: <...>]
Agree / Clash / Blind spot caught: one line each
```

## 6. Save (opt-in)
Only on "save it": write `active/council-[timestamp].md`. Else don't.

## Grounding (opt-in)
Search only for version/date/price/API claims. Max 1 per advisor, 2 per council; more triggers handoff.

## Hand off to War Room if ANY holds
Wrong call costs >1 week to recover; irreversible (data migration, public commitment, hiring); reviewers hit insufficient-info, name the missing fact, and it's not 30-second-supplyable; any advisor wants >1 search. Handoff is a success; Shape B carries the re-framed question.

## Calibration
First ~20 runs: did advisors split on substance or only form? If form-only on most, the diversity failed; council this skill's redesign in War Room.
