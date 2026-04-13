# Dev Pipeline Template

This repository is a **multi-agent development pipeline template** — a structured, 9-step process for shipping features with high confidence and minimal cloud cost. Each step routes work to the model best suited for the task, and the high-iteration inner loop (Steps 4–6) runs entirely on free local inference.

---

## Pipeline Overview

| Step | Name | Model | Purpose |
|------|------|-------|---------|
| 1 | Planning | Gemini Pro | Deep spec analysis and gap identification |
| 2 | Validation | Claude Opus | Strict spec review and ambiguity flagging |
| 3 | Task Breakdown | Claude Sonnet | Ordered, test-first task list |
| 4 | Write Tests | Qwen3 / Gemini Flash | TDD — tests before code |
| 5 | Write Code | Qwen3 / Gemini Flash | Minimum implementation to pass tests |
| 6 | Fix Until Green | Qwen3 / Gemini Flash | Iterate until full suite passes |
| 7 | Code Review | Claude Sonnet | Quality, security, and style |
| 8 | Integration Tests | Claude Sonnet | End-to-end and contract validation |
| 9 | Spec Validation | Gemini Pro | Confirm all acceptance criteria are met |

Steps 4–6 run on free local inference — zero cloud cost for the high-iteration loop.

---

## Repository Layout

| Path | Purpose |
|------|---------|
| [.agents/skills/dev-pipeline/SKILL.md](.agents/skills/dev-pipeline/SKILL.md) | Skill entry point — frontmatter + overview |
| [.agents/skills/dev-pipeline/references/pipeline.md](.agents/skills/dev-pipeline/references/pipeline.md) | Full pipeline: all 9 step prompts, rules, and cost table |
| [.agents/skills/dev-pipeline/templates/feature_spec.md](.agents/skills/dev-pipeline/templates/feature_spec.md) | Spec template (output of Step 1) |
| [.agents/skills/dev-pipeline/templates/task_breakdown.md](.agents/skills/dev-pipeline/templates/task_breakdown.md) | Task breakdown template (output of Step 3) |
| [.agents/skills/dev-pipeline/templates/budget_tracker.md](.agents/skills/dev-pipeline/templates/budget_tracker.md) | Token/cost tracker — fill in after each cloud step |

---

## Model Routing Logic

**Gemini Pro** (Steps 1 and 9) — used for open-ended generation and final verification. Step 1 needs a model that can reason broadly about product requirements and surface gaps. Step 9 needs the same breadth to confirm the implementation matches the original intent.

**Claude Opus** (Step 2) — used for strict, adversarial review. Opus is the most rigorous Claude model and is well-suited for blocking work that should not proceed. This is the gatekeeper step; cost is justified because it prevents expensive mistakes downstream.

**Claude Sonnet** (Steps 3, 7, 8) — used for structured, code-aware reasoning. Sonnet handles task decomposition, code review, and integration testing where strong coding ability matters but Opus-level cost is unnecessary.

**Qwen3 / Gemini Flash** (Steps 4–6) — used for the TDD inner loop. These steps are the highest-volume and most iterative. Running them on free local inference eliminates cloud cost entirely for the part of the pipeline that repeats most.

---

## Core Rule: No Code Without a Spec

**Never skip Step 1.** No implementation work begins until a feature spec exists and has passed Step 2 validation. This is the single most important constraint in the pipeline.

- A rough idea is not a spec.
- "We'll figure it out as we go" is not a spec.
- Step 1 exists to surface ambiguity before code is written, not after.

If a spec is missing, go back to Step 1 before doing anything else.

---

## Instructions for the AI

When a user asks for help building or modifying a feature in any project that uses this pipeline:

1. **Ask whether a spec exists.** If the user does not have a completed `feature_spec.md`, do not write code. Offer to run Step 1 first using the prompt in [.agents/skills/dev-pipeline/references/pipeline.md](.agents/skills/dev-pipeline/references/pipeline.md).

2. **If a spec exists, ask if it has passed Step 2 validation.** If not, run the Step 2 validation prompt before proceeding.

3. **Use the correct model for each step.** Do not use Claude for Steps 1, 4, 5, 6, or 9. If asked to run those steps, remind the user of the intended model and redirect accordingly.

4. **Follow the step order.** Do not skip steps or run them out of sequence. Each step's output is a required input to the next.

5. **Track cost.** Remind the user to record token usage in `.agents/skills/dev-pipeline/templates/budget_tracker.md` after each cloud step (1, 2, 3, 7, 8, 9).

6. **Enforce the no-code-without-spec rule.** If a user asks you to "just write the code quickly," decline and offer to run Step 1 instead. A 10-minute spec saves hours of rework.
