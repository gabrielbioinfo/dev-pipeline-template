# Feature Spec: [Feature Name]

> **Status:** Draft / In Review / Validated  
> **Pipeline Step:** 1 — Planning  
> **Date:** YYYY-MM-DD  
> **Author:**  

---

## Problem Statement

<!-- What user pain or business need does this solve?
Be specific: who is affected, how often, and what breaks or fails today. -->

---

## Proposed Solution

<!-- High-level approach. What will be built?
Avoid implementation detail here — save that for Data Models and API Endpoints.
Include a short rationale for why this approach over alternatives. -->

---

## Data Models

<!-- New schemas or changes to existing schemas.
Use a table or code block for each model. -->

### [Model Name]

| Field | Type | Nullable | Default | Notes |
|-------|------|----------|---------|-------|
| id | uuid | No | gen_random_uuid() | Primary key |
| created_at | timestamptz | No | now() | |

```sql
-- Migration sketch (optional but useful for reviewers)
ALTER TABLE example ADD COLUMN ...;
```

---

## API Endpoints

<!-- One section per endpoint. -->

### `METHOD /path`

**Description:** What this endpoint does.

**Request:**
```json
{
  "field": "type"
}
```

**Response (200):**
```json
{
  "field": "type"
}
```

**Error cases:**
| Status | Condition |
|--------|-----------|
| 400 | Invalid input |
| 404 | Resource not found |
| 409 | Conflict |

---

## Design Decisions

<!-- Key trade-offs made. For each decision, state what was chosen and why.
Include rejected alternatives so future readers understand the reasoning. -->

### Decision 1: [Short label]

**Chosen:** …  
**Rejected alternatives:** …  
**Rationale:** …

---

## Edge Cases

<!-- Boundary conditions, failure modes, and unusual inputs.
For each: what is the input/state, and what is the expected behavior? -->

| Scenario | Expected Behavior |
|----------|------------------|
| Empty list | Return 200 with `[]` |
| Concurrent writes | Last write wins / conflict error |
| Upstream timeout | Return 503, surface error to client |

---

## Dependencies

<!-- External services, feature flags, other teams, or infrastructure this feature requires. -->

- **Feature flags:** `feature_xyz_enabled` must be on
- **External services:** Stripe webhook endpoint must be configured
- **Team dependencies:** Platform team must provision the new queue before deploy
- **Migrations:** Must run before new code is deployed

---

## Acceptance Criteria

<!-- Numbered, testable, and unambiguous. Each criterion should be independently verifiable.
Bad: "The feature works correctly."
Good: "Given a valid request, the endpoint returns 200 with the created resource within 200ms." -->

1. Given …, when …, then ….
2. Given …, when …, then ….
3. Given …, when …, then ….

---

## Validation Notes

<!-- Filled in during Step 2 (Claude Opus validation).
Leave blank until then. -->

**Verdict:** PASS / FAIL  
**Blocking issues:**
- 

**Warnings:**
- 

---

## Out of Scope

<!-- Explicitly list what this feature does NOT address.
This prevents scope creep and sets expectations with stakeholders. -->

- This feature does not handle …
- Internationalization is deferred to a future iteration
- Admin tooling for managing X is out of scope
