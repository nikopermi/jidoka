# jidoka

Plan-first pipeline for Claude Code. Four skills, one contract: research the code, grill the design, outline the phases, review at two gates. Nothing gets built until the plan survives an adversarial read.

Jidoka: stop the line when a defect appears, fix the cause, resume. Applied to agent work, the cheapest place to stop is the plan.

## Install

```
/plugin marketplace add nikopermi/jidoka
/plugin install jidoka@jidoka
```

## Pipeline

```
/research <task>          -> docs/plans/ACTIVE/<date>-<slug>/01-research.md
/grill                    -> 02-design.md   (the one doc a human reviews)
/outline                  -> 03-outline.md  (phases, scope, tests, verify, budget)
/llm-code-review outline  -> SETTLED or NOT SETTLED, before Phase 1
  ... implement one subagent per phase ...
/llm-code-review diff     -> findings by failure class, before merge
```

Each stage writes one file into the task folder and stops. The next stage reads it. The folder path `docs/plans/ACTIVE/<slug>/` is the contract between stages; move finished folders out of `ACTIVE/` however you like.

## Skills

| Skill | Does | Produces |
|---|---|---|
| `research` | Asks the user one to three scoping questions first, then fans out to Explore agents, writes what the code does today with `path:line` evidence. Describes, never proposes. | `01-research.md`, under 150 lines |
| `grill` | Structured interview. Models the design as a decision tree, asks the whole current frontier each round, one problem line and one counter-argument per question. Facts are fetched by subagents; decisions are the user's. | `02-design.md`, decisions with rejected alternatives |
| `outline` | Two to five vertical phases, each with explicit file scope, tests-first, a verify command that exits non-zero on failure, and a line budget. A `## Must handle` section walks state, writers/consumers, inputs, failures, mechanisms, facts. | `03-outline.md` |
| `llm-code-review` | Reviews for the nine ways agent work fails that humans rarely do: unverified premise, self-confirming proof, routing around a constraint, loss hidden by green, fabrication over failure, unnamed case, split ownership, drift, slop. Two modes: `outline <path>` before code, `diff [target]` before merge. Runs in a fresh subagent. | Findings by severity and class; SETTLED / NOT SETTLED |

## Why this shape

- **Questions before search.** Agents skip scoping about half the time when not forced. `research` hard-stops until the user answers.
- **Thin outlines.** A 1000-line plan diverges from the code. A 100-line outline with per-phase scope and verify commands gets built and checked.
- **Review the plan, not just the diff.** One wrong line in an outline becomes many wrong lines of code. The same failure-class list runs at both gates.
- **Fresh reviewer.** The session that wrote the work does not review it.

## Companion

`llm-code-review` complements the built-in `/code-review`; it does not replace it. Run both before merge.

## License

MIT
