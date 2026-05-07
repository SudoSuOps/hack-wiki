---
type: deal
slug: texas-roadhouse-del-rio-tx
name: "Texas Roadhouse · 1918 Veterans Boulevard · Del Rio, TX"
ts_created: 2026-05-07T15:30:00Z
ts_updated: 2026-05-07T15:30:00Z
ts_first_seen: 2026-05-06T00:00:00Z
asset_type: "STNL retail · restaurant"
tenant_slug: tenants/texas-roadhouse
market_slug: markets/del-rio-tx
broker_slug: brokers/secure-net-lease/ben-deskins
lead_slug: leads/2026-05-06-ben-deskins-3-deals
price_usd: 2477064
noi_usd: 135000
cap_rate_pct: 5.45
lease_type: "NNN (verify)"
guaranty: "Corporate (Texas Roadhouse Inc.)"
url_om: "https://securenetlease.com/properties/texas-roadhouse-del-rio-tx/"
status: "first-look · awaiting-DD · CAP-RATE-FLAG"
flag_credit_vs_cap_mismatch: true
relationships:
  - rel: HAS_TENANT
    target: tenants/texas-roadhouse
  - rel: IS_IN
    target: markets/del-rio-tx
  - rel: REFERENCED_BY
    target: leads/2026-05-06-ben-deskins-3-deals
tags: [stnl, texas-roadhouse, restaurant, del-rio, cap-rate-flag, atlas-apex-call]
---

# Texas Roadhouse · 1918 Veterans Boulevard · Del Rio, TX

## Snapshot

| Field    | Value                                                                             |
|----------|-----------------------------------------------------------------------------------|
| Tenant   | [Texas Roadhouse](../../../tenants/texas-roadhouse.md) · **BB+ (S&P)** · NOT IG   |
| Market   | [Del Rio, TX](../../../markets/del-rio-tx.md) (small market · ~30K pop)           |
| Price    | $2,477,064                                                                        |
| NOI      | ~$135,000                                                                         |
| Cap rate | **5.45%** ⚠ TIGHTER THAN IG STARBUCKS                                             |
| Lease    | NNN                                                                               |
| Guaranty | Corporate (Texas Roadhouse Inc · BB+ S&P · NOT investment-grade)                  |

## ⚠ The cap-rate flag (Atlas APEX call)

**5.45% cap on a BB+ tenant is TIGHTER than the same broker's IG Starbucks at 6.35-6.45%.** That's not how credit-quality should drive pricing. Three plausible explanations:

1. **15-20-yr primary-term lease** justifying the premium
2. **Brand-new build** with full primary-term lease
3. **Market is mispricing the credit risk**
4. **Land/location** doing heavy lifting (Del Rio TX corridor specifics)

Atlas-9B caught this on first eval (cli-fired 2026-05-07):

> "5.45% is below recent market comps for similar freestanding net leases in Tier 2/3 Texas markets (where 6.0-6.5% is more typical). This suggests either: cap rate misstated · NOI inflated · or premium-pricing for 10+ year lease. Aggressive but not impossible — needs verification."

That's the senior-broker call. Atlas's doctrine voice transferred clean from the cook.

## Atlas eval

- **Cap rate:** 5.45% ✓ EXACT
- **Tier:** HONEY 9/10 (curator) · **promoted to APEX** by senior-hack review for the credit-vs-cap-spread catch
- **Pair shipped to:** `/data1/virgin-honey/cre/approved/t12-apex-tx-roadhouse-credit-cap-spread.jsonl`

## Strategic notes

Held back the cap-rate-spread observation from our first email reply (cold first touch · too aggressive). Saving it for the call · let Ben react in person. If he can justify the cap with a 15-20 year primary-term lease, the deal becomes interesting. If he can't, we know.

This deal alone validates that Atlas-9B v1 has real broker-grade lease-economics literacy. Not just cap-rate math · the SPREAD analysis between credit and pricing. Doctrine voice receipt.

## Sources

- **OM:** https://securenetlease.com/properties/texas-roadhouse-del-rio-tx/
- **Broker:** [Ben Deskins](../../../brokers/secure-net-lease/ben-deskins.md)
- **Lead:** [2026-05-06 Ben Deskins 3-deals](../../../leads/2026-05-06-ben-deskins-3-deals.md)
