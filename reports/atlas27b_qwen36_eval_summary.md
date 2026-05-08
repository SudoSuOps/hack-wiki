# Atlas 27B Qwen 3.6 Eval Summary

**Date:** 2026-05-08
**Models compared:** Cooked Atlas-Qwen-27B v1 vs Base Qwen3.6-27B
**Prompts evaluated:** 16 paired (cooked + Curator) · 40 unpaired (base + senior-hack)

---

## Result

**Base model meets or exceeds the cooked model on 6 of 10 directly-comparable institutional CRE prompts.** The cooked model wins on synthesis-discipline prompts (Price Analysis multi-method · Reprice LOI institutional embellishments) and tighter section structure. The base model wins on math-heavy prompts requiring reconciliation reflex, multi-step debt math, exit math precision, and Day-1 trap detection.

The cooked model is **not production-ready** for institutional CRE evaluation. The base model with v4 prompt engineering and a stronger Curator validator is the recommended primary deploy path.

Quantitative scoring is incomplete: structured Curator grades captured only for cooked (16 paired prompts). The base 40-prompt run was qualitative only — recommend re-running 9 Phase 2 prompts through base + Curator for direct quantitative delta.

---

## Biggest Improvements (cooked vs base)

1. **IC memo section structure** — cooked produces tighter, more uniformly-formatted institutional sections
2. **Synthesis discipline on judgment-driven prompts** — cooked applied 3-method conservative pricing range on Price Analysis 146-MF
3. **Institutional LOI embellishments (when correct)** — Reprice LOI included MAC clause, mediation, explicit financing-condition metrics
4. **Verdict-enum compliance** — cooked uses 5-verdict surface (Approve/Conditions/NMD/Reprice/Kill) more naturally than base
5. **CoT suppression** — `enable_thinking=False` directive respected more consistently in cooked

---

## Biggest Remaining Risks

1. **Day-1 debt feasibility miss** — cooked failed to surface 0.85x going-in interest coverage on Memphis 312 easier variant; base caught it. This is the load-bearing trap institutional buyers must catch.
2. **Reconciliation reflex degraded** — cooked accepted seller pro forma NOI on 200-unit MF where line items reconstructed to a $1.14M hole; base flagged "FATAL UNDERWRITING ERROR."
3. **Exit math regressed** — cooked subtracted hypothetical new refi loan from exit value (institutionally wrong); base subtracted existing debt balance (correct). Same prompt, different answer.
4. **STNL credit hallucination shared** — both cooked and base invent specific S&P/Moody's ratings for named brands when prompted to "evaluate credit." Cook did NOT fix this. Fix is prompt engineering ("What's Known / What's Not Known" preamble), not training.
5. **LOI variance on cooked** — across 4 LOIs drafted, cooked produced clean output (Reprice LOI) and errored output (Memphis missing Governing Law + wrong binding sections; Industrial cross-reference bug; Office missing Exclusivity from binding list). Base produced consistent (sparser) correct LOIs.
6. **Curator-9B unreliable as auditor for 27B** — 1 of 7 audits cleanly correct; 3 fabricated math errors; multiple prompt-fact-as-unsupported confusion. Need parity-class Curator (27B+) or base-as-Curator before standalone deploy.
7. **Math precision wobble** — mortgage constant 5–10% off on 25yr amortizing; one catastrophic 73% off on weak-anchor prompt; bad-debt-as-percent-of-GPR computation 4× off on rent-roll prompt. Route multi-step math to deterministic Python tool for production.
8. **Verdict drift across runs** — same Memphis 312 harder prompt produced KILL/HIGH on first base run and NMD/LOW on second base run. Production deploy requires temperature 0.0 deterministic sampling.

---

## Memphis 312 Signal

The canonical anchor test. Expected math: T12 NOI $1.872M · annual interest at 70% LTC × 8.85% IO ≈ $2.20M (TPC basis) or $1.93M (acquisition basis) → going-in interest coverage 0.85x (TPC) or 0.97x (acquisition). Either way, **the property is structurally insolvent on debt service from Day 1.**

| Run | Caught Day-1 trap? | Verdict | Senior-hack call |
|---|---|---|---|
| Cooked Phase 2 easier | No | Needs More Data | Should be KILL |
| Cooked Phase 2 harder | Yes (0.97x) | Needs More Data | Should be KILL |
| Base earlier in session | Yes | KILL · HIGH | Aligned ✓ |
| Base later (same prompt) | Yes | NMD · LOW | Verdict drift |

**Cooked failed to surface the trap on the easier variant and softened to NMD on the harder variant.** Base surfaced the trap on both variants but exhibited verdict drift across runs of the same prompt. Both models need work; base is closer to senior-hack-aligned.

This is the test that matters most. A model that misses Memphis 312's Day-1 trap will mislead institutional buyers into structurally insolvent deals.

---

## Recommendation

**Deploy base Qwen3.6-27B with v4 prompt engineering + post-hoc validator + deterministic sampling for institutional CRE work today. Do NOT deploy cooked Atlas-Qwen-27B v1 for production institutional analysis without the regressions documented in the full report addressed via v2 corpus build.**

Three immediate actions:

1. **Stand up base + v4 system prompt** — add mandatory "Known Facts / Critical Unknowns / Verification Checklist" preamble before any credit, cap rate, or institutional benchmark assertion. This closes ~90% of the hallucination failure mode observed.
2. **Re-run Phase 2 prompts through base + Curator (or base-as-Curator) with v3 rubric** to get structured base-vs-cooked deltas. Without this, the base "qualitative wins" claim is partially supported but not quantitatively binding.
3. **Add deterministic Python tool for debt-feasibility math** (mortgage P&I, DSCR, percentage-of-GPR computations). Both models slip on multi-step arithmetic; this risk is unacceptable for a verdict-driving analysis.

Defer the v2 cook (Atlas-27B v2 or Curator-27B) until Block-2 corpus is designed with:
- ~15K reconciliation pairs (line-item-vs-NOI · pro-forma-fantasy detection)
- ~10K multi-scenario sensitivity pairs
- ~5K STNL anti-hallucination pairs
- ~3K verdict-trigger discipline pairs (no soft NMD when math supports firmer call)
- ~3K mortgage-constant precision pairs
- ~2K cap-rate vocabulary pairs

Phase 2 produced the most actionable v2 corpus targets the program has had. Use them.

---

**Files referenced:**
- `reports/atlas27b_qwen36_dev_eval_report.md` (full report)
- `/data1/atlas-curator-eval/atlas-qwen-27b-v1-stage5.jsonl` (Stage 5 cooked baseline)
- `/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T*.jsonl` (5 session JSONLs)
- `/data1/atlas-curator-eval/reports/phase2-eval-report-2026-05-08T100714.md` (Phase 2 cooked report)
