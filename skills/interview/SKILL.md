---
name: interview
description: Interview the user about a plan, decision, or idea. Use when the user uses any 'interview' trigger phrases or when you ask them a question.
---

Interview the user until the decisions are settled. Map its decisions as a **design tree**: every decision branches into the decisions that hang off it. Don't start implementation until the tree is settled, you reach a shared understanding, and the user green-lights it.

## Mode

Modes nest, shallow to deep: `results` (outcome), `architecture` (shape), `interface` (contracts), `implementation` (code). Settle the mode before the first question; if the user does not name one, ask in an `ask_user_question` call headed `select mode` with one option per mode, recommending one.

A question deeper than the mode is not asked. Decide it and note the call in the summary.

**Calibrate the interview to the task.** Being asked to interview is not a mandate for a fixed number of rounds. A complex, lengthy task may need several rounds working the whole tree. A simple task may need one round of two or three questions, or none, if every open point has an obvious answer. How many rounds you run is set by how many questions worth asking are left, not by the ritual.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

**Filter every question before you ask it.** The mode already removed everything deeper than its level. What remains earns a place only if the user's answer would actually change what you do. Drop any question that fails this test:

- **Obvious answer.** If one option is clearly better, because the user's request, existing conventions, or common sense already settle it, don't ask. The same goes for options that are effectively equivalent.
- **Validation.** Never ask the user to confirm something you found or verified yourself. Ask only when findings are ambiguous or conflict.
- **Fact-finding.** Facts are yours to dig up, not the user's. A question you could answer by looking at the code, filesystem, or tools is a waste of the user's time. Dispatch a sub-agent instead.

What remains after the filter are the _decisions_ that belong to the user: taste, tradeoffs, intent, constraints only they know. Don't undervalue the high-level decisions here: scope, depth, architecture, and organization belong in the tree as much as implementation choices.

Ask every round through the `ask_user_question` tool: one call whose `questions` array holds the whole frontier for this round, one entry per frontier question. Any context the user needs goes in short paragraphs (one per question) before the tool call, so each entry carries only its question:

- `id`: a stable slug for the question.
- `question`: the question, short and concise.
- `header`: a short title for the question.
- `options`: the choices, each with a `label` and a one-sentence `description`. Put your recommended answer first.
- `multi_select`: `true` when the answer can include more than one option.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier, reapply the filter, and ask the next round with another `ask_user_question` call. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Screen every round for hidden dependencies before you send it. A question whose validity, options, or relevance changes based on how another question in the same round is answered does not belong in this round. Defer it to the next round; ask it only once its prerequisite is settled.

When a frontier question needs a fact from the environment, dispatch a sub-agent to find it and don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. Some things can only be found by the user; accept that and ask them to look.

**Green light.** When the frontier is empty, summarize what will be built, including any calls you made deeper than the mode, and ask whether to `Implement` or `Stop here`. `Implement`: Load and follow the `implement` skill.

A question that surfaces during implementation reopens the frontier. A blocker the plan didn't anticipate, a case the settled decisions don't cover: it enters the frontier like any other question and resumes the interview. It closes the same way it closed before: empty frontier, then a fresh green light.
