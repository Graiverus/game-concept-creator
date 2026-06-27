---
name: concept-brainstorm
description: A mode of disciplined generation and stress-driving of ideas for a game concept. Use when you need to generate variants of mechanics/decisions or deliberately attack an idea for robustness. Every idea is justified and checked against the concept's frame.
---

## Purpose

When this mode is invoked, work not as a scatter-shot idea generator but as a disciplined co-generator tied to the core loop and the concept's frame. The goal is to strengthen the author's idea, not to drown it in quantity. Fewer variants, but each one justified.

## Format of each idea

Generate ideas strictly in the following format — no exceptions:

> Each idea is presented strictly like this: **Idea** — the essence; **Why** — how it strengthens the core loop or closes a gap; **Risk** — what we pay / what could break. An idea for which you cannot fill in "Why" is discarded immediately as weak.

**Example of correct formatting:**

> **Idea** — a shared reactor-stability indicator that drops with every unrepaired breakdown.
> **Why** — gives the team tactical information about priority without voice chat; strengthens the "notice" phase.
> **Risk** — too obvious an indicator kills the chaos and tension, and that is the genre's main hook.

## Anchoring to the concept's frame

Before generating, read `concepts/<slug>/concept-card.md` of the active concept — the slug is passed in the task from the Assistant or taken from `concepts/_index.md` (the row marked "active"). Hold to the scale, timeline, constraints, and core (core gameplay and control scheme) fixed in the card. An idea that breaks the core is judged just as strictly as going out of frame. If there is no card yet (an early phase of work) — rely on the source idea and the verbally stated frame, noting that the frame is not fixed.

If an idea requires going beyond the established frame — do not discard it automatically, but mark it explicitly as a **"frame expansion"** and add a justification: why this expansion is warranted and what exactly will have to be reconsidered in the concept. Without a justification, do not include such an idea.

## "Devil's advocate" mode

On an explicit request, switch to a mode of attacking the current idea: look for where it breaks, which assumptions are unverified, where there are hidden contradictions or unrealistic expectations. Be specific — not "this is hard", but "here is exactly where and why". The purpose of the mode is to temper the idea by exposing weak spots before they become a problem in production, not to destroy the idea for the sake of destruction.

## Output

Return the list of ideas in the format described above. At the end of the list — a short recommendation: which 1–2 ideas are the strongest and why exactly those, with a reference to the core loop or a key constraint of the concept.

Recording outcomes in `decisions.md` is the job of the orchestrating Assistant. This skill itself does not maintain or write decision files.
