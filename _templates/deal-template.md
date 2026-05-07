---
type: deal
slug: <deal-slug>
name: "Tenant Address City State"
ts_created: 2026-MM-DDTHH:MM:SSZ
ts_updated: 2026-MM-DDTHH:MM:SSZ
ts_first_seen: 2026-MM-DDTHH:MM:SSZ
asset_type: "STNL retail | Industrial | MF | Mixed"
tenant_slug: tenants/<tenant-slug>
market_slug: markets/<market-slug>
property_slug: properties/<property-slug>
broker_slug: brokers/<firm-slug>/<broker-slug>
lead_slug: leads/<lead-slug>
price_usd: 0
noi_usd: 0
cap_rate_pct: 0.0
lease_type: "NNN | Absolute NNN | Modified NNN | Ground"
lease_term_years_remaining: 0.0
rent_bumps: ""
guaranty: "Corporate | Franchisee | Personal | None"
landlord_responsibility: ""
building_sf: 0
lot_acres: 0.0
url_om: ""
status: "first-look | under-review | LOI | escrow | closed | passed | declined | ghosted"
relationships:
  - rel: HAS_TENANT
    target: tenants/<tenant-slug>
  - rel: IS_IN
    target: markets/<market-slug>
  - rel: IS_AT
    target: properties/<property-slug>
  - rel: REFERENCED_BY
    target: leads/<lead-slug>
tags: []
---

# {tenant} · {address} · {city}, {state}

## Snapshot

| Field            | Value                                  |
|------------------|----------------------------------------|
| Tenant           | [{tenant}](../../../tenants/{slug}.md) |
| Asset type       | {asset_type}                           |
| Price            | $X,XXX,XXX                             |
| NOI              | $XXX,XXX                               |
| Cap rate         | X.XX%                                  |
| Lease type       | NNN / etc                              |
| Term remaining   | X years                                |
| Rent bumps       | X% every Y years                       |
| Guaranty         | Corporate                              |

## Sources

- OM: <link>
- Broker: [{broker_name}](../../../brokers/{firm}/{slug}.md)
- Lead: [{lead}](../../../leads/{slug}.md)

## Atlas eval (when run)

- **Cap rate:** [calculated]
- **Tier:** APEX / HONEY / JELLY / etc.
- **Curator score:** N/10
- **Verdict:** Pursue / Pass / Decline / Need-more

## Diligence checklist

- [ ] Lease term verification
- [ ] Tenant credit pull
- [ ] Property condition / roof age
- [ ] Market comps
- [ ] Environmental Phase I
- [ ] Survey + title

## Decision log

[narrative]
