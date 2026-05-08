---
type: doctrine
slug: tribunal-before-training
name: "Tribunal Before Training"
ts_created: 2026-05-08T16:00:00Z
ts_updated: 2026-05-08T16:00:00Z
status: canon
authority: founder
relationships:
  - rel: APPLIES_TO
    target: atlas-quality-stack
  - rel: CITES
    target: atlas27b-qwen36-dev-eval-report
tags: [doctrine, training, tribunal, curator, atlas, base-eval, repair]
---

# 🐝 Tribunal Before Training

**Pull date:** 2026-05-08
**Audience:** anyone proposing or operating a model cook for the Swarm fleet

---

## Doctrine Statement

> **Tribunal begins before training.**

A model is not improved because it was fine-tuned. A model is improved only when the trained model can be shown to outperform its base model against known failure modes, under audit, with a clear delta.

If we cannot point to the specific failure modes the cook repaired and prove them with a base-vs-trained delta — under Curator audit and Tribunal seal — then the cook produced no measurable improvement, regardless of how the loss curve looked.

---

## Why This Exists

New-generation base models are already strong. Qwen3.6-27B walked into our shop with deep CRE asset-class knowledge across multifamily, industrial, retail, self-storage, STNL, and grocery-anchored work — without a single Swarm pair training it. Modern bases bring institutional vocabulary, asset-class taxonomies, financial math, and analytical frameworks already in their weights.

The job of fine-tuning has changed.

Fine-tuning is no longer about teaching the model everything. It is about identifying the specific failure modes a strong base model exhibits in our domain, generating training pairs that repair exactly those failure modes, and proving the cook delivered the repair.

Without a base eval, improvement claims are vibes. With a base eval, improvement claims become measurable deltas.

> **Without a base eval, there is no delta. Without delta, there is no honey.**

---

## The New Training Law

```text
No model enters training without a base-model baseline eval.
No synthetic pair is created without a failure it repairs.
No trained model is accepted without a base-vs-trained delta report.
No output becomes honey without Curator audit and Tribunal seal.
No verdict ships to a customer without senior-hack final review.
```

These rules are non-negotiable. A cook that violates any of them produces output that cannot be sealed into the corpus, regardless of how strong the loss curve or eval scores appear.

---

## The Tribunal Sequence

Every cook follows this sequence. The order matters. Skipping a stage breaks the chain.

```
STAGE 1 · BASE EVAL
   Drop the base model into the dev-eval dashboard.
   Run the canary suite (or domain-realistic prompts) without
   any cooked weights. Capture every output. Senior-hack reads
   each one against institutional rubric.
   
   Output: BASE_BASELINE.jsonl
           Failure-mode taxonomy (what does base get wrong?)
           Strength inventory (what does base already do well?)

STAGE 2 · TARGETED REPAIR DESIGN
   For each failure mode in the base taxonomy, design training
   pairs that REPAIR that specific failure. Each pair carries
   metadata: failure_source, repair_goal, asset_class,
   verification_method.
   
   Output: REPAIR_PAIRS.jsonl
           Every pair must answer: which base failure does
           this fix? How will we verify the fix held?

STAGE 3 · COOK
   Standard Gold Standard recipe (LR 1e-5 · LoRA r=64 alpha=32 ·
   bf16 · 30% epoch fraction · cosine schedule). The recipe is
   constant. The variable is corpus composition.

STAGE 4 · TRAINED EVAL
   Same prompts as STAGE 1 base eval. Cooked model output.
   Senior-hack reads against same rubric. Direct apples-to-apples
   comparison.
   
   Output: TRAINED_RESULTS.jsonl

STAGE 5 · CURATOR AUDIT
   SwarmCurator (parity-class to Atlas) audits the trained
   model's outputs. Checks:
     • Math accuracy
     • Grounding (claims trace to inputs)
     • Unsupported assertions (invented credit ratings, cap
       rates, market data)
     • Missing diligence
     • Recommendation alignment (does the verdict track the
       evidence?)
   
   Output: CURATOR_AUDIT.jsonl
           Per-prompt: Accept / Accept with Revisions / Reject

STAGE 6 · TRIBUNAL SEAL
   Senior hack arbitrates between base output, trained output,
   and Curator audit. Final verdict per pair:
     • HONEY   · trained > base · Curator clean · production fuel
     • JELLY   · partial improvement · HITL gate before training
     • PROPOLIS · trained < base or fabrication · failure receipt
   
   Output: TRIBUNAL_RULING.jsonl
           Sealed with timestamp + senior-hack signature

STAGE 7 · DELTA REPORT
   Aggregate base vs trained vs Curator vs Tribunal across all
   prompts. Quantify the cook's impact:
     • Wins (where trained > base) by category
     • Losses (where trained < base) by category
     • Ties
     • Hallucination compliance delta
     • Math precision delta
     • Verdict consistency delta
   
   Output: DELTA_REPORT.md
           Founder-grade, technical, no marketing fluff
```

A cook is not declared production-ready until STAGE 7 is complete and the delta is positive on the failure modes the cook was designed to repair.

---

## Operational Rules

**Rule 1 · Base eval before any cook.**
Before a single GPU hour is spent training, the base model must run the same prompts the trained model will be evaluated on. If the base model already passes, the cook is unnecessary.

**Rule 2 · Pairs map to failures.**
Every training pair carries explicit metadata linking it to a base-model failure mode. No "general improvement" pairs. No "domain stuffing" pairs. Each pair has a job.

**Rule 3 · Preserve base strengths.**
The cook must not regress capabilities the base already has. If the base catches a Day-1 trap and the cooked model misses it, the cook failed even if loss decreased. Verify this in STAGE 4 against STAGE 1 baseline.

**Rule 4 · Curator audits parity-class.**
A 9B model cannot reliably audit a 27B model's institutional output. Curator must be parity-class (same parameter count) or the audit becomes adversarial-hallucination, not honest QC.

**Rule 5 · Hallucination is a deal-breaker.**
Inventing tenant credit ratings, market cap rate ranges, or institutional benchmarks fails the cook regardless of other improvements. The "do not invent" rule must be respected with measurable compliance delta vs base.

**Rule 6 · Verdict drift is a production blocker.**
A model that produces different verdicts on the same prompt across runs is not production-ready. Customer-facing runs use temperature 0.0 deterministic sampling. Cooks must demonstrate verdict stability.

**Rule 7 · Math precision must be tested.**
Multi-step math (mortgage P&I · DSCR · waterfall returns · IRR) is verified against deterministic answer keys, not narrative quality. If the model loses precision, route the calc to a Python tool — do not trust either base or trained model alone.

**Rule 8 · Senior hack signs every Tribunal seal.**
Until parity Curator ships, the senior hack is the binding signal on whether a pair routes to HONEY or PROPOLIS. No automated routing of borderline pairs.

---

## The Pair-Repair Mapping

Every training pair in the v2+ corpus must carry this metadata structure:

```json
{
  "id": "pair-<uuid>",
  "messages": [...],
  "metadata": {
    "failure_source": "base-model failure mode this pair repairs",
    "repair_goal": "specific behavior the trained model should exhibit",
    "asset_class": "multifamily | industrial | retail | stnl | grocery | etc",
    "verification_method": "how STAGE 4 will confirm the repair held",
    "base_eval_evidence": "specific base-eval prompt(s) where this failure was observed",
    "royal_jelly_tier": "apex | honey | jelly | pollen | propolis",
    "senior_hack_seal": "timestamp + signature when sealed"
  }
}
```

A pair without this metadata cannot enter training. A pair with this metadata but with no corresponding base-eval evidence is generic domain stuffing — reject it.

---

## Honey / Jelly / Propolis · Both Sides of the Pipeline

The Royal Jelly classification applies to both **training inputs** (corpus pairs) and **training outputs** (model responses post-cook).

### Training inputs (corpus pairs)

| Tier | Use | Source |
|---|---|---|
| **APEX** | Hand-curated · self-heal · doctrine proof · multi-turn correction arcs | Senior-hack-sealed sessions where Atlas got it right under pressure |
| **HONEY** | Production training fuel | Verified domain pairs that repair specific base failures |
| **JELLY** | HITL gate · review before training | Promising pairs that need senior-hack confirmation on edge cases |
| **POLLEN** | Raw signal · distill not direct | Prompt-response pairs that need shaping before training use |
| **PROPOLIS** | Failure receipts · retrain priority signal | Sealed examples of what the cook should NOT produce |

### Training outputs (post-cook responses)

| Tier | Curator Decision | Routing |
|---|---|---|
| **APEX** | Accept clean · trained ≥ base · institutional gold | Seal as v2+ training input · feed next cook |
| **HONEY** | Accept clean · trained ≥ base · production-ready | Customer deploy after senior-hack final review |
| **JELLY** | Accept with Revisions · partial improvement | HITL review · either polish or downgrade |
| **POLLEN** | Accept with Revisions · trained ≈ base · no clear delta | Don't seal · investigate why the cook didn't improve |
| **PROPOLIS** | Reject · trained < base or fabrication | Failure receipt · log to retrain-priority dataset |

The flywheel is honest because both sides of the pipeline get classified by the same standard. Bad pairs in produces bad outputs out. The classification on the output side feeds the classification on the input side for the next cook.

---

## Where This Doctrine Came From

This doctrine was forced into existence by the 2026-05-08 Atlas-Qwen-27B v1 dev-eval session. The cook landed clean — final eval_loss 0.4540, token accuracy 86.41%, 7-of-7 PROMOTE on Stage 5 lenient rubric. By every conventional cook-success metric, the model was production-ready.

Then we ran the base.

Across 10 directly-comparable institutional CRE prompts:

- Base **outperformed** the cooked Atlas on 6 prompts (Capital Markets exit math · Waterfall computation · 200-unit MF reconciliation · Memphis 312 Day-1 trap on easier variant · LOI Memphis correctness · LOI Industrial correctness).
- Cooked **outperformed** base on 2 prompts (Price Analysis multi-method synthesis · LOI Reprice institutional embellishments).
- 1 tie.
- 1 untested overlap.

The Stage 5 rubric (1-10 lenient, 7/7 PROMOTE) had been calibrated too generously. The Phase 2 rubric (1-5 strict, 0/9 clean Accept on the same model) was closer to institutional reality. The cook had degraded the base's reconciliation reflex and Day-1 trap detection while adding marginal synthesis polish.

Without the base eval, this regression would have shipped. The cook would have been declared a success by loss curve and lenient rubric. Customer-facing IC memos would have missed the structural traps the base catches natively.

The lesson is now doctrine: **Tribunal begins before training.**

The full receipts are sealed at:

- [`reports/atlas27b_qwen36_dev_eval_report.md`](../reports/atlas27b_qwen36_dev_eval_report.md)
- [`reports/atlas27b_qwen36_eval_summary.md`](../reports/atlas27b_qwen36_eval_summary.md)
- [`reports/atlas27b_eval_metrics.json`](../reports/atlas27b_eval_metrics.json)
- [`reports/atlas27b_eval_metrics.csv`](../reports/atlas27b_eval_metrics.csv)

---

## Coding Agent Boundary

Claude (or any coding agent in the loop) can propose pairs, draft repair targets, generate eval prompts, and write delta reports. Coding agents do not seal. Sealing is a Tribunal act, executed by the senior hack against terminal proof.

> **Claude can propose. Terminal must prove. Tribunal seals.**

If a pair, output, or verdict has not been verified against terminal evidence and signed by the senior hack, it is not honey — it is a candidate. Candidates wait until they are proven or rejected. There is no in-between in the corpus.

---

## Cross-references

- [`doctrine/atlas-quality-stack.md`](./atlas-quality-stack.md) — what's actually in the model · the corpus, the discipline, the receipts
- [`reports/atlas27b_qwen36_dev_eval_report.md`](../reports/atlas27b_qwen36_dev_eval_report.md) — the eval that produced this doctrine

---

**End of doctrine.**
