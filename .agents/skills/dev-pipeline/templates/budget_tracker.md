# Budget Tracker: [Feature Name]

> **Pipeline run date:** YYYY-MM-DD  
> **Spec link:** [feature_spec.md](feature_spec.md)  

---

## Token Usage by Pipeline Step

> Record actual token counts from API responses.  
> Steps 4–6 run on free local inference — no token cost.

| Step | Model | Input Tokens | Cached Tokens | Output Tokens | Input Cost | Output Cost | Total Cost |
|------|-------|-------------:|--------------:|--------------:|-----------:|------------:|-----------:|
| 1 — Planning | Gemini Pro | | — | | | | |
| 2 — Validation | Claude Opus | | | | | | |
| 3 — Task Breakdown | Claude Sonnet | | | | | | |
| 4 — Write Tests | Qwen3 / Gemini Flash | — | — | — | $0.00 | $0.00 | $0.00 |
| 5 — Write Code | Qwen3 / Gemini Flash | — | — | — | $0.00 | $0.00 | $0.00 |
| 6 — Fix Until Green | Qwen3 / Gemini Flash | — | — | — | $0.00 | $0.00 | $0.00 |
| 7 — Code Review | Claude Sonnet | | | | | | |
| 8 — Integration Tests | Claude Sonnet | | | | | | |
| 9 — Spec Validation | Gemini Pro | | — | | | | |
| **Total** | | | | | | | **$0.00** |

**Cache savings:** $X.XX (cached tokens billed at ~10% of input rate)

---

## Time Tracking

| Step | Start | End | Wall Time | Notes |
|------|-------|-----|-----------|-------|
| 1 — Planning | HH:MM | HH:MM | Xm | |
| 2 — Validation | HH:MM | HH:MM | Xm | |
| 3 — Task Breakdown | HH:MM | HH:MM | Xm | |
| 4–6 — TDD Loop (Task 1) | HH:MM | HH:MM | Xm | N iterations |
| 4–6 — TDD Loop (Task 2) | HH:MM | HH:MM | Xm | N iterations |
| 4–6 — TDD Loop (Task 3) | HH:MM | HH:MM | Xm | N iterations |
| 7 — Code Review | HH:MM | HH:MM | Xm | |
| 8 — Integration Tests | HH:MM | HH:MM | Xm | |
| 9 — Spec Validation | HH:MM | HH:MM | Xm | |
| **Total** | | | **Xh Ym** | |

---

## Client Pricing Reference

Use this formula to calculate what to charge a client for this pipeline run.

```
billable_cost = total_api_cost × markup
```

| Tier | Markup | When to use |
|------|-------:|-------------|
| Internal / tooling | 1.0× | No markup — internal productivity |
| Standard project | 2.0× | Normal client work |
| Urgent / expedited | 3.0× | Same-day turnaround or after-hours |
| Retainer | 1.5× | Pre-negotiated monthly volume |

**This run:**

| Item | Value |
|------|-------|
| Raw API cost | $0.00 |
| Markup tier | Standard (2.0×) |
| Billable amount | $0.00 |
| Wall time (hours) | 0.0h |
| Effective hourly rate | $0.00/h |

---

## Notes

<!-- Anything unusual about this run: retries, model fallbacks, spec loops, etc. -->
