# 🏆🏆 EvalOps — Continuous Integration & Evaluation for LLM Applications

> **"GitHub Actions for LLM reliability."** On every prompt, model, or code change,
> EvalOps runs the full quality battery — capability evals, regression detection,
> red-team probes, and cost/latency checks — **gates the merge**, and then
> **continuously monitors** the deployed app for quality drift.

This is the **capstone**. It doesn't reinvent anything — it *assembles* the earlier
portfolio projects into one coherent platform, which is exactly the point: it proves
you can think at the systems level about LLM reliability, the literal job description
of the emerging **LLM Reliability / AI Evaluation Engineer** roles.

---

## The thesis (put this at the top of the repo)

Software teams don't ship code without CI. In 2026, AI teams still ship prompt and
model changes with a vibe check. EvalOps closes that gap: it brings **CI/CD discipline,
regression gates, and release safety to non-deterministic AI systems** — the discipline
a test-automation engineer already owns, applied to the domain that most needs it.

---

## What it is (system architecture)

```
                        ┌──────────────────────────────────────────────┐
   PR / push  ─────────▶│                 EvalOps CI                     │
   (prompt, model,      │                                                │
    code change)        │  1. CAPABILITY   ← Project 5 eval harness      │
                        │     evals & regression (bootstrap CIs, gate)   │
                        │  2. SAFETY       ← Project 6 red-team suite    │
                        │     garak / deepteam probes (OWASP LLM Top 10) │
                        │  3. COST/LATENCY budgets                       │
                        │            │                                   │
                        │            ▼                                   │
                        │     PASS ──▶ merge + deploy                    │
                        │     FAIL ──▶ block + PR comment w/ diff        │
                        └───────────────────────┬──────────────────────┘
                                                │ deploy
                                                ▼
   ┌─────────────────────────┐        ┌──────────────────────────────┐
   │  App under test          │        │   PRODUCTION MONITORING       │
   │  ← Project 4 RAG /        │◀──────▶│  traces (LangSmith/Phoenix)   │
   │    Project 7 agent        │  live  │  quality-drift detectors:     │
   └─────────────────────────┘ traffic │   • rising refusal rate       │
                                        │   • dropping faithfulness     │
              dashboard ◀───────────────│   • cost creep / latency SLO  │
        (per-commit eval history,       │  → alert → open an eval issue │
         safety posture, live health)   └──────────────────────────────┘
```

## How it reuses the portfolio (build-on-top, not from-scratch)

| EvalOps component | Comes from | Role it showcases |
|---|---|---|
| App under test (RAG) | **Project 4** — Grounded RAG | AI Engineer |
| App under test (agent) | **Project 7** — Guardrailed agent | AI Engineer |
| Capability eval + regression gate | **Project 5** — Eval Bake-Off harness | **Evals** |
| Safety / red-team gate | **Project 6** — Break the Bot | **Red-team** |
| Deploy + drift monitoring loop | **Project 8** — MLOps pipeline | **MLOps** |
| The orchestration + dashboard | **new in this repo** | Systems / platform |

If you've built 4–8, EvalOps is mostly integration + a dashboard — realistic for a
ramping engineer, and the assembly itself is a maturity signal.

---

## Feature scope

**Pre-merge (the gate):**
- Runs the Project-5 capability suite; blocks if a tracked metric regresses beyond threshold (bootstrap-CI aware, so it won't block on noise).
- Runs a fast Project-6 red-team probe set; blocks on any new high-severity finding (a jailbreak that used to be blocked now succeeds = regression).
- Enforces cost-per-call and p95-latency budgets.
- Posts a **PR comment** with the before/after score diff + safety delta.

**Post-deploy (the monitor):**
- Traces production calls (Arize Phoenix / LangSmith).
- Quality-drift detectors: refusal-rate spike, faithfulness drop (sampled online evals), cost creep.
- On drift → alert + auto-open a tracked "eval issue" with example transcripts.

**Dashboard:**
- Per-commit eval history (is quality trending up or down over time?).
- Current safety posture (open findings by OWASP category).
- Live production health (SLOs, drift status).

## Suggested stack
- **Orchestration:** GitHub App / GitHub Actions backbone (reuse the Project-5 workflow).
- **Capability evals:** the Project-5 harness (promptfoo / DeepEval under the hood).
- **Safety:** garak + deepteam / promptfoo red-team (Project 6).
- **Tracing & online evals:** Arize Phoenix (OSS, self-hostable) or LangSmith.
- **Drift:** Evidently or custom detectors.
- **Dashboard:** Next.js or Streamlit; deploy on Fly.io / Render / a cloud VM for a live URL.

## Milestones
1. **M1 — Gate MVP:** wire Project-5 capability evals into CI as a blocking gate on a real app (Project 4). Ship the PR-comment diff.
2. **M2 — Safety gate:** add the Project-6 red-team probes as a second gate; track findings by OWASP category.
3. **M3 — Monitoring:** trace the deployed app; add one drift detector (refusal rate or faithfulness) that opens an issue.
4. **M4 — Dashboard + demo:** per-commit history + live health page; deploy it; record a 2-minute demo video; write the capstone blog post.

## Presentation checklist
- [ ] Architecture diagram at the top (the one above, cleaned up in Excalidraw).
- [ ] A live dashboard URL + a 2-minute demo video.
- [ ] A headline story: *"caught a prompt change that regressed faithfulness 0.94 → 0.88 and re-enabled a jailbreak — blocked before merge."*
- [ ] The capstone write-up tying evals + safety + ops into one thesis about LLM reliability.

---

_The flagship. Everything else in the portfolio is a component of this. Build it last,
present it first._
