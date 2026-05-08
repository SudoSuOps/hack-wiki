# 🐝 Tribunal Verdicts · Sealed Cook Rulings

Per [Tribunal Before Training](../doctrine/tribunal-before-training.md), every cook produces a sealed Tribunal Verdict at Stage 6 of the Tribunal Sequence. This directory holds those verdicts.

A Tribunal Verdict is **not an eval report**. The eval report (in [`reports/`](../reports/)) is the analysis. The Tribunal Verdict is the formal ruling — sealed by the senior hack, citing the doctrine, classifying the cook, and routing the cook's outputs to Royal Jelly tiers.

---

## Active Verdicts

### [Atlas 27B Qwen 3.6 Cook v1 · 2026-05-08](./2026-05-08-atlas-qwen-27b-cook-v1.md)

> **Do not promote to IC Draft Ready. Classify as Jelly/Propolis. Use Cook v1 failures to generate Block-2 repair corpus. Deploy Base + v4 prompt + deterministic validator for internal analyst assist only.**

The first formal Tribunal verdict under the Tribunal Before Training doctrine. Cook v1 landed clean by conventional metrics (loss 0.4540 · 7-of-7 PROMOTE on lenient Stage 5 rubric) but base outperformed cooked on 6 of 10 directly comparable institutional CRE prompts. The cook was a net regression on math depth and reconciliation reflex.

Machine-readable failure taxonomy: [`2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json`](./2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json) — 4 critical failures mapped to 6 repair blocks for Block-2 corpus generation.

**Sealed by:** Senior Hack · Donovan Mackey

---

## Verdict File Naming Convention

```
YYYY-MM-DD-<model-id>.md
YYYY-MM-DD-<model-id>-failure-taxonomy.json
```

Date is the Tribunal seal date (not the cook completion date). Model ID is the canonical model slug.

---

## Verdict Classification Tiers

Per [Tribunal Before Training doctrine](../doctrine/tribunal-before-training.md):

| Verdict | Meaning | Deployment |
|---|---|---|
| **APEX** | Trained model significantly outperforms base · institutional gold | Customer-facing after senior-hack review |
| **HONEY** | Trained model meets or exceeds base on majority of prompts | Production deploy after senior-hack final review |
| **JELLY** | Trained model partial improvement · some categories tied or weaker | HITL gate · internal analyst assist only |
| **JELLY/PROPOLIS** | Trained model net regression on majority · some honey pairs salvageable | Internal analyst assist only · feed failures to next cook's repair corpus |
| **PROPOLIS** | Trained model worse than base across the board | Failure receipt · do not deploy · retrain priority |

---

## How to Add a New Verdict

When a new cook completes Stage 6 of the Tribunal Sequence:

1. Copy `2026-05-08-atlas-qwen-27b-cook-v1.md` as a template (it is the canonical verdict structure)
2. Update frontmatter: cook ID · base model · sealed date · classification
3. Fill in: what was cooked · why it failed/passed · base-vs-trained comparison · Royal Jelly routing · repair blocks (if applicable) · production deployment decision
4. Generate companion failure-taxonomy JSON with critical_failures and repair_blocks fields
5. Cross-link to: Tribunal Before Training doctrine · the eval report (in `reports/`) · sister cook verdicts
6. Submit via PR. Senior hack seals or rejects.

The verdict is not sealed until the senior hack signs the frontmatter `sealed_by` field with a timestamp.

---

## Cross-references

- [`doctrine/tribunal-before-training.md`](../doctrine/tribunal-before-training.md) — the doctrine that governs all Tribunal verdicts
- [`reports/`](../reports/) — eval analysis that produces the evidence verdicts cite
- [`doctrine/atlas-quality-stack.md`](../doctrine/atlas-quality-stack.md) — corpus discipline that fed the cooks being judged
