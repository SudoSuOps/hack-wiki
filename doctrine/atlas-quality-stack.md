# 🐝 Atlas Quality Stack · the corpus, the discipline, the receipts

**Pull date:** 2026-05-07
**Audience:** anyone evaluating "what's actually in the model"

This document shows what trained Atlas-9B, what's training Atlas-Qwen-27B right now, and the discipline that filters every pair before it reaches a GPU.

---

## TL;DR

```
Atlas-9B v1     → 45,039 purpose-built CRE capital-markets pairs
                  29 unique system prompts · loss 0.5253 · 11.15h cook
                  cooked 2026-03-11 on PRO 6000 Blackwell

Atlas-Qwen-27B  → 244,725 pairs · 12 curated sources · disciplined 50% cull
v1 (cooking)      from a 486K v3 superset · apex + honey + royal jelly only
                  every source SHA256-anchored · cap-rate-controlled mixing

Discipline       → jelly verification score ≥ 75 · fingerprint dedup ·
                   schema strip · tight caps to prevent dilution
                   (e.g., 810,097 CRE pairs CAPPED to 20,000 to keep
                   the high-doctrine sources from being drowned out)

Daily flywheel   → virgin honey from real evals captured today ·
                   3 apex · 5 honey · 2 jelly · 2 failure (with
                   correction doctrine attached) · feeds v2 cook
```

---

## 1. ATLAS-9B v1 · the production model

**MANIFEST.json** (lives at `swarmrails:/data2/swarmatlas-9b/MANIFEST.json`):

```json
{
  "model": "SwarmAtlas-9B",
  "base": "Qwen/Qwen3.5-9B",
  "method": "bf16 LoRA r=64 alpha=32",
  "config_source": "Swarm Gold Standard",
  "parent_build": "SwarmAtlas-27B (loss 0.4186, same data)",
  "data": {
    "train_count": 45039,
    "eval_count": 500,
    "train_sha256": "ac558ae35272ac610b0704beba988cf768add7cf4dc885ef2b966fcb9360cd9c",
    "eval_sha256": "8892cc465577bf00dfe6dbd5ecae5756a2b6a46f5b86e1950c9135ae796ef0bb",
    "system_prompt_diversity": 29
  },
  "training": {
    "steps": 844, "max_steps": 844,
    "final_loss": 0.5253,
    "learning_rate": 1e-05,
    "effective_batch": 32, "max_seq_len": 4096,
    "packing": true,
    "epoch_fraction": 0.6
  },
  "hardware": {"gpu": "RTX PRO 6000 Blackwell", "vram_gb": 96},
  "elapsed_hours": 11.15,
  "completed_at": "2026-03-11T21:09:48Z"
}
```

**Source corpus:**
- File: `/data2/datasets/swarmchain-datasets/cre/swarmcapitalmarkets_train.jsonl`
- Size: 456 MB · 45,039 pairs
- Pair schema: `{id, deal_id, task_type, difficulty, messages, metadata}`
- Task types include: `hedge_strategy`, `rate_advisory`, `dscr_stress`, etc.
- Difficulty levels: low / medium / high
- 29 distinct system-prompt voices (verified by sampling)
- Single-source purpose-built corpus (not a scraped mix · every pair is broker-grade synthetic data anchored to a specific deal_id)

**Why it works:** the 29 system prompts × 1,553 pairs avg/prompt = consistent voice across the entire training set. The doctrine ("SwarmCapital · AI-powered capital markets advisor for CRE · 30 years of institutional debt and equity experience") gets reinforced in every batch.

---

## 2. ATLAS-Qwen-27B v1 (cooking right now · step ~1,100/2,294)

**MANIFEST_SLICE.json** (lives at `swarmrails:/data1/atlas-qwen-27b-v4/MANIFEST_SLICE.json`):

```json
{
  "build": "Atlas-Qwen-27B",
  "block_version": "Block-1-v4 (DISCIPLINED CULL · post-peer-review)",
  "built_at": "2026-05-07T01:34:47Z",
  "train": {
    "records": 244725,
    "size_mb": 1325.7,
    "sha256": "885982da27b5008aac275efa577fe506cf4ece2304faedf73ab8312db650e473"
  },
  "compares_to_block_1_v3": {
    "block_1_v3_records": 486428,
    "delta_vs_v3": -241703,
    "delta_pct_vs_v3": -49.7
  },
  "doctrine": "Disciplined cull of v3 per senior peer review. Drops Mixed/
               Specialist/Taste tiers · keeps apex + Honey + Royal Jelly only.
               12 sources (vs 41 in v3) · target 150-180K records. Designed
               to pair with packing=False (SDPA + GDN safe) and re-baselined
               max_steps for proper 0.4-0.6 epoch training depth."
}
```

**The 12 sources · tier breakdown (every one SHA256-anchored):**

| #  | Source                          | Tier                                | Raw    | After dedup | After cap | SHA256 (first 16) |
|----|---------------------------------|-------------------------------------|--------|-------------|-----------|-------------------|
| 1  | bee_hive_train_data             | premium · SwarmRefinery doctrine    | 94,768 | 94,768      | 94,768    | 72feeb6709f99c56  |
| 2  | judge_cre_30k                   | evaluation · CRE A/B/C grading      | 30,000 | 30,000      | 30,000    | 3d182d685db34c65  |
| 3  | signal_canonical                | premium · capped for ratio          | 47,538 | 47,538      | 25,000    | e914027a11f1921f  |
| 4  | grants_royal_jelly_hyphen       | royal_jelly apex                    | 70,394 | 22,076      | 22,076    | 5ad821539120fdf7  |
| 5  | jelly_eval                      | expert · SwarmJudge eval            | 20,764 | 20,764      | 20,764    | 5bad562fa4a48ae9  |
| 6  | cre_honey_volume                | premium · capped tight              | 810,097| 718,370     | **20,000**| 770942e35d62581b  |
| 7  | finance_honey                   | honey                               | 14,366 | 13,145      | 13,145    | bbea3e7da6477a3e  |
| 8  | grants_royal_jelly_underscore   | royal_jelly apex                    | 33,485 | 12,101      | 12,101    | ca64c5eb1ce9f191  |
| 9  | stream_blockchain               | specialist · new economy doctrine   | 5,046  | 5,011       | 5,011     | f9f4e61706c63e81  |
| 10 | finance_royal_jelly_apex        | apex                                | 1,221  | 1,221       | 1,221     | 0c01461be11d88d6  |
| 11 | board_member_500                | apex                                | 500    | 497         | 497       | 537fa460dc258710  |
| 12 | signal_platinum                 | apex                                | 142    | 142         | 142       | 22d04c18581d0431  |
|    | **TOTAL**                       |                                     |        |             | **244,725** |              |

**Note the discipline on row 6:** `cre_honey_volume` had **810,097 raw pairs**. After dedup → **718,370**. Then **CAPPED at 20,000.** That's dropping 698K pairs of a single source to preserve the doctrine balance. Volume-without-quality is the failure mode that gets cooks lobotomized · we cull it tight.

---

## 3. Quality filters · the discipline

Every pair gets filtered before it can land in a training file:

```
filters: {
  "jelly_threshold_verification_score": 75,     ← below this = drop
  "fingerprint_dedup": true,                     ← exact-match dedup
  "min_messages": 2,                             ← need at least Q+A
  "schema_strip": "messages-only"                ← strip heterogeneous metadata
}
```

Total drops in Block-1-v4 build:
- `drop_score_below_75`: 85,765 (cre_honey_volume · doctrine threshold)
- `drop_fingerprint_duplicate`: 76,963 (across all sources)
- `drop_minmsg`: 0 (all sources have ≥2 messages)

---

## 4. Royal Jelly tier system

Every pair carries a tier label · drives downstream routing:

```
APEX        the gold of the gold · doctrine-perfect · always include
            sources: signal_platinum (142) · board_member_500 (497) ·
                     finance_royal_jelly_apex (1,221) ·
                     grants_royal_jelly_* (royal_jelly apex) (34,177)

ROYAL_JELLY (apex sub-tier) · doctrine-aligned · primary training material

HONEY       broker-grade · ships to next-cook by default
            sources: finance_honey (13,145) · cre_honey_volume capped (20,000)

JELLY       useful but flagged · ships to review queue · gets a correction note

POLLEN      generic / weak signal · archive · low training value

PROPOLIS    failure / contamination · quarantine · never trains a model
            (think-tag leakage · IRR fabrication · etc.)

FAILURE     special tag (orthogonal to score) · the wrong-answer side of the
            self-heal arc · highest-information training signal because it
            shows the gap explicitly · feeds the v2 retrain-priority queue
```

---

## 5. Today's flywheel · virgin honey from real evals (2026-05-07)

**13 turns evaluated through Atlas-9B + SwarmCurator-9B dual stack.**
Routed to virgin-honey lanes:

```
/data1/virgin-honey/cre/approved/                ← ships to next cook
├── turn-04-apex.jsonl       Atlas caught all 3 contradictions on Absolute-NNN deal
├── turn-06-apex.jsonl       Atlas honored explicit no-CoT instruction perfect
├── t12-apex-tx-roadhouse-credit-cap-spread.jsonl  ⭐ Atlas saw 5.45 cap on BB+
                                                    tenant was below IG comps · senior-
                                                    broker-grade doctrine call
├── turn-03-honey.jsonl      production-ready broker email
├── turn-05-honey.jsonl      0-100 scoring rubric application
├── turn-07-honey.jsonl      messy broker notes · uncertainty preservation
├── t9-honey-pharmacy-4.2M.jsonl   refused to fabricate IRR explicitly
├── t10-honey-starbucks-ingleside.jsonl
└── t11-honey-starbucks-woodward.jsonl

/data1/virgin-honey/cre/jelly/                   ← review + correction note
├── turn-01-jelly.jsonl      year-12 reference with 7.5y remaining
└── t8-jelly-auto-parts-2.85M.jsonl  same year-numbering pattern

/data1/virgin-honey/cre/failures/                ← v2 corpus repair lane
├── tenant-credit-rating/dg-investment-grade-error.jsonl   (+ correction doctrine)
└── voicemail-handling/t13-voicemail-degenerate-loop.jsonl (+ correction doctrine)
```

Each failure file carries the WRONG ANSWER **and** the CORRECTION DOCTRINE for v2:

```json
{
  "record_type": "correction_doctrine",
  "failure_mode": "tenant_credit_rating_overcorrection",
  "correction_doctrine": {
    "rules": [
      "BBB- or higher from S&P/Fitch is investment grade.",
      "Baa3 or higher from Moody's is investment grade.",
      "BBB / Baa2 is investment grade.",
      "BB+ / Ba1 and below are below investment grade.",
      "A broker claim of 'investment grade tenant' should still be verified..."
    ],
    "training_objectives": [...]
  }
}
```

That's the **flywheel**: every eval session generates honey for the next cook · failures generate correction pairs · in 6 months Atlas v3 has narrow gaps closed by data we earned, not data we scraped.

---

## 6. Provenance summary · receipts at every layer

```
Atlas-9B v1 corpus
  swarmcapitalmarkets_train.jsonl     SHA256 ac558ae35272ac61...
  swarmcapitalmarkets_eval.jsonl      SHA256 8892cc465577bf00...

Atlas-Qwen-27B v1 corpus (cooking now)
  train.jsonl (Block-1-v4)            SHA256 885982da27b5008a...
  eval.jsonl (cross-cook comparable)  SHA256 7f025a264210174a...
  + 12 source files each SHA256-stamped (see table above)

Today's virgin honey
  Each routed pair carries: ts_admitted · session_id · turn_idx · model ·
  curator_grade · source_transcript · routed_to_lane

Honey Ledger (separate SQLite at /data2/swarmdev/honey_ledger.db · 784 MB)
  Cross-references every signal → pair → batch → cook → model → revenue
  Full provenance chain · 7 tables · Hedera-anchorable

The wiki/graph (today)
  github.com/SudoSuOps/hack-wiki                ← the firm's institutional memory
  swarmrails:/data2/swarmdev/hack_graph.kuzu    ← Cypher-queryable nervous system
  13 nodes · 19 edges · seeded with Ben Deskins inaugural lead
```

---

## 7. The compute that produced it

```
Hardware fleet (per memory, 2026-05-04):
  186 GPUs · ~14 TB VRAM total
  126× RTX PRO 6000 Blackwell (96 GB each)
  48× RTX 4500 (32 GB each · 200W · perfect for 24/7 hosting)
  12× RTX 5090 (32 GB each)

Atlas-9B v1 cook footprint:
  GPU: 1× PRO 6000 Blackwell
  Time: 11.15 hours
  Final loss: 0.5253
  Eval loss reference: 0.4186 (parent Atlas-27B same data)
  Cost: ~$1.10 in electrons (11.15h × 450W × $0.10/kWh)

Atlas-Qwen-27B v1 cook footprint (in flight):
  GPU: 1× PRO 6000 Blackwell (GPU 0 swarmrails)
  Wall-clock target: ~30-50h (early-stop expected ~25-30h)
  Currently: step ~1,100 / 2,294 · 48% complete
  Cost so far: ~$2.50 in electrons
```

---

## 8. The discipline that compounds

The corpus stack alone is one half of the moat. The other half is the discipline:

```
PRE-COOK
  · senior-hack peer review on every recipe (canary-then-cook skill)
  · MANIFEST_SLICE built BEFORE training · sha256-stamped · auditable
  · canary smoke run validates the full pipeline before GPU spend
  · /eval-curator skill grades the canary

DURING-COOK
  · cook-monitoring skill tracks 6 telemetry streams continuously
  · gpu-miner-review skill enforces thermal bands (75-82°C target)
  · client-update skill produces 6h cadence updates to stakeholders
  · cook-flightsheet skill records every event · sr hack signs off

POST-COOK
  · /eval-curator graded session (today's 13 turns = the receipt)
  · virgin-honey routing per Royal Jelly tier
  · failures get correction doctrine attached for v2 retrain
  · cook-om generates the offering memorandum (sales-ready document)

PUBLIC LAYER (in build)
  · hack-wiki · github.com/SudoSuOps/hack-wiki
  · graph anchored to Hedera (per Glass-Wall doctrine)
  · evals.defendable.eth (future) for third-party verifiable receipts
```

---

## Iconic line

> *"Quality compounds. Volume without quality dilutes.*
> *Cap the volume. Anchor the quality. The model lives at that ratio."*

— Swarm & Bee operating doctrine, 2026

---

*This document is auto-generatable from MANIFEST.json + MANIFEST_SLICE.json + virgin-honey tree on swarmrails. Every number above is reproducible from receipts · no narrative · no marketing · the truth from the dial-monitor of the corpus itself.*
