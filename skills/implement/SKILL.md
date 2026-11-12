---
name: implement
description: Implement a feature end to end. Use when asked to implement or build a feature or change.
---

Carry the work to the end: every requirement in place, and the tests that cover it run.

## Mode and style

Mode decides who does the work. Style guides how the solution is built. Settle both before the work starts; any mode can use either style.

The user's message may name either or both. Ask the user for any missing selection as your first act.

### Mode

1. `simple`. You implement the work in this session.
2. `auto-audit`. You implement it, then a fresh subagent audits it against the requirements, and you close the loop on what it finds.
3. `orchestrator`. You manage the work, and every piece of it goes to a subagent.

Recommend one from the work's size, how many independent areas it touches, and how breaking it is, and append `(recommended)` to that option.

Then load the mode's files and follow it on top of this one.

| mode         | load                                                           |
| ------------ | -------------------------------------------------------------- |
| simple       | this file alone                                                |
| auto-audit   | `modes/auto-audit.md`, `modes/audit.md`                        |
| orchestrator | `modes/orchestrator.md`, `modes/dispatch.md`, `modes/audit.md` |

### Style

Offer `lazy` and `engineer`, recommending one. Load [styles/lazy.md](styles/lazy.md) or [styles/engineer.md](styles/engineer.md) accordingly.

Apply the style throughout the work and pass its file path to workers and auditors.

## The work

The repository's own rules win wherever they disagree with this file.

Work through the requirements, and each one is done when its completion criterion holds. Code that exists is not a finished requirement.

The requirements can be wrong about the existing code, since they were settled in conversation rather than read from the tree. Correct the record as you go, and say what changed. A departure from what the user asked for is a different thing, and it gets reported rather than made quietly, so the reader can see the choice that was made.

Never assume or guess anything, ask the user. A decision that belongs to the user goes through the `interview` skill, including a question that appears mid-implementation. A blocker gets raised before it gets worked around.
