---
type: deal
slug: starbucks-ingleside-tx
name: "Starbucks · 2975 Main Street · Ingleside, TX"
ts_created: 2026-05-07T15:30:00Z
ts_updated: 2026-05-07T15:30:00Z
ts_first_seen: 2026-05-06T00:00:00Z
asset_type: "STNL retail"
tenant_slug: tenants/starbucks
market_slug: markets/ingleside-tx
broker_slug: brokers/secure-net-lease/ben-deskins
lead_slug: leads/2026-05-06-ben-deskins-3-deals
price_usd: 2472000
noi_usd: 156972
cap_rate_pct: 6.35
lease_type: "NNN (verify · structure not provided in deal sheet)"
lease_term_years_remaining: null
rent_bumps: "not provided"
guaranty: "Corporate (Starbucks Corporation)"
landlord_responsibility: "not provided"
building_sf: null
url_om: "https://securenetlease.com/properties/starbucks-ingleside-tx/"
status: "first-look · awaiting-DD"
ben_anchor_deal: true
relationships:
  - rel: HAS_TENANT
    target: tenants/starbucks
  - rel: IS_IN
    target: markets/ingleside-tx
  - rel: REFERENCED_BY
    target: leads/2026-05-06-ben-deskins-3-deals
tags: [stnl, starbucks, texas, ingleside, ben-anchor, in-the-lane]
---

# Starbucks · 2975 Main Street · Ingleside, TX

## Snapshot

| Field            | Value                                                            |
|------------------|------------------------------------------------------------------|
| Tenant           | [Starbucks](../../../tenants/starbucks.md) · BBB+ (S&P)          |
| Market           | [Ingleside, TX](../../../markets/ingleside-tx.md) (Corpus Christi MSA satellite) |
| Asset type       | STNL retail · single-tenant net lease                            |
| Price            | $2,472,000                                                       |
| NOI              | $157,014 (calculated · cap × price)                              |
| Cap rate         | 6.35%                                                            |
| Lease type       | NNN (verify · landlord responsibilities not provided)            |
| Term remaining   | not provided                                                     |
| Rent bumps       | not provided                                                     |
| Guaranty         | Corporate (Starbucks Corporation · BBB+ S&P)                     |

## Sources

- **OM:** https://securenetlease.com/properties/starbucks-ingleside-tx/
- **Broker:** [Ben Deskins](../../../brokers/secure-net-lease/ben-deskins.md) · [Secure Net Lease](../../../firms/secure-net-lease.md)
- **Lead:** [2026-05-06 Ben Deskins 3-deals](../../../leads/2026-05-06-ben-deskins-3-deals.md)

## Atlas eval (cli-fired 2026-05-07)

- **Cap rate:** 6.35% ✓ EXACT
- **Strengths:** Starbucks corporate guarantor (BBB+) · NNN structure · Ingleside is suburban Corpus Christi MSA satellite
- **Risks flagged:** SF not provided · lease term not provided · cap rate "in range" but tight without lease structure
- **Verdict:** "Need more — insufficient data to underwrite"
- **Tier:** HONEY · 9/10
- **Pair shipped to:** `/data1/virgin-honey/cre/approved/t10-honey-starbucks-ingleside.jsonl`

## Why this is Ben's anchor deal

Highest cap of the 3 (6.35 vs 6.45 vs 5.45) AND BBB+ corporate guarantor. Ben anchored on this in the voicemail because:
- Easiest YES for the buyer (best cap)
- Strongest credit story
- Cleanest pitch (no awkward cap-rate-vs-credit explaining like the Texas Roadhouse)

Classic broker move · lead with the most-likely-yes deal to qualify the buyer.

## Diligence checklist

- [ ] Lease term remaining (asked in our email reply)
- [ ] Roof/structure responsibility (asked in our email reply)
- [ ] Square footage (for $/SF benchmark)
- [ ] Rent bumps schedule
- [ ] Renewal options
- [ ] Property condition / age
- [ ] Market comps for Ingleside / Corpus Christi MSA Starbucks net leases
- [ ] Environmental Phase I
- [ ] Survey + title

## Decision log

- **2026-05-06** · received from Ben (email)
- **2026-05-07** · Atlas eval HONEY 9/10 · need-more verdict · DD questions sent in email reply
- **2026-05-07** · awaiting Ben's reply to lease-term + capex-split question
