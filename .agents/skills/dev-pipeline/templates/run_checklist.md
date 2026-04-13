# Pipeline Run Checklist

Copy this checklist for each feature run. Check off each item before moving to the next step.

---

## Before You Start

- [ ] `feature_spec.md` contains the raw feature request (rough is fine — Step 1 will refine it)
- [ ] `budget_tracker.md` is reset for this feature
- [ ] No implementation work has started yet

---

## Step 1 — Planning

- [ ] **Window: Gemini Pro** (Alternatives: Claude Opus, GPT-4o)
- [ ] Paste the Step 1 prompt from `references/pipeline.md` with your feature request
- [ ] Save the output to `templates/feature_spec.md`

---

## Step 2 — Validation

- [ ] **Window: Claude Opus** (Alternatives: Gemini Pro, GPT-4o)
- [ ] Paste the Step 2 prompt with the completed `feature_spec.md`
- [ ] Verdict is **PASS** → proceed to Step 3
- [ ] Verdict is **FAIL** → fix spec and return to Step 1
- [ ] Log tokens in `budget_tracker.md`

---

## Step 3 — Task Breakdown

- [ ] **Window: Claude Sonnet** (Alternatives: GPT-4o, Gemini Pro)
- [ ] Paste the Step 3 prompt with the validated spec
- [ ] Save the output to `templates/task_breakdown.md`
- [ ] All tasks are S, M, or L — no XL tasks remain
- [ ] Log tokens in `budget_tracker.md`

---

## Steps 4–6 — TDD Loop (repeat per task)

> **Window: Qwen3 or Gemini Flash (local inference)**
> Alternatives: Llama 3, DeepSeek Coder, Claude Haiku

For each task in `task_breakdown.md`:

- [ ] **Step 4:** Tests written — all failing (red)
- [ ] **Step 5:** Implementation written — tests should be close to passing
- [ ] **Step 6:** Iterate until full test suite is green
- [ ] Move to next task and repeat

---

## Step 7 — Code Review

- [ ] **Window: Claude Sonnet** (Alternatives: GPT-4o, Gemini Pro)
- [ ] Paste the Step 7 prompt with implementation files, test files, and original task
- [ ] No **MUST CHANGE** items remain
- [ ] Log tokens in `budget_tracker.md`

---

## Step 8 — Integration Tests

- [ ] **Window: Claude Sonnet** (Alternatives: GPT-4o, Gemini Pro)
- [ ] Paste the Step 8 prompt with spec and implementation summary
- [ ] All integration tests passing
- [ ] Contract change notice written (if applicable)
- [ ] Log tokens in `budget_tracker.md`

---

## Step 9 — Spec Validation

- [ ] **Window: Gemini Pro** (Alternatives: Claude Opus, GPT-4o)
- [ ] Paste the Step 9 prompt with original spec, implementation summary, and test results
- [ ] Verdict is **SHIP** → feature is done
- [ ] Verdict is **NO-SHIP** → return to the step indicated in the report
- [ ] Log tokens in `budget_tracker.md`

---

## Done

- [ ] All acceptance criteria are PASS in the Step 9 table
- [ ] `budget_tracker.md` has token usage for all cloud steps
- [ ] Code is merged or ready for review
