# CLAUDE.md · hack-wiki repo guidance

Project-specific context for Claude sessions in this repo. Read this before making changes.

---

## What this repo is

`hack-wiki` is the firm's institutional memory for Swarm & Bee. Layer 1 (markdown + YAML frontmatter) of a 3-layer architecture:

- **Layer 1 · Wiki** · this repo · git history is the audit trail
- **Layer 2 · Graph** · Kuzu embedded graph DB on swarmrails (`/data2/swarmdev/hack_graph.kuzu`)
- **Layer 3 · Sync** · `hack_wiki_sync.py` parses frontmatter into graph upserts

Every entity (broker · firm · tenant · market · property · deal · lead · conversation · doctrine · outcome · hack · tribunal) carries YAML frontmatter so the sync script can lift it into Kuzu.

---

## Directory structure (key dirs only)

```
hack-wiki/
├── README.md            entry-point · architecture overview
├── _templates/          entity templates (broker · deal · investor)
├── doctrine/            operating canon · sealed lessons
│   ├── README.md
│   ├── tribunal-before-training.md   ← THE governing doctrine for all cooks
│   └── atlas-quality-stack.md
├── tribunals/           sealed cook verdicts (NEW · 2026-05-08)
│   ├── README.md
│   ├── 2026-05-08-atlas-qwen-27b-cook-v1.md
│   └── 2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json
├── reports/             eval analysis (input to verdicts)
│   ├── atlas27b_qwen36_dev_eval_report.md
│   ├── atlas27b_qwen36_eval_summary.md
│   ├── atlas27b_eval_metrics.json
│   └── atlas27b_eval_metrics.csv
├── investors/           21 family-office entries (Sunbelt micro-tier)
├── brokers/, firms/, tenants/, markets/, properties/
├── deals/YYYY/MM/, leads/, conversations/, outcomes/
└── hacks/               model profiles
```

---

## Sealed doctrine you must respect when proposing model work

### Tribunal Before Training · 2026-05-08

> Tribunal begins before training.

The 7-stage sequence is operational law for every cook in the Swarm fleet:

```
1. Base Eval                  Run base on canary suite · capture failure modes
2. Targeted Repair Design     Generate pairs that repair specific base failures
3. Cook                       Standard Gold Standard recipe
4. Trained Eval               Same prompts as Stage 1 · base-vs-trained
5. Curator Audit              Parity-class auditor · math · grounding · alignment
6. Tribunal Seal              Senior-hack arbitrates · routes outputs to RJ tiers
7. Delta Report               Quantified base-vs-trained-vs-Curator delta
```

**No model enters training without a base-model baseline eval.**
**No synthetic pair is created without a failure it repairs.**
**No trained model is accepted without a base-vs-trained delta report.**
**No output becomes honey without Curator audit and Tribunal seal.**

If you propose a cook that skips Stage 2 (targeted repair design from base eval), it is already pre-rejected. Cook v1 skipped Stage 2 and produced a net regression. Do not repeat.

Full doctrine: [`doctrine/tribunal-before-training.md`](./doctrine/tribunal-before-training.md)

---

## Active Tribunal Verdict

### Atlas-Qwen-27B v1 · 2026-05-08 · Jelly/Propolis

```
VERDICT:        Do NOT promote to IC Draft Ready
CLASSIFICATION: Jelly/Propolis
DEPLOYMENT:     Internal Analyst Assist ONLY
```

The cooked Atlas-Qwen-27B v1 lost to its own base on 6 of 10 directly comparable institutional CRE prompts. Cook delivered net regression on math depth, reconciliation reflex, Day-1 trap detection, and LOI substantive correctness. It won only on synthesis-discipline prompts (Price Analysis multi-method, LOI Reprice institutional embellishments).

**Recommended production stack:** Base Qwen3.6-27B + v4 system prompt + temperature 0.0 deterministic + verification framing on STNL prompts + deterministic Python tool for verdict-driving math + senior-hack final review.

Cook v2 is gated behind Block-2 corpus design with 6 repair blocks (reconciliation_pairs · bridge_refi_pairs · verdict_trigger_pairs · stnl_anti_hallucination_pairs · mortgage_constant_pairs · loi_binding_section_pairs). See [`tribunals/2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json`](./tribunals/2026-05-08-atlas-qwen-27b-cook-v1-failure-taxonomy.json) for machine-readable spec.

Full verdict: [`tribunals/2026-05-08-atlas-qwen-27b-cook-v1.md`](./tribunals/2026-05-08-atlas-qwen-27b-cook-v1.md)

---

## Tone and contributions

- **Founder-grade · no marketing fluff.** The wiki is operational record. If a sentence reads like a brochure, rewrite it.
- **Operational rules over philosophy.** Every doctrine entry must include rules, not just principles.
- **Receipts cross-link.** Every doctrine, verdict, or major analysis cites the report or session log that produced it.
- **YAML frontmatter on entities.** Doctrine and tribunal pages added 2026-05-08 carry frontmatter for graph sync.
- **🐝 emoji prefix on doctrine + tribunal page titles.** Maintains Swarm aesthetic consistency.

---

## How to propose a new doctrine entry

1. The lesson must have cost real money, time, or trust to learn (no theoretical doctrine)
2. Stateable as operational rules (not just philosophy)
3. Senior hack signs off that it reflects how the firm actually runs
4. Cross-link to receipts (eval reports, deal files, conversation logs that prove the doctrine)
5. Use [`doctrine/tribunal-before-training.md`](./doctrine/tribunal-before-training.md) as canonical structure template
6. Submit via PR · senior hack seals or rejects

Marketing fluff gets rejected. Operational substance with terminal proof gets sealed.

---

## How to propose a new Tribunal verdict (after a cook)

1. Cook completes Stage 6 of the Tribunal Sequence
2. Copy [`tribunals/2026-05-08-atlas-qwen-27b-cook-v1.md`](./tribunals/2026-05-08-atlas-qwen-27b-cook-v1.md) as template
3. Update frontmatter (cook ID · base · sealed date · verdict · classification)
4. Fill 11 sections: what was cooked · why it failed/passed · base-vs-trained · RJ routing · repair blocks · production deployment · sequence compliance · sealed verdict · cross-refs
5. Generate companion `failure-taxonomy.json` with `critical_failures` + `repair_blocks` fields (machine-readable for next cook's corpus build)
6. Cross-link to: doctrine · the eval report · sister cook verdicts
7. Senior hack signs `sealed_by` field with timestamp to make it canon

---

## Coding-agent boundary (per Tribunal doctrine)

> Claude can propose. Terminal must prove. Tribunal seals.

If you (Claude) make a claim about model performance, training data quality, or cook outcomes, the claim is a candidate until verified against terminal output and signed by the senior hack. Do not seal pairs into the corpus or promote verdicts without that chain.

---

## Common Bash patterns in this repo

```bash
# SSH to swarmrails (model artifacts + eval data)
sshpass -p 'mack' ssh -o StrictHostKeyChecking=no swarm@192.168.0.100 '<command>'

# Pull latest session JSONL
sshpass -p 'mack' scp swarm@192.168.0.100:/data1/atlas-curator-eval/sessions/atlas-eval-<ts>.jsonl ./

# Sync wiki frontmatter to Kuzu graph (when sync script is wired)
python3 hack_wiki_sync.py
```

---

## What NOT to delete

- Cook v1 merged weights at `swarmrails:/data1/atlas-qwen-27b-v4/merged/` (referenced by Tribunal Verdict)
- Cook v1 LoRA adapter at `swarmrails:/data1/atlas-qwen-27b-v4/lora-adapter/checkpoint-2294`
- Session JSONLs at `swarmrails:/data1/atlas-curator-eval/sessions/` (feed v2 corpus)
- Phase 2 reports at `swarmrails:/data1/atlas-curator-eval/reports/`
- Stage 5 baseline at `swarmrails:/data1/atlas-curator-eval/atlas-qwen-27b-v1-stage5.jsonl`

These artifacts are evidence under sealed verdicts. They feed v2 corpus build. They survive past v2 cook to allow audit trails.

---

**Last updated:** 2026-05-08 (Atlas-Qwen-27B v1 Cook · Tribunal Before Training doctrine sealed)
