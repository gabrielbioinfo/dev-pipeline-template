# dev-pipeline-template

A multi-agent development pipeline that routes each step of the feature lifecycle to the model best suited for the task — not the most expensive one available.

---

## The Problem

Using a single frontier model for everything — planning, coding, review, validation — is both wasteful and suboptimal. Spec analysis and final validation need broad reasoning. Code review needs strong code understanding. The TDD inner loop runs hundreds of small iterations and needs to be free.

This template wires the right model to the right step, keeping the high-iteration loop on local inference and the cloud steps surgical.

---

## The Pipeline

| Step | Name | Model | Purpose |
|------|------|-------|---------|
| 1 | Planning | Gemini Pro | Deep spec analysis and gap identification |
| 2 | Validation | Claude Opus | Strict spec review and ambiguity flagging |
| 3 | Task Breakdown | Claude Sonnet | Ordered, test-first task list |
| 4 | Write Tests | Qwen3 / Gemini Flash | TDD — tests written before code |
| 5 | Write Code | Qwen3 / Gemini Flash | Minimum implementation to pass tests |
| 6 | Fix Until Green | Qwen3 / Gemini Flash | Iterate until full suite passes |
| 7 | Code Review | Claude Sonnet | Correctness, security, and style |
| 8 | Integration Tests | Claude Sonnet | End-to-end and contract validation |
| 9 | Spec Validation | Gemini Pro | Confirm all acceptance criteria are met |

Steps 4–6 run on free local inference. Zero cloud cost for the high-iteration TDD loop.

---

## Model Routing

| Task | Model | Why |
|------|-------|-----|
| Spec generation and final sign-off | Gemini Pro | Broad product reasoning; strong at structured generation and gap detection |
| Adversarial spec review | Claude Opus | Most rigorous Claude model; justified cost for a blocking gate that prevents rework |
| Task planning, code review, integration tests | Claude Sonnet | Strong coding ability at a fraction of Opus cost; ideal for structured, code-aware tasks |
| TDD inner loop (write tests → write code → fix) | Qwen3 / Gemini Flash | Free local inference; cost matters here because this loop runs many times per feature |

---

## Repository Structure

```
dev-pipeline-template/
├── .agents/
│   └── skills/
│       └── dev-pipeline/
│           ├── SKILL.md                       # Skill entry point — frontmatter + overview
│           ├── references/
│           │   └── pipeline.md                # Full pipeline: all 9 step prompts, rules, cost table
│           └── templates/
│               ├── feature_spec.md            # Spec template — output of Step 1
│               ├── task_breakdown.md          # Task breakdown template — output of Step 3
│               └── budget_tracker.md          # Token/cost tracker — fill after each cloud step
├── AGENTS.md                                  # Auto-loaded by OpenCode
├── CLAUDE.md                                  # Auto-loaded by Claude Code
└── LICENSE                                    # MIT
```

---

## Quick Start

**1. Use this template**

Click **Use this template** on GitHub, or clone directly:

```bash
git clone https://github.com/studiocraft-ai/dev-pipeline-template.git
cd dev-pipeline-template
```

**2. Add the pipeline skill to your IDE**

The skill is already in `.agents/skills/dev-pipeline/` and will be picked up automatically by Claude Code, OpenCode, Codex CLI, and Antigravity.

For Cursor or Windsurf, add `.agents/skills/dev-pipeline/references/pipeline.md` to your project context or rules file.

**3. Configure API keys**

You need at minimum:
- **Gemini Pro** — [Google AI Studio](https://aistudio.google.com/) (Steps 1 and 9)
- **Claude Opus + Sonnet** — [Anthropic Console](https://console.anthropic.com/) (Steps 2, 3, 7, 8)
- **Qwen3 or Gemini Flash locally** — [Ollama](https://ollama.com/) or [LM Studio](https://lmstudio.ai/) (Steps 4–6)

**4. Start a feature**

Copy `.agents/skills/dev-pipeline/templates/feature_spec.md` to your project, drop in a rough feature request, and run Step 1.

---

## Cost Estimate

Typical cost for a medium-complexity feature (single developer, one pipeline run):

| Step | Model | Est. Tokens | Est. Cost |
|------|-------|-------------|-----------|
| 1 — Planning | Gemini Pro | 10k in / 2k out | ~$0.02 |
| 2 — Validation | Claude Opus | 6k in / 1.5k out (cached) | ~$0.16 |
| 3 — Task Breakdown | Claude Sonnet | 5k in / 2.5k out (cached) | ~$0.05 |
| 4–6 — TDD Loop | Qwen3 / Gemini Flash | — | $0.00 |
| 7 — Code Review | Claude Sonnet | 10k in / 2k out (cached) | ~$0.06 |
| 8 — Integration Tests | Claude Sonnet | 8k in / 3k out (cached) | ~$0.06 |
| 9 — Spec Validation | Gemini Pro | 6k in / 1.5k out | ~$0.03 |
| **Total** | | | **~$0.38** |

Track actual usage per run in `.agents/skills/dev-pipeline/templates/budget_tracker.md`.

---

## Example Project

**minimal-ai-fastapi-pipeline** — a minimal FastAPI service built end-to-end using this pipeline, with all spec, task breakdown, and budget tracker files included. *(coming soon)*

---

## Philosophy

> The best model for the job is not always the most expensive one.

Frontier models are exceptional at reasoning under ambiguity and catching subtle bugs. Local models are fast, free, and perfectly capable of TDD iteration. The pipeline separates these concerns deliberately.

---

## License

MIT — see [LICENSE](LICENSE).  
Built by [Bioinfo Studio](https://studiocraft.ai) / [studiocraft.ai](https://studiocraft.ai).
