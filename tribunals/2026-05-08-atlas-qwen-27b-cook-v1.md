---
type: tribunal
slug: cook-v1-atlas-qwen-27b
name: "Atlas 27B Qwen 3.6 Cook v1 Tribunal Verdict"
ts_created: 2026-05-08T16:30:00Z
ts_updated: 2026-05-08T16:30:00Z
status: sealed
authority: senior-hack
cook_model: atlas-qwen-27b-v1
base_model: qwen3.6-27b
verdict: jelly-propolis
production_classification: internal-analyst-assist-only
relationships:
  - rel: APPLIES_TO
    target: atlas-qwen-27b-v1
  - rel: CITES
    target: tribunal-before-training
  - rel: CITES
    target: atlas27b-qwen36-dev-eval-report
tags: [tribunal, verdict, cook-v1, atlas, qwen, sealed]
---

# 🐝 Atlas 27B Qwen 3.6 Cook v1 · Tribunal Verdict

**Sealed:** 2026-05-08
**Sealed by:** Senior Hack (Donovan Mackey · Founder)
**Doctrine cited:** [Tribunal Before Training](../doctrine/tribunal-before-training.md)
**Receipts:** [`reports/atlas27b_qwen36_dev_eval_report.md`](../reports/atlas27b_qwen36_dev_eval_report.md)

---

## Verdict

> **Do not promote to IC Draft Ready.**
> **Classify as Jelly/Propolis.**
> **Use Cook v1 failures to generate Block-2 repair corpus.**
> **Deploy Base + v4 prompt + deterministic validator for internal analyst assist only.**

This is the formal Tribunal seal on Atlas-Qwen-27B v1. The cook does not pass to production. The cook does not pass to IC Draft Ready. The cook outputs route to the Royal Jelly tier system per the failure pattern observed.

---

## What Was Cooked

| Field | Value |
|---|---|
| Cook ID | atlas-qwen-27b-v1 |
| Base model | Qwen3.6-27B (`/data2/qwen-3.6-27b/`) |
| Corpus | Block-1-v4 · 244,725 pairs · 12 curated sources · cap-rate-controlled mixing |
| Recipe | Gold Standard · LoRA r=64 alpha=32 · LR 1e-5 · bf16 · 30% epoch fraction · cosine schedule |
| Hardware | RTX PRO 6000 Blackwell 96GB · swarmrails GPU 0 |
| Wall clock | 29h 28min |
| Final eval_loss | 0.4540 |
| Final token accuracy | 86.41% |
| Adapter | `/data1/atlas-qwen-27b-v4/lora-adapter/checkpoint-2294` |
| Merged weights | `/data1/atlas-qwen-27b-v4/merged/` (12 shards · 51 GB · text-only post-merge) |
| Cook completed | 2026-05-08T07:19 UTC |

By every conventional cook-success metric the cook landed clean. Stage 5 lenient rubric (1-10 scale) graded the model 7-of-7 PROMOTE with means 8.71 / 9.43 / 9.0 / 8.71 across doctrine / numeric / framing / judgment.

The Tribunal verdict is based on what came after.

---

## Why The Cook Failed Tribunal

### Stage 4 · Trained Eval

Phase 2 v3 strict rubric (1-5 scale) graded the same model **0-of-9 clean Accept**. Means dropped to 3.57 / 2.71 / 2.29 / 3.29 / 2.57 across Accuracy / Grounding / Underwriting Discipline / Risk Awareness / IC Readiness. Same model · stricter calibration · opposite read.

### Stage 5 · Curator Audit

SwarmCurator-9B reviewed 7 audits in detail. **1 of 7 audits cleanly correct.** Failure modes included fabricated math errors that did not exist, prompt-fact-as-unsupported confusion, missed real Atlas errors, and recommendations that contradicted explicit prompt specifications. Curator-9B is structurally undersized to audit 27B institutional output.

### Stage 6 · Tribunal · The Decisive Comparison

The base model (Qwen3.6-27B) was loaded into the same dashboard and run on the same prompts. Direct comparison on 10 overlapping institutional prompts:

| Result | Count |
|---|---:|
| BASE outperformed cooked | 6 |
| COOKED outperformed base | 2 |
| Tie | 1 |
| Untested overlap | 1 |

The cook delivered a NET REGRESSION on 6 of 10 prompts versus its own base. Specifically:

- **Capital Markets 248-MF:** cooked subtracted hypothetical NEW refi loan from exit value (institutionally wrong); base subtracted EXISTING debt balance (correct exit math)
- **Waterfall 180-unit:** cooked listed "metrics to recalculate"; base ran the actual waterfall and surfaced the LP pref shortfall in year 1
- **200-unit MF screen:** cooked accepted seller pro forma NOI at face value; base reconstructed NOI from line items and surfaced a $1.14M hole flagged as "FATAL UNDERWRITING ERROR"
- **Memphis 312 easier variant:** cooked deferred to NMD; base caught the going-in 0.97x interest coverage and reached KILL with high confidence
- **LOI Memphis:** cooked listed Section 19 (Conditions to Closing) as binding (legally wrong) and omitted Governing Law section entirely; base produced correct binding-section list and included Governing Law
- **LOI Industrial:** cooked introduced cross-reference bug ("Section 1 (Confidentiality)" when Section 1 was Property Description) and ROFR overreach beyond exclusivity; base avoided both errors

Cooked won only on synthesis-discipline prompts (Price Analysis multi-method conservative pricing range; LOI Reprice institutional embellishments including MAC clause and mediation).

The cook traded reconciliation reflex, multi-step debt math, and verdict commitment for marginal synthesis polish on judgment-driven prompts. **For Donovan's primary use case — institutional CRE underwriting — the cook is a net regression.**

---

## Royal Jelly Tier Routing

Per the [Tribunal Before Training doctrine](../doctrine/tribunal-before-training.md), the cook outputs are classified by where they sat against base in the Tribunal comparison:

| Cook v1 outputs | Tier | Routing |
|---|---|---|
| Price Analysis multi-method · LOI Reprice institutional embellishments | **HONEY** | Production training fuel for v2+ corpus on synthesis-discipline pairs |
| Memphis 312 harder (tied with base on Day-1 trap) · clean partial wins | **JELLY** | HITL gate · senior-hack review before sealing |
| Outputs where cooked merely matched base · no clear delta | **POLLEN** | Raw signal · don't seal as training fuel |
| Outputs where cooked underperformed base (the 6 prompts above) | **PROPOLIS** | Failure receipts · retrain priority signal · feed Block-2 repair-pair generator |

The cook v1 corpus body is mostly POLLEN/PROPOLIS for v2 purposes. The HONEY pairs are the small minority where cooked produced output base could not.

---

## Repair Block Generation

The 6 PROPOLIS prompts identify 6 specific repair blocks for v2 corpus generation. See the machine-readable failure taxonomy at [`cook-v1-failure-taxonomy.json`](./2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json).

Block-2 corpus must include explicit repair pairs for each critical failure surfaced in Cook v1:

```text
reconciliation_pairs            ~15K · catch line-item-vs-stated-NOI contradictions
bridge_refi_pairs               ~3K · Day-1 interest coverage + amortizing DSCR + refi math
verdict_trigger_pairs           ~3K · evidence → verdict mapping discipline · no soft NMD
stnl_anti_hallucination_pairs   ~5K · refuse to invent specific credit ratings
mortgage_constant_pairs         ~3K · multi-step mortgage P&I precision
loi_binding_section_pairs       ~5K · binding-section list discipline + governing law
```

Total v2 specialized pairs: ~34K. Combined with capped Block-1-v4 narrative pairs (≤30% of total = ~73K max), Block-2 target corpus size ~107K pairs (less than half of Cook v1's 244K · because the cook proved that volume of generic narrative is not the bottleneck).

---

## Production Deployment Decision

### Trained Atlas-Qwen-27B v1 · Internal Analyst Assist ONLY

What it can do:
- Draft IC memo skeletons in institutional voice
- Run cap rate / price-per-unit / TPC calculations
- Identify standard CRE risk categories
- Apply 5-verdict enum (Approve / Conditions / NMD / Reprice / Kill)

What it must NOT do:
- Customer-facing IC memos without senior-hack final review
- Day-1 debt feasibility analysis (cooked v1 missed Memphis trap)
- STNL credit assessment (hallucinates specific ratings)
- Standalone deployment without parity-class Curator validator
- Production LOI drafting (variance issue · sometimes clean · sometimes errored)

### Recommended Production Path · Base + v4 Prompt + Deterministic Validator

Deploy stack:

```
ATLAS LAYER       Base Qwen3.6-27B (no cook)
                  + v4 system prompt (mandatory Known/Unknown preamble)
                  + temperature 0.0 deterministic sampling for customer-facing
                  + verification framing on STNL prompts to prevent credit hallucination

VALIDATOR LAYER   Base Qwen3.6-27B with Curator institutional QC system prompt
                  (until parity Curator-27B ships)
                  + audit Atlas output against math, grounding, unsupported claims,
                    missing diligence, recommendation alignment

MATH LAYER        Deterministic Python tool for verdict-driving math
                  (mortgage P&I · DSCR · waterfall returns · IRR)
                  Both base and cooked exhibit precision wobble · do not trust
                  either model alone for production output

HUMAN LAYER       Senior hack final review on every customer-facing output
                  No automated routing of borderline pairs
                  Tribunal seal required before any honey classification
```

This stack is shippable today without any further cook investment. The cook v1 should NOT be deployed in production · its outputs feed v2 training, not v2 production.

---

## Tribunal Sequence Compliance

Per [`doctrine/tribunal-before-training.md`](../doctrine/tribunal-before-training.md):

| Stage | Status | Artifact |
|---|---|---|
| 1. Base Eval | ✓ | Base ran 40 prompts · `/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T112840.jsonl` |
| 2. Targeted Repair Design | ✗ NOT DONE | Cook v1 corpus was generic Block-1-v4 narrative · not failure-targeted |
| 3. Cook | ✓ | 29h 28min · loss 0.4540 · 86.41% token acc |
| 4. Trained Eval | ✓ | Phase 2 v3 rubric · 9 paired · 0-of-9 clean Accept |
| 5. Curator Audit | ✓ (with caveat) | Curator-9B audited · 1-of-7 cleanly correct · structurally undersized |
| 6. Tribunal Seal | ✓ THIS DOC | Senior-hack ruling · sealed 2026-05-08 |
| 7. Delta Report | ✓ | [`reports/atlas27b_qwen36_dev_eval_report.md`](../reports/atlas27b_qwen36_dev_eval_report.md) |

Stage 2 was skipped. The cook v1 went straight from corpus to GPU without a base-eval-driven repair design. This is the procedural failure the [Tribunal Before Training doctrine](../doctrine/tribunal-before-training.md) was sealed to prevent. **Cook v2 will not enter Stage 3 until Stage 1 + Stage 2 are complete with senior-hack sign-off.**

---

## Sealed Verdict (Final)

```text
COOK:           Atlas-Qwen-27B v1
DATE SEALED:    2026-05-08
DOCTRINE:       Tribunal Before Training
VERDICT:        Do NOT promote to IC Draft Ready
CLASSIFICATION: Jelly/Propolis (mostly · with isolated HONEY pairs)
DEPLOYMENT:     Internal Analyst Assist ONLY · with senior-hack final review
NEXT STEP:      Generate Block-2 repair corpus from Cook v1 PROPOLIS pairs
                Re-cook only after Stage 1 + Stage 2 of Tribunal sequence complete
SEALED BY:      Senior Hack · Donovan Mackey
```

The cook is a teacher · not a product. Its failures map to specific repair blocks. Its successes (Price Analysis · Reprice LOI) seal as HONEY. Its regressions (Memphis · Capital Markets · Waterfall · LOI errors) seal as PROPOLIS and feed v2 corpus design.

This is the firm-OS flywheel working as designed: cook → eval → tribunal → repair corpus → cook v2. The only failure would have been shipping Cook v1 to production. Tribunal prevented that.

---

## Cross-references

- [Tribunal Before Training](../doctrine/tribunal-before-training.md) · the doctrine that governs this verdict
- [`reports/atlas27b_qwen36_dev_eval_report.md`](../reports/atlas27b_qwen36_dev_eval_report.md) · the evidence
- [`tribunals/2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json`](./2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json) · machine-readable failure → repair-block mapping
- [`doctrine/atlas-quality-stack.md`](../doctrine/atlas-quality-stack.md) · what was actually in the model

---

**End of Tribunal Verdict.**
