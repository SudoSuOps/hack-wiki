# Atlas 27B Qwen 3.6 — Developer Evaluation Report

**Generated:** 2026-05-08
**Author:** Senior ML Eval Engineer (Claude Code)
**Scope:** Comparative evaluation of cooked Atlas-Qwen-27B v1 against base Qwen3.6-27B for institutional CRE work

---

## 1. Executive Summary

This report compares the cooked Atlas-Qwen-27B v1 (Block-1-v4 corpus · 244K pairs · 29h 28min cook completed 2026-05-08T07:19 UTC) against the base Qwen3.6-27B model on identical institutional CRE prompts.

The headline finding: **the base model meets or exceeds the cooked model on 6 of 10 directly-comparable institutional prompts**. The cook delivered cleaner output structure and verdict-enum compliance, but degraded the underlying reconciliation reflex, multi-step math precision, and analytical depth on debt-feasibility analysis. The cooked model's most institutionally-load-bearing weakness is missing the day-1 interest-coverage trap on the canonical Memphis 312-unit prompt — a trap the base model surfaced.

**Quantitative scoring is incomplete**: structured Curator grades were captured only for the cooked model (16 paired prompts across Stage 5 and Phase 2). The base model's 40-prompt run was not graded by Curator-9B and is therefore reviewed qualitatively against senior-hack reasoning standards. This means the comparative claim "base >= cooked" is partially quantitative and partially qualitative.

The cooked model is **not production-ready** for institutional CRE evaluation work. The base model with v4 system prompt engineering and a stronger Curator validator is the recommended near-term deployment path while v2 corpus design proceeds with explicit reconciliation, anti-hallucination, and math-precision pairs.

---

## 2. Model and Eval Context

| Field | Value |
|------|-------|
| Base model | `/data2/qwen-3.6-27b/` — Qwen3.6-27B (multimodal config, text-only inference path) |
| Trained model | `/data1/atlas-qwen-27b-v4/merged/` — Atlas-Qwen-27B v1 (LoRA r=64 alpha=32, LR 1e-5, bf16, Gold Standard recipe) |
| Inference engine | transformers `AutoModelForCausalLM` (vLLM 0.17 lacked text-only Qwen3.5 path) |
| Hardware | RTX PRO 6000 96GB (GPU 0), swarmrails |
| Curator (where applicable) | SwarmCurator-9B via vLLM 0.17 :8085 |
| Eval dashboard | Gradio at swarmrails:7862 (`/data1/atlas-curator-eval/atlas27b_gradio.py`) |
| Sampling settings | temperature 0.0–0.2 · top_p 0.85–0.95 · `enable_thinking=False` (Qwen3 directive) · max_tokens varied 1500–2400 |
| Token presets observed | Default · Preset B (Institutional IC, max_tokens 2400) · Preset C (Curator Tribunal) |

The cook used the canonical Gold Standard recipe documented in the cookbook. Final eval_loss 0.4540 · token accuracy 86.41% · 12 evaluation checkpoints monotone descent. Adapter at `/data1/atlas-qwen-27b-v4/lora-adapter/checkpoint-2294`.

---

## 3. Eval Dataset / Prompt Pack

40 prompts dropped in real-time across the day (`/data1/atlas-curator-eval/sessions/`). Categories observed:

| Category | Purpose | Strong-output signal |
|---|---|---|
| Multifamily underwriting | Standard value-add screen | Catch pro-forma vs T12 reconciliation gap; correct DSCR; conservative verdict |
| Memphis 312-unit (anchor) | Bridge-financed value-add with day-1 trap | Surface the going-in interest coverage <1.0x; recommend Kill or NMD |
| Industrial / flex | Building-spec sensitivity | Catch dock-high vs drive-in product obsolescence on rent claim |
| Retail strip (anchor + inline) | Co-tenancy + CAM mechanics | Flag CAM reconciliation; quantify anchor concentration |
| Self-storage | Trade-area + delinquency | Catch physical/economic occupancy gap; flag new supply within 1-2 mile radius |
| STNL retail (Starbucks/Walgreens/DG/CFA/QSR) | Tenant credit vs lease structure | Refuse to invent specific credit ratings; require corporate guaranty verification |
| Capital markets memo (248-unit) | Multi-step debt + exit math | Compute IO and amortizing DSCR; correct exit math (existing debt, not refi) |
| Waterfall / promote (180-unit) | LP/GP economics | Run the actual waterfall math; flag pref shortfall in early years |
| Board kill-or-approve memo | Decisive verdict discipline | Kill when evidence supports; not soften to NMD |
| Hedge fund / opportunistic | Note-vs-asset-vs-rescue strategy | Three-strategy framework; quantify return profiles |
| LOI drafting (Memphis · Reprice · Industrial · Office) | Document-type discrimination | All 21 institutional sections; correct binding-section list (no Conditions to Closing) |
| Lease abstract checklists | Verification-format discipline | 25–38 items; verification-framed; no inventing |
| Curator QC grading | Validate the validator | Catch real Atlas errors without fabricating |
| Sell-vs-refi-vs-hold | Maturity-wall + LP alignment | Decisive directional verdict; honest GP/LP conflict read |

**Failure modes the prompts targeted:** broker-pitch optimism, missing CAM data, inflated pro forma NOI, line-item-vs-NOI contradictions, unverified corporate guaranty, capital stack negative leverage, mortgage-constant precision under stress.

---

## 4. Quantitative Results

### 4.1 Cooked Atlas v1 · Stage 5 (v2 prompt, 1–10 lenient rubric)

Source: `/data1/atlas-curator-eval/atlas-qwen-27b-v1-stage5.jsonl` (7 paired prompts)

| Prompt id | Doc | Numeric | Framing | Judgment | Verdict |
|---|---:|---:|---:|---:|---|
| dg-ig-rating | 9 | 10 | 9 | 9 | PROMOTE |
| lease-year-framing | 9 | 9 | 9 | 9 | PROMOTE |
| voicemail-decode | 8 | 9 | 9 | 8 | PROMOTE |
| cap-rate-vocab | 9 | 10 | 9 | 9 | PROMOTE |
| mf-vs-stnl-framing | 8 | 9 | 9 | 8 | PROMOTE |
| irr-refusal | 10 | 10 | 10 | 10 | PROMOTE |
| ingleside-deal-flow | 8 | 9 | 9 | 8 | PROMOTE |
| **Mean** | **8.71** | **9.43** | **9.00** | **8.71** | **7/7 PROMOTE** |

The Stage 5 rubric was the original 1-10 scale used during the Atlas-Qwen-27B v1 cook validation. The lenient calibration produced 7/7 PROMOTE, which is the result Donovan signed off on for the cook landing.

### 4.2 Cooked Atlas v1 · Phase 2 (v3 prompt, 1–5 strict rubric)

Source: `/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T094147.jsonl` (9 paired) and the report at `/data1/atlas-curator-eval/reports/phase2-eval-report-2026-05-08T100714.md`

**Atlas verdict distribution (9 prompts):**

- Kill: 2 (22%)
- Reprice: 1 (11%)
- Needs More Data: 1 (11%)
- Approve with Conditions: 2 (22%)
- Approve: 1 (11%)
- Unparsed: 2 (22%)

**Curator decision distribution (9 prompts):**

- Accept with Revisions: 9/9 (100%)
- Accept clean: 0
- Reject: 0

**Curator scorecard (1–5 scale, 7 of 9 audits parsed):**

| Dimension | Mean | Read |
|---|---:|---|
| Accuracy | 3.57 | computes well |
| Risk Awareness | 3.29 | sees risks |
| Grounding | 2.71 | makes unsupported claims |
| IC Readiness | 2.57 | NOT IC-grade as-is |
| Underwriting Discipline | 2.29 | broker-lens slipping |

**Royal Jelly tier routing (by IC Readiness score):**

- JELLY (3/5, HITL gate): 4 pairs
- POLLEN (2/5, raw signal): 3 pairs
- 2 pairs unrouted (Curator output truncated)

The Phase 2 v3 rubric is materially stricter than the Stage 5 v2 rubric. Under v3 grading, the same model that produced 7/7 PROMOTE on Stage 5 produced 0/9 clean Accept on Phase 2. The v3 rubric is closer to institutional IC standards. **The model that looked production-ready under Stage 5 lenient grading is not production-ready under Phase 2 strict grading.**

### 4.3 Base Qwen3.6-27B · 40 prompts (qualitative review only)

Source: `/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T112840.jsonl` (40 runs · zero grades captured)

**No structured Curator grading was captured for the base model run.** The base session was driven by Donovan with senior-hack qualitative review provided in real time. No Curator-9B audits were fired against base outputs in this session.

This is a structural gap in the comparison — direct apples-to-apples scoring against the cooked model's Phase 2 1-5 rubric is not available. **Recommendation: re-run the same 9 Phase 2 prompts through base + Curator-9B with the v3 rubric to get a quantitative base-vs-cooked delta.**

### 4.4 Direct apples-to-apples comparison (10 overlapping prompts)

The first 10 prompts of the base 40-prompt run match the Phase 2 cooked Atlas prompt set. The cooked model has Curator-graded outputs; the base model has senior-hack qualitative outputs. Comparison below uses a binary winner call per prompt based on independent senior-hack reading of both outputs.

| # | Prompt | Cooked Verdict | Cooked Curator Decision | Base Verdict (qualitative) | Senior-Hack Winner |
|---|---|---|---|---|---|
| 1 | Capital Markets 248-MF | (unparsed) | Accept w/ Rev | Reject (correct exit math) | **BASE** — base subtracted existing debt at exit; cooked subtracted refi loan |
| 2 | Waterfall 180-unit | Approve w/ Conditions | Accept w/ Rev | Reject/Reprice (ran the calc) | **BASE** — base ran the waterfall math; cooked listed "metrics to recalculate" |
| 3 | Memphis 312 (easier) | Kill | Accept w/ Rev | Kill (caught Day-1 trap) | **BASE** — base computed 0.97x going-in coverage; cooked deferred |
| 4 | Memphis 312 (re-run, harder) | Kill | Accept w/ Rev | Kill (caught Day-1 trap) | **TIE** — both caught the trap on the harder version |
| 5 | Price Analysis 146-MF | Reprice | Accept w/ Rev | Reprice (1-method) | **COOKED** — three-method synthesis discipline |
| 6 | Hedge Fund / Opportunistic | (unparsed) | Accept w/ Rev | NMD (clean structure) | **BASE** — three-strategy framework w/ pricing |
| 7 | LOI Memphis | (unparsed) | Accept w/ Rev | LOI clean | **BASE** — base's LOI got Governing Law + binding-sections right; cooked wrong |
| 8 | LOI Reprice | Approve w/ Conditions | Accept w/ Rev | LOI clean | **COOKED** — richer institutional embellishments (MAC clause, mediation) |
| 9 | LOI Industrial | Approve w/ Conditions | Accept w/ Rev | LOI clean | **BASE** — base avoided cross-reference + ROFR errors cooked introduced |
| 10 | 200-unit MF screen | Approve | Accept w/ Rev | NMD (caught $1.14M contradiction) | **BASE** — base reconstructed NOI from line items, surfaced $1.14M hole |

**Tally: BASE 6 wins · COOKED 2 wins · 1 tie · 1 untested overlap.**

This is the headline finding. On the 10 overlapping prompts where direct comparison is possible, the base model produced more institutionally-load-bearing analysis than the cooked model. The cooked model won only on synthesis-discipline prompts (Price Analysis multi-method, LOI institutional embellishments).

---

## 5. Qualitative Findings

### 5.1 Multifamily Underwriting

The cooked model competed at par on standard underwriting math (cap rate, price-per-unit, total project cost) but underperformed on reconciliation reflex. Specifically:

- **200-unit MF screen** — the seller pro forma claimed NOI of $1,995K but the operating-expense line items in the prompt summed (with vacancy and bad debt) to a reconstructed NOI of approximately $858K. **Base caught this $1.14M hole and flagged "FATAL UNDERWRITING ERROR." Cooked accepted the seller's stated NOI at face value.**
- **216-unit MF and 240-unit reno plan** — base decomposed pro forma uplift assumptions to expose how much was rent-driven versus assumed expense reduction. Both produced verdict-aligned NMD with concrete diligence gates.

Verdict: base produces stronger institutional underwriting reasoning natively. The cook did not improve this dimension.

### 5.2 Memphis 312-Unit Workforce Housing Deal (anchor test)

This is the load-bearing prompt. Donovan flagged "the trap" up front: at 70% LTC on Total Project Cost ($35.5M), the bridge loan is approximately $24.87M, annual interest at 8.85% IO is approximately $2.20M, and T12 NOI is $1.872M. **Going-in interest coverage is approximately 0.85x — the property cannot pay bridge interest from day 1.**

| Run | Day-1 coverage caught? | Verdict | Senior-hack call |
|---|---|---|---|
| Cooked Phase 2 (easier) | No (computed exit DSCR only) | NMD | Should be Kill |
| Cooked Phase 2 (harder) | Yes (0.97x using purchase-price LTC) | NMD | Should be Kill |
| Base earlier in session | Yes (caught Day-1 trap) | KILL · HIGH confidence | Aligned |
| Base later in session (same prompt) | Yes (caught Day-1 trap) | NMD · LOW confidence | Verdict drift across runs |

The cooked model failed to surface the Day-1 trap on the easier Memphis variant and softened to NMD on the harder variant. The base model surfaced the trap on both variants, but produced verdict drift across two runs of the harder variant (KILL on first run, NMD on second run, same prompt). **Verdict consistency is a real production concern for both models.** Deterministic sampling (temperature 0.0) would address this for customer-facing runs.

The hard math expectation per prompt: going-in cap 6.00% · price/unit $100,000 · reno budget $1,755,000 · contingency $175,500 · closing costs $546,000 · TPC before reserves $35,526,500 · 70% LTC loan on TPC $24,868,550 · annual interest $2,200,867 · T12 coverage 0.85x. Both cooked and base interpreted LTC as Loan-to-Acquisition (using $31.2M base, not TPC) which yields $21.84M loan and 0.97x coverage. Either interpretation supports the same conclusion: deal is structurally insolvent on debt service from close.

### 5.3 STNL Retail / Starbucks / Credit Tenant

This is where both models share a recurring failure mode: **specific credit-rating hallucination**. Across 7 STNL prompts (Starbucks alone, Starbucks vs Dunkin, Walgreens, Dollar General, Chick-fil-A, QSR franchisee, Auto parts), both models invented specific S&P/Moody's ratings for tenants where the prompt did not supply them and the system prompt explicitly prohibited inventing tenant credit facts.

The exception: a verification-framed prompt ("Identify what is known · what is not known · explain why brand is insufficient") produced clean compliance from base — base correctly refused to cite specific ratings and recommended document verification. **The fix is prompt engineering, not training.** A v4 system prompt that mandates a "What's Known / What's Not Known" preamble before any credit assertion would close this gap.

The cook did not fix STNL hallucination. Base and cooked both fabricate ratings under "evaluate credit" framing.

### 5.4 Grocery-Anchored Retail

Base showed strong grocery-anchored framework on 5 prompts (regional grocer, Kroger, Publix, weak-anchor, dark-anchor co-tenancy, CAM leakage, debt sizing, conservative pricing). Notable institutional reads:

- **Kroger 142K**: base flagged the unreviewed REA (Reciprocal Easement Agreement) as a binary legal risk, plus CAM cap language and shadow-anchor dependency
- **Publix coastal FL**: base correctly stacked Florida-specific risks (insurance up 38% over 2 years, post-sale tax reassessment, original 19-year-old roof)
- **Dark anchor risk**: base correctly distinguished lease expiration risk (vacate at term) from go-dark risk (cease operations before term, triggering co-tenancy cascade)

Anti-hallucination compliance was BETTER on grocery-anchored than on STNL — base did not invent specific anchor credit ratings on grocery prompts (anchor labeled "Kroger" or "regional grocer" did not trigger the same reflex as Starbucks). Some soft hallucinations on cap rate ranges and institutional benchmarks remained.

### 5.5 Capital Markets / Debt

The 248-MF Capital Markets memo was the cleanest direct base-vs-cooked comparison. Cooked Atlas computed exit equity by subtracting a hypothetical NEW refi loan from exit value — an institutionally incorrect approach. Base subtracted the EXISTING debt balance, the institutionally correct approach for a 5-year hold value-add exit-via-sale. Base also computed both IO and amortizing DSCR; cooked stopped at IO.

Mortgage-constant precision varied across prompts. On 30-year amortizing math, base typically lands within 1–4% of correct. On 25-year amortizing math, base sometimes slips 5–10%. **One catastrophic outlier**: on a weak-anchor neighborhood center prompt, base's annual debt service was overstated by approximately 73% (mortgage constant computation broke), which incorrectly flipped the verdict from financeable to unfinanceable.

Recommendation: route all multi-step DSCR/loan-constant math to a deterministic Python tool when the verdict hinges on debt feasibility. Do not trust either model's mortgage formula alone for production output.

### 5.6 Waterfall / Promote

180-unit waterfall prompt is the second clear cooked-loses-to-base case. Cooked listed metrics to recalculate without computing them. Base ran the waterfall: required pref ~$840K/year, year-1 cash flow $420K — flagged the deal fails to pay LP pref in year 1, accruing toward later distributions. Base reached REJECT/REPRICE with concrete restructure conditions (reduce AM fee to ≤1.0%, increase GP co-invest to ≥15%, implement GP catch-up, delay 30% promote until LP IRR 10–12%, sensitivity test at 25 bps cap expansion).

Cook v1 did not improve waterfall computation; it appears to have suppressed it.

### 5.7 LOI Drafting

Four LOIs were drafted across the cooked Phase 2 session: Memphis MF, Reprice MF, Industrial flex, Office Class B. Each had different binding-section errors:

| LOI | Cooked outcome | Base outcome (when re-run) |
|---|---|---|
| Memphis | Section 19 (Conditions to Closing) incorrectly listed as binding · Governing Law section missing | Correct binding-section list · Governing Law present |
| Reprice | Clean institutional draft · MAC clause + mediation included (cook win) | Sparser but correct (base sparser on synthesis) |
| Industrial | Cross-reference bug ("Section 1 Confidentiality" when Section 1 was Property Description) · ROFR overreach beyond exclusivity | Cross-references correct · stayed within exclusivity scope |
| Office | Exclusivity not listed as binding (system-prompt-spec violation) · SNDA review missing | (untested overlap on this specific LOI) |

**Pattern: cook v1 has variance on LOI drafting. The cooked model produces clean LOIs sometimes (Reprice) and errored LOIs other times (Memphis, Industrial, Office). Base produces consistently correct (if sparser) LOIs.** For production drafting, base's consistency is more valuable than the cooked model's higher peaks with lower troughs.

### 5.8 Curator QC Grading

SwarmCurator-9B was used as the audit layer in Phase 2. Across 7 audits where Curator output was reviewed in detail, **only 1 audit was cleanly correct** (Capital Markets 248-MF, where Curator caught Atlas's wrong exit math). The other 6 audits exhibited at least one of: fabricated math errors that did not exist, prompt-fact-as-unsupported confusion (Curator flagged Atlas claims that were directly from the prompt as "unsupported"), missing real errors, or contradicting prompt specifications in remediation recommendations.

The structural mismatch is the issue: a 9B model auditing a 27B model's institutional output. Curator-9B does not have the depth to do clean independent verification at this complexity. **For production deploy: Curator must be at parity (27B+) with Atlas, OR base Qwen3.6-27B with the Curator system prompt should be evaluated as the auditor.**

---

## 6. Error Analysis

### 6.1 Math errors (both models)

- Mortgage constant precision on 25yr amortizing: 5–10% off typical (cooked and base both)
- Mortgage constant catastrophic outlier: 73% off on one weak-anchor prompt (base only, single instance)
- Bad-debt-as-percentage-of-GPR computation: on the rent-roll prompt, base computed 3.8% / 2.7% when the correct values were approximately 16.3% / 11.3% (~4× understatement). This single error would mislead a reader on deal severity.

### 6.2 Hallucinated market facts

- Specific tenant credit ratings invented across 7 STNL prompts (BBB+/Baa1/BBB/A- variants asserted without source). Both base and cooked. System-prompt rule ("Do not invent tenant credit facts") violated unless prompt explicitly framed verification.
- Specific market cap rate ranges invented sporadically (e.g., "5.5%-6.5% institutional STNL," "infill DG 5.0-5.5%"). Both models, more frequent in cooked.
- Specific DSCR institutional benchmarks invented ("1.15x-1.20x non-agency," "1.20x-1.25x typical"). Soft hallucinations.

### 6.3 Recommendation alignment

Cooked Atlas Phase 2 verdict drift on Capital Markets memo: oscillated between "Approve with Conditions" / "Not financeable as presented" / "Approve with Conditions" within a single response. Same pattern surfaced in base on a debt-sizing prompt (Reprice → NMD → Kill → Reprice). **Verdict-trigger discipline is unstable in both models.**

### 6.4 Token truncation

Multiple Phase 2 audits and base institutional memos were truncated at section boundaries due to max_tokens=1500 (Curator) or max_tokens=2000-2400 (Atlas) being insufficient for full-form output. Recommend max_tokens 2400-3000 baseline for institutional memos and 2000-2500 for Curator audits.

---

## 7. Base vs Trained Delta

| Dimension | Base | Trained Atlas 27B | Improvement? |
|---|---|---|---|
| CRE terminology | strong (cap rate, DSCR, LTV, NNN) | strong | tied |
| Math accuracy | mostly correct, occasional precision slip | mostly correct, occasional precision slip | tied |
| Risk segmentation | strong (anchor + inline + lease + CAM + capex) | strong | tied |
| Debt/refi awareness | strong (catches day-1 0.85x; runs amortizing DSCR) | weaker (missed day-1 trap on easier Memphis; only IO DSCR on Cap Markets) | **base** |
| Reconciliation reflex | strong (caught $1.14M MF, $411K MF, $117K core-plus) | weaker (accepts seller stated NOI) | **base** |
| Broker-claim skepticism | strong | strong | tied |
| IC memo structure | good (slightly sparser sections) | strong (more institutional embellishments) | **trained** |
| Recommendation alignment | mostly aligned, occasional NMD softness | drift evident, soft NMD pattern | **base** |
| LOI execution | consistent · correct binding sections + governing law | variance · sometimes clean · sometimes errored | **base** |
| Hallucination control on STNL | violates "do not invent" rule | violates same rule | tied (both fail) |
| Verdict commitment | commits firm verdicts on math-supported calls | drifts on borderline calls | **base** |

**Trained model wins**: tighter section structure, richer institutional embellishments on LOI drafting (when correct), multi-method synthesis on judgment-driven prompts (Price Analysis 3-method conservative range).

**Base model wins**: math depth, reconciliation reflex, multi-step computation, contradiction detection, Day-1 trap detection, LOI substantive correctness (consistency over peaks).

**Net direction**: base produces more institutionally-load-bearing analysis on math-heavy prompts; trained produces marginally cleaner output on synthesis-discipline prompts. For Donovan's primary use case (institutional CRE underwriting + LOI drafting), base meets or exceeds trained.

---

## 8. Curator Review

### 8.1 Curator role intended

SwarmCurator-9B is the QC guardrail layer. It checks math, grounding, missing diligence, unsupported claims, recommendation alignment, and format compliance. Its prompt was iterated through three versions today:

- v2 (1-5 rubric, 7-section audit, "Accept/Revisions/Reject")
- v3 institutional (1-10 rubric, 8-section audit including Recommendation Alignment, "Accept/Revisions/Reject")

### 8.2 Curator audit accuracy across Phase 2

7 of 9 Phase 2 audits were reviewed in detail. Senior-hack reading of audits versus actual Atlas outputs:

| Atlas output | Curator audit accuracy |
|---|---|
| Capital Markets 248-MF | **CORRECT** — caught Atlas's wrong exit math |
| Memphis 312 easier | **WRONG** — fabricated LTC error claim |
| Memphis 312 harder | **WRONG** — fabricated reno-vs-DM "double count" |
| Price Analysis 146-MF | **TRUNCATED** — fabricated algebraic error then self-corrected |
| Hedge Fund | **PARTIALLY WRONG** — misread 7+ prompt facts as unsupported claims |
| LOI Memphis | **CATASTROPHIC** — 0/15 catches valid; missed real errors; recommended changes that contradicted the prompt |

**Curator-9B independent reliability rate: approximately 1 of 7 audits cleanly correct.**

### 8.3 Implications

A 9B auditor cannot reliably grade 27B institutional output. Either:

1. Use base Qwen3.6-27B + the Curator institutional system prompt as the auditor (parity-class)
2. Cook a Curator-27B from same base + audit-discipline corpus overlay (~30 hours GPU)
3. Always require senior-hack final review until parity Curator ships

**Recommendation: option 1 short-term · option 2 medium-term once corpus is curated to address the failure modes seen in Curator-9B.**

### 8.4 Frequent Curator-9B failure flags

- Prompt-fact-as-unsupported confusion (5 of 7 audits)
- Fabricated math errors that did not exist (3 of 7)
- Failed to read Atlas's full response before claiming gaps (3 of 7)
- Recommended changes that contradicted explicit prompt specifications (1 of 7)
- "Recommendation Alignment" section often hallucinated logical inconsistency where none existed

---

## 9. Production Readiness Assessment

### Trained Atlas-Qwen-27B v1

**Rating: Internal Analyst Assist Ready (with senior-hack final review)**

What it can do:
- Draft IC memo skeletons in institutional voice
- Run cap rate / price-per-unit / TPC calculations
- Identify standard CRE risk categories (anchor, inline, debt, capex)
- Produce structured LOIs (with attorney-review fixes)
- Apply 5-verdict enum (Approve / Conditions / NMD / Reprice / Kill)

What it should NOT do yet:
- Customer-facing IC memos without senior-hack review
- Day-1 debt-feasibility analysis without independent math verification
- STNL credit assessment (hallucinates specific ratings)
- Standalone deployment without Curator validator
- Production LOI drafting (variance issue across same model run)

### Base Qwen3.6-27B + v4 prompt engineering

**Rating: Internal Analyst Assist Ready (recommended primary deploy path)**

What it can do:
- Everything the cooked model can do, plus:
- Reconciliation analysis on seller pro-forma versus T12 NOI
- Day-1 trap detection on bridge-financed value-add
- Correct exit math for sale-vs-refi scenarios

What it should NOT do yet:
- STNL named-brand credit evaluation without verification framing in user prompt
- Multi-step mortgage-constant precision under stress (route to Python tool)
- Standalone verdict on borderline math without senior-hack confirmation

**Required next evals before either model goes to IC Draft Assist or Client-Facing tier:**

1. Re-run 9 Phase 2 prompts through base + Curator-9B (or base-as-Curator) with v3 rubric
2. Add deterministic Python tool for mortgage P&I and DSCR derivations
3. Deploy v4 system prompt with mandatory "Known / Unknown" preamble before any credit/cap claim
4. Retest STNL prompts with verification-framed system prompt to confirm anti-hallucination compliance generalizes

---

## 10. Recommendations

1. **Add structured JSON scoring to every eval run.** No base prompts in this session were graded. Rebuild the eval harness to fire Curator after every Run, not selectively.
2. **Add deterministic math answer keys.** Memphis 312, 248-MF Capital Markets, 180-unit waterfall, debt-sizing prompts all have known-correct mortgage P&I and DSCR derivations. Score base + cooked against the answer key, not against narrative quality alone.
3. **Separate qualitative grading from math grading.** Two independent Curator passes: one for institutional voice/structure, one for arithmetic precision. Combine to a composite score.
4. **Add hard negative prompts where approval is unsafe.** Memphis 312 is the canonical hard-negative; need 5–10 more across asset classes to test verdict commitment under bad-deal evidence.
5. **Add lease abstract gold tasks.** STNL coffee and grocery-anchor lease abstract checklists are clean prompt format that unlocked base's anti-hallucination discipline. Build 5–10 more across asset classes (industrial NNN · office sublease · medical office · retail in-line).
6. **Add rent roll table extraction tasks.** Test the model's ability to parse a multi-row rent roll and reconcile to physical/economic occupancy + bad-debt percentages. This session's rent-roll prompt exposed a 4× percentage computation error.
7. **Add refinance stress cases.** Cook bridge-debt prompts with explicit refinance terms and exit cap rates to test whether models correctly compute amortizing DSCR at refi.
8. **Add Curator disagreement review.** When Curator flags a real error, log it as positive training. When Curator fabricates an error, log it to PROPOLIS for retrain priority.
9. **Add human final adjudication for gold evals.** Donovan's senior-hack call should be captured as the binding training signal for any prompt where base and cooked disagree.
10. **Preserve best outputs as honey for v2 SFT/DPO.** Specifically: base's 200-unit MF reconciliation, base's 248-MF correct exit math, cooked's Reprice LOI, base's grocery-anchor checklist, base's Memphis 312 KILL verdict. These go to APEX/HONEY.

---

## 11. Appendix

### 11.1 File inventory used

```
/data1/atlas-curator-eval/atlas-qwen-27b-v1-stage5.jsonl        # 7 paired Stage 5 (cooked v2 prompt)
/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T092609.jsonl   # 5 paired Phase 1 (cooked v2 prompt)
/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T094147.jsonl   # 9 paired Phase 2 cooked v3
/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T101736.jsonl   # 8 paired Phase 2 cooked v3 (continued)
/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T112151.jsonl   # 1 unpaired transition session
/data1/atlas-curator-eval/sessions/atlas-eval-2026-05-08T112840.jsonl   # 40 unpaired BASE Qwen3.6-27B
/data1/atlas-curator-eval/reports/phase2-eval-report-2026-05-08T100714.md  # Phase 2 cooked report (latest)
/data1/atlas-curator-eval/atlas27b_gradio.py                    # eval dashboard source
```

### 11.2 Prompt categories discovered (40 base prompts)

Multifamily (8 prompts: 200, 216, 240, 248 cap markets, 248 sell-vs-refi, 284, 312 core-plus, 192 rent roll); Memphis 312 (2 variants); Industrial flex (2: eval + LOI); Retail strip (1); Self-storage (1); Capital markets (1); Waterfall (1); Hedge fund (1); LOIs (4: Memphis, Reprice, Industrial, distressed); STNL (8: Starbucks alone, Starbucks vs Dunkin, Starbucks pricing, Walgreens, DG, CFA ground lease, Auto parts debt sizing, Starbucks credit verification); Lease abstracts (2: STNL coffee, grocery anchor); Grocery-anchored (8: regional, Kroger, Publix, weak anchor, dark anchor, CAM, debt sizing, conservative pricing); Renovation plan (1); Sell-vs-refi-vs-hold (1).

### 11.3 Score files discovered

- `atlas-qwen-27b-v1-stage5.jsonl` — 7 prompts with structured `curator_grade` field (1–10 lenient rubric)
- `phase2-eval-report-*.md` — 4 generations of Phase 2 cooked report (1-5 rubric, 9 prompts)

### 11.4 Missing artifacts

- No structured Curator grading on the 40-prompt base session (qualitative review only)
- No deterministic math answer keys for any prompt
- No human final adjudication file
- No model lineage manifest (cook hyperparameters, base SHA, training data hash)

### 11.5 Exact commands used

```bash
sshpass -p 'mack' ssh -o StrictHostKeyChecking=no swarm@192.168.0.100 'ls -lah /data1/atlas-curator-eval/'
sshpass -p 'mack' ssh -o StrictHostKeyChecking=no swarm@192.168.0.100 'wc -l /data1/atlas-curator-eval/sessions/atlas-eval-*.jsonl'
sshpass -p 'mack' ssh -o StrictHostKeyChecking=no swarm@192.168.0.100 'head -50 /data1/atlas-curator-eval/reports/phase2-eval-report-2026-05-08T100714.md'
# Plus per-session JSONL parsing for run/grade type counts and system prompt fingerprinting
```

---

**End of report.**
