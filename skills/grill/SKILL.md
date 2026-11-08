---
name: grill
description: Relentless structured interview to stress-test a plan, design, or decision before work starts. Use when the user says "grill me/this", asks to pressure-test thinking, or before approving a non-trivial plan doc. Maps the design as a decision tree, asks in frontier rounds with a one-line problem and a counter-argument per question, and records settled terms and decisions in the repo's existing docs.
---

Interview the user until shared understanding is reached. The subject is a
plan, design, or decision; the output is a settled design tree plus updated
docs. Do not start implementation until the user confirms the understanding
is shared.

## The design tree

Model the subject as a tree: every decision branches into the decisions that
depend on it. The frontier is every decision whose prerequisites are already
settled — the questions askable now without guessing at unheard answers.

Work in rounds:

1. Ask the entire current frontier in one message. Number each question.
   A question whose answer depends on another question still open this round
   belongs to a later round.
2. For every question, state the problem in one line, then give your
   counter-argument: the strongest case against the plan's current answer,
   or against the obvious answer when the plan has none. Back it with
   evidence (a file and line, a measurement, a vendor doc). Give no
   recommendation line. Format, plain markdown, no emoji:

   **Q1: <title>**
   Problem: <one line: what is unsettled and what it costs>
   Counter-argument: <two or three sentences, with the evidence>

3. Wait for answers. Each answer reshapes the tree: settled decisions push
   the frontier outward. Recompute and ask the next round.
4. Done when the frontier is empty — every branch visited, nothing silently
   assumed. State the settled tree as a short summary and get confirmation.

## Facts vs decisions

Finding facts is never the user's job. When a frontier question needs a fact
from the environment (code, git history, vendor docs), dispatch a subagent to
fetch it and hold only the downstream questions; ask the rest of the frontier
now. Decisions are the user's: put each one to them and wait. Do not settle a
decision yourself just because a round went unanswered.

## Recording (inline, not batched)

As terms and decisions settle, write them down where this repo already keeps
truth — do not introduce a parallel doc system:

- Decisions with real reversal cost, where a future reader would ask "why",
  and where genuine alternatives existed: record in the plan doc under a
  `## Decisions` section (one line: decision, alternatives rejected, why).
  All three conditions or it is not worth recording.
- Terminology that got sharpened (a term renamed, a distinction drawn, a
  word banned for a surface): record where the repo keeps its vocabulary —
  typically CLAUDE.md invariants or an architecture doc, whichever already
  owns the concept. Only create a new CONTEXT.md in repos that have no docs
  system of record.
- Write entries when they settle, in the same turn — not as a batch at the
  end.
- When the subject is a task folder (`docs/plans/ACTIVE/<slug>/01-research.md`
  exists), the settled tree itself goes to `02-design.md` in that folder:
  goal, decisions with rejected alternatives, what is not being done, and
  the phase split as bullets. Cap it near 200 lines; it is the one artifact
  the user reviews, and `/outline` derives the phases from it.

## Voice

Terse, direct, zero emoji. Challenge weak answers — a grill that accepts the
first answer is not a grill. Concede plainly when the user's answer beats the
counter-argument.
