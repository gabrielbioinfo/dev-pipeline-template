---
name: dev-pipeline
description: Multi-agent development pipeline. Use when implementing features, fixing bugs, or doing code review. Routes each step to the optimal model.
---

# Dev Pipeline Skill

A structured 9-step multi-agent pipeline for shipping features with high confidence and minimal cloud cost.

| Step | Name | Model | Purpose |
|------|------|-------|---------|
| 1 | Planning | Gemini Pro | Deep spec analysis and gap identification |
| 2 | Validation | Claude Opus | Strict spec review and ambiguity flagging |
| 3 | Task Breakdown | Claude Sonnet | Ordered, test-first task list |
| 4 | Write Tests | Qwen3 / Gemini Flash | TDD — tests written before code |
| 5 | Write Code | Qwen3 / Gemini Flash | Minimum implementation to pass tests |
| 6 | Fix Until Green | Qwen3 / Gemini Flash | Iterate until full test suite passes |
| 7 | Code Review | Claude Sonnet | Quality, security, and style review |
| 8 | Integration Tests | Claude Sonnet | End-to-end and contract validation |
| 9 | Spec Validation | Gemini Pro | Confirm all acceptance criteria are met |

Steps 4–6 run on free local inference — zero cloud cost for the high-iteration TDD loop.

**Core rule:** Never skip Step 1. No code without a spec.

---

## Skill Contents

- Full step-by-step prompts and rules: [references/pipeline.md](references/pipeline.md)
- Spec template (output of Step 1): [templates/feature_spec.md](templates/feature_spec.md)
- Task breakdown template (output of Step 3): [templates/task_breakdown.md](templates/task_breakdown.md)
- Token/cost tracker: [templates/budget_tracker.md](templates/budget_tracker.md)

Read `references/pipeline.md` for the full pipeline: all 9 step prompts, model routing rationale, cost table, and running instructions.
