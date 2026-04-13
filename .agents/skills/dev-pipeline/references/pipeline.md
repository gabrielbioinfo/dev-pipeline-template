# Dev Pipeline Skill

A structured 9-step multi-agent pipeline for shipping features with high confidence and minimal cloud cost. Each step uses the most cost-effective model suited to the task, with free local inference (Qwen3/Gemini Flash) for the highest-volume steps.

---

## Pipeline Overview

| Step | Name | Model | Purpose |
|------|------|-------|---------|
| 1 | Planning | Gemini Pro | Deep spec analysis and gap identification |
| 2 | Validation | Claude Opus | Strict spec review and ambiguity flagging |
| 3 | Task Breakdown | Claude Sonnet | Ordered, test-first task list |
| 4 | Write Tests | Qwen3 / Gemini Flash | TDD — tests written before code |
| 5 | Write Code | Qwen3 / Gemini Flash | Implementation to make tests pass |
| 6 | Fix Until Green | Qwen3 / Gemini Flash | Iterate until full test suite passes |
| 7 | Code Review | Claude Sonnet | Quality, security, and style review |
| 8 | Integration Tests | Claude Sonnet | End-to-end and contract validation |
| 9 | Spec Validation | Gemini Pro | Confirm all acceptance criteria are met |

Steps 4–6 run on free local inference, making the high-iteration loop zero cloud cost.

---

## Cost Tracking

| Step | Model | Est. Input Tokens | Est. Output Tokens | Cached? |
|------|-------|------------------:|-------------------:|---------|
| 1 — Planning | Gemini Pro | ~8,000 | ~2,000 | No |
| 2 — Validation | Claude Opus | ~6,000 | ~1,500 | Yes |
| 3 — Task Breakdown | Claude Sonnet | ~5,000 | ~2,500 | Yes |
| 4 — Write Tests | Qwen3 / Gemini Flash | — | — | Free |
| 5 — Write Code | Qwen3 / Gemini Flash | — | — | Free |
| 6 — Fix Until Green | Qwen3 / Gemini Flash | — | — | Free |
| 7 — Code Review | Claude Sonnet | ~10,000 | ~2,000 | Yes |
| 8 — Integration Tests | Claude Sonnet | ~8,000 | ~3,000 | Yes |
| 9 — Spec Validation | Gemini Pro | ~6,000 | ~1,500 | No |

Use `templates/budget_tracker.md` to record actual usage per run.

---

## Step 1 — Planning

> **MODEL:**
> - **Primary:** Gemini Pro
> - **Alternatives:** Claude Opus, GPT-4o
> Open: AI Studio or gemini.google.com

**Goal:** Produce a complete, unambiguous feature spec from a raw idea or brief.

**Trigger:** New feature request or user story lands in the backlog.

**Prompt template:**

```
You are a senior product engineer. Given the following feature request, produce a structured spec.

Feature request:
<INSERT_REQUEST>

Output a markdown document with these sections:
- Problem Statement: what user pain does this solve?
- Proposed Solution: the approach at a high level
- Data Models: any new or changed schemas
- API Endpoints: routes, methods, request/response shapes
- Design Decisions: key trade-offs and rationale
- Edge Cases: failure modes and boundary conditions
- Dependencies: external services, feature flags, or teams
- Acceptance Criteria: numbered, testable outcomes
- Out of Scope: what this feature deliberately does not address

Be specific. Prefer concrete examples over abstract descriptions.
Flag any assumptions you are making.
```

**Output:** Completed `templates/feature_spec.md`.

---

## Step 2 — Validation

> **MODEL:**
> - **Primary:** Claude Opus
> - **Alternatives:** Gemini Pro, GPT-4o
> Open: claude.ai → new chat → Opus

**Goal:** Catch gaps, contradictions, and untestable criteria before any code is written.

**Trigger:** Feature spec from Step 1 is ready for review.

**Prompt template:**

```
You are a strict technical reviewer. Your job is to find problems in this spec before anyone writes a line of code.

Spec:
<INSERT_SPEC>

Review for:
1. Ambiguous or untestable acceptance criteria — flag each one
2. Missing edge cases — list what's not covered
3. Contradictions within the spec
4. Unstated dependencies or integration risks
5. Scope creep risk — any criteria that imply more work than the spec acknowledges

Output:
- A list of BLOCKING issues that must be resolved before proceeding
- A list of WARNINGS that should be addressed but are not blockers
- A PASS or FAIL verdict

If FAIL, the spec goes back to Step 1. If PASS, proceed to Step 3.
```

**Output:** Validation report. Spec either advances to Step 3 or loops back to Step 1.

---

## Step 3 — Task Breakdown

> **MODEL:**
> - **Primary:** Claude Sonnet
> - **Alternatives:** GPT-4o, Gemini Pro
> Open: Claude Code or claude.ai → Sonnet

**Goal:** Convert the validated spec into an ordered, test-first task list.

**Trigger:** Spec has passed validation.

**Prompt template:**

```
You are a senior engineer breaking down a validated spec into implementation tasks.

Spec:
<INSERT_VALIDATED_SPEC>

Produce an ordered task list using `templates/task_breakdown.md`.

Rules:
- Each task must be independently completable and verifiable
- Tests come before implementation — list what tests to write first for each task
- Order tasks so that dependencies are satisfied before dependents
- Assign complexity: S (< 2h), M (2–4h), L (4–8h), XL (needs splitting)
- Split any XL tasks before finalizing the list
- Include the files likely to be modified for each task

Do not include tasks for documentation or deployment — focus on code and tests only.
```

**Output:** Completed `templates/task_breakdown.md`.

---

## Step 4 — Write Tests

> **MODEL:**
> - **Primary:** Qwen3 or Gemini Flash (local inference)
> - **Alternatives:** Llama 3, DeepSeek Coder, Claude Haiku
> Zero or low cloud cost.

**Goal:** Write the full test suite for the current task before writing any implementation code.

**Trigger:** Task Breakdown is complete. Begin at Task 1.

**Prompt template:**

```
You are writing tests for the following task. Do not write any implementation code yet.

Task:
<INSERT_TASK>

Acceptance criteria:
<INSERT_CRITERIA>

Existing codebase context:
<INSERT_RELEVANT_FILES>

Write:
- Unit tests covering the happy path
- Unit tests covering each edge case listed in the task
- One integration test stub (mark as skipped if infrastructure is not available)

Use the project's existing test framework and conventions.
Tests should fail right now because the implementation does not exist yet.
```

**Output:** New or updated test files. All tests red.

---

## Step 5 — Write Code

> **MODEL:**
> - **Primary:** Qwen3 or Gemini Flash (local inference)
> - **Alternatives:** Llama 3, DeepSeek Coder, Claude Haiku
> Zero or low cloud cost.

**Goal:** Write the minimum implementation needed to make the tests pass.

**Trigger:** Tests for the current task are written and confirmed failing.

**Prompt template:**

```
You are implementing code to make failing tests pass.

Failing tests:
<INSERT_TEST_FILE>

Task description:
<INSERT_TASK>

Existing codebase context:
<INSERT_RELEVANT_FILES>

Rules:
- Write only what is needed to make the tests pass
- Do not add features, logging, or error handling beyond what the tests require
- Follow the project's existing conventions and style
- Do not modify the test files

Output the implementation file(s) only.
```

**Output:** Implementation files. Tests should now be passing (or close to it).

---

## Step 6 — Fix Until Green

> **MODEL:**
> - **Primary:** Qwen3 or Gemini Flash (local inference)
> - **Alternatives:** Llama 3, DeepSeek Coder, Claude Haiku
> Zero or low cloud cost.

**Goal:** Iterate on the implementation until the full test suite is green.

**Trigger:** Step 5 is complete. One or more tests still failing.

**Prompt template:**

```
The following tests are still failing. Fix the implementation.

Failing test output:
<INSERT_TEST_OUTPUT>

Current implementation:
<INSERT_IMPLEMENTATION_FILE>

Rules:
- Do not modify test files
- Make the smallest change that fixes the failure
- Re-run tests after each change and report results
- If a test reveals a real bug in the spec, flag it and stop — do not paper over it

Repeat until all tests pass.
```

**Loop:** Repeat Step 6 until all tests pass. Then move to Step 7.

---

## Step 7 — Code Review

> **MODEL:**
> - **Primary:** Claude Sonnet
> - **Alternatives:** GPT-4o, Gemini Pro
> Open: Claude Code or claude.ai → Sonnet

**Goal:** Review the implementation for correctness, security, maintainability, and style.

**Trigger:** Full test suite is green for the current task.

**Prompt template:**

```
You are doing a thorough code review. The tests pass — now check everything else.

Implementation:
<INSERT_IMPLEMENTATION_FILES>

Tests:
<INSERT_TEST_FILES>

Original task:
<INSERT_TASK>

Review for:
1. Correctness — does the code actually do what it claims?
2. Security — injection, auth, input validation, secrets exposure
3. Performance — obvious bottlenecks or missing indexes
4. Maintainability — clarity, naming, complexity
5. Test coverage — are the tests actually asserting the right things?
6. Style — consistency with the rest of the codebase

Output:
- MUST CHANGE: blocking issues with specific line references
- SHOULD CHANGE: non-blocking improvements
- LGTM items: things done well worth noting

If any MUST CHANGE items exist, fix them before proceeding to Step 8.
```

**Output:** Code review report. Blocking issues are fixed before Step 8.

---

## Step 8 — Integration Tests

> **MODEL:**
> - **Primary:** Claude Sonnet
> - **Alternatives:** GPT-4o, Gemini Pro
> Open: Claude Code or claude.ai → Sonnet

**Goal:** Validate the feature works end-to-end and does not break existing contracts.

**Trigger:** Code review is clean. Implementation is complete for all tasks in the breakdown.

**Prompt template:**

```
You are writing and running integration tests for a completed feature.

Feature spec:
<INSERT_SPEC>

Implementation summary:
<INSERT_IMPLEMENTATION_SUMMARY>

Existing integration test suite:
<INSERT_EXISTING_INTEGRATION_TESTS>

Tasks:
1. Write integration tests that exercise the full request/response cycle for each acceptance criterion
2. Identify any existing integration tests that touch the modified code paths and confirm they still pass
3. Flag any contract changes (API shape, event schema, database migration) that downstream consumers need to know about

Output:
- New integration test file(s)
- List of affected existing tests and their status
- Contract change notice (if any)
```

**Output:** Integration tests passing. Contract change notice if applicable.

---

## Step 9 — Spec Validation

> **MODEL:**
> - **Primary:** Gemini Pro
> - **Alternatives:** Claude Opus, GPT-4o
> Open: AI Studio or gemini.google.com

**Goal:** Confirm that every acceptance criterion in the original spec is demonstrably met.

**Trigger:** Integration tests pass. Feature is considered code-complete.

**Prompt template:**

```
You are doing a final validation pass. Check whether this implementation satisfies the original spec.

Original spec:
<INSERT_ORIGINAL_SPEC>

Implementation:
<INSERT_IMPLEMENTATION_SUMMARY>

Test results:
<INSERT_TEST_RESULTS>

For each acceptance criterion in the spec:
- PASS: criterion is met and can be verified from the test results or implementation
- PARTIAL: criterion is partially met — describe what is missing
- FAIL: criterion is not met — describe the gap

Output a table:
| # | Criterion | Status | Evidence or Gap |
|---|-----------|--------|-----------------|

Then give an overall SHIP or NO-SHIP verdict.
If NO-SHIP, list the criteria that need to be resolved and suggest which pipeline step to return to.
```

**Output:** Spec validation table with SHIP / NO-SHIP verdict.

---

## Running the Pipeline

1. Fill in `templates/feature_spec.md` with your initial feature request (rough is fine — Step 1 will refine it).
2. Run Steps 1–3 sequentially. Each step's output feeds the next.
3. For each task in the breakdown, run Steps 4–6 in a local loop until green.
4. Run Steps 7–9 once all tasks are complete.
5. Record token usage in `templates/budget_tracker.md` after each cloud step.

**Shortcut for small tasks:** If the feature is a single-task change, you may collapse Steps 3–6 into one pass. Use judgment.
