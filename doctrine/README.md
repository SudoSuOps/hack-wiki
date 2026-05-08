# 🐝 Swarm Doctrine · Operating Canon

Operating wisdom from running the firm. Not philosophy · operational rules with terminal proof behind them. Each doctrine entry is sealed when the lesson became expensive enough to cost real GPU hours, real broker time, or real LP capital — then captured here so the same lesson does not have to be re-paid.

---

## Active Doctrine

### [Tribunal Before Training](./tribunal-before-training.md)

> *Tribunal begins before training.*

Modern base models are already strong. Fine-tuning is no longer about teaching the model everything — it is about identifying specific failure modes a strong base model exhibits in our domain, generating training pairs that repair exactly those failure modes, and proving the cook delivered the repair under audit. Without a base eval, improvement claims are vibes. With a base eval, improvement claims become measurable deltas.

**Sealed:** 2026-05-08
**Forced into existence by:** Atlas-Qwen-27B v1 dev-eval session where the base model outperformed the cooked model on 6 of 10 directly comparable institutional CRE prompts.

---

### [Atlas Quality Stack](./atlas-quality-stack.md)

The corpus, the discipline, the receipts. What's actually in Atlas-9B v1 (production) and Atlas-Qwen-27B v1 (cooking history). Source data, training recipe, dedup discipline, daily virgin-honey flywheel.

**Pull date:** 2026-05-07

---

## Sister directories

Sealed Tribunal Verdicts (per the Tribunal Before Training doctrine) live in [`tribunals/`](../tribunals/README.md). Doctrine is the operating rule; the verdict is the specific ruling against a specific cook. Both must reference each other.

---

## Adding new doctrine

Doctrine entries get sealed when:

1. The lesson cost real money, real time, or real customer trust to learn.
2. The lesson is general enough to apply to future cooks, deals, or operations.
3. The lesson can be stated as operational rules — not just philosophy.
4. The senior hack has signed off that the doctrine reflects how the firm actually runs.

To propose new doctrine:

1. Copy `tribunal-before-training.md` as a template (it shows the canonical structure)
2. Use 🐝 + title format
3. Include: Doctrine Statement · Why This Exists · Operational Rules · Where This Came From
4. Cross-link to receipts (eval reports, deal files, conversation logs that prove the doctrine)
5. Submit via PR. Senior hack seals or rejects.

Marketing fluff gets rejected. Operational substance with terminal proof gets sealed.
