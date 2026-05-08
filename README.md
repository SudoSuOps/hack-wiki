# 🐝 Hack-Wiki · CRE Knowledge Graph for Swarm & Bee

The firm's institutional memory. Every dial, lead, deal, conversation, tenant, market, doctrine point — captured here. The wiki is the human-readable layer. The graph (Kuzu, runs on swarmrails) is the machine-readable nervous system. Together they're the moat that compounds with every dial.

> *"The grind compounds only with capture."*

---

## Architecture · 3 layers

```
LAYER 1 · WIKI               this repo · markdown + YAML frontmatter
                             git history is the audit trail
                             public-safe subset → swarmandbee.ai/research

LAYER 2 · GRAPH              Kuzu embedded graph DB on swarmrails
                             /data2/swarmdev/hack_graph.kuzu
                             Cypher-compatible queries
                             served to Atlas + Curator + AIOV pipeline

LAYER 3 · SYNC               hack_wiki_sync.py
                             parses wiki frontmatter → upserts graph
                             runs on git post-commit + cron
```

---

## Directory structure

```
hack-wiki/
├── README.md                this file
├── _templates/              entity templates (broker, deal, etc)
├── brokers/                 individual brokers (Ben Deskins, etc.)
│   └── <firm-slug>/<broker-slug>.md
├── firms/                   brokerage firms (Secure Net Lease, etc.)
│   └── <firm-slug>.md
├── tenants/                 tenants (Starbucks, DG, Texas Roadhouse, etc.)
│   └── <tenant-slug>.md
├── markets/                 geographic markets (Memphis MSA, Del Rio TX, etc.)
│   └── <market-slug>.md
├── properties/              specific addresses (2975 Main St Ingleside)
│   └── <property-slug>.md
├── deals/                   every deal evaluated · organized by date
│   └── YYYY/MM/<deal-slug>.md
├── leads/                   broker outreach · processed via /leads skill
│   └── YYYY-MM-DD-<broker>-<short>.md
├── conversations/           emails · voicemails · calls · meetings
│   └── YYYY-MM-DD-<broker>-<channel>.md
├── doctrine/                operating wisdom (canonical scenes from /dmack)
│   └── <doctrine-slug>.md
├── hacks/                   model profiles (Atlas-9B · Hack-STNL-DG · etc.)
│   └── <hack-slug>.md
└── outcomes/                deal outcomes
    ├── closed/              steak-dinner deals
    ├── passed/              we passed
    ├── declined/            seller declined our offer
    └── ghosted/             went silent
```

---

## YAML frontmatter spec

Every entity carries machine-readable frontmatter that the sync script parses into graph nodes + edges:

```yaml
---
type: broker | firm | tenant | market | property | deal | lead | conversation | doctrine | outcome | hack
slug: kebab-case-id
name: "Display Name"
ts_created: 2026-05-07T15:00:00Z
ts_updated: 2026-05-07T15:00:00Z
# type-specific fields below...
relationships:
  - rel: WORKS_AT | SENT | REFERENCES | HAS_TENANT | IS_IN | IS_AT | BETWEEN | RESULTED_IN | APPLIES_TO | COOKS_FROM | CITES
    target: <slug-of-target-entity>
tags: [list, of, tags]
---

# Human-readable content below

[notes · context · narrative]
```

---

## Graph schema · 11 node types · 10 relationship types

### Nodes
- **Broker** — individuals who represent firms (Ben Deskins)
- **Firm** — brokerage shops (Secure Net Lease)
- **Tenant** — the actual tenants on leases (Starbucks · DG · Texas Roadhouse)
- **Market** — geographic markets (Del Rio TX · Memphis MSA)
- **Property** — specific addresses (2975 Main St Ingleside)
- **Deal** — properties being evaluated (Starbucks Ingleside @ 6.35 cap)
- **Lead** — inbound broker outreach (Ben's 3-deal email + voicemail)
- **Conversation** — emails · voicemails · calls · meetings
- **Outcome** — closed · passed · declined · ghosted
- **Doctrine** — operating wisdom (the Q1 Harvey scene · MAGIC framework)
- **Hack** — model profiles (Atlas-9B · Hack-STNL-DG)

### Relationships
- `WORKS_AT` (Broker → Firm)
- `SENT` (Broker → Lead)
- `REFERENCES` (Lead → Deal)
- `HAS_TENANT` (Deal → Tenant)
- `IS_IN` (Deal → Market)
- `IS_AT` (Deal → Property)
- `BETWEEN` (Conversation → Lead)
- `RESULTED_IN` (Deal → Outcome)
- `APPLIES_TO` (Doctrine → Deal/Lead/Conversation)
- `COOKS_FROM` (Hack → Tenant/Vertical)

---

## How to add a new entry

1. Pick the right directory (entity type)
2. Copy `_templates/<type>-template.md` to your target path
3. Fill in YAML frontmatter (keep relationships pointing to slugs that exist)
4. Write the human-readable content below the frontmatter
5. `git add` + `git commit` + `git push`
6. Sync script auto-updates the graph within 5 min (cron) or instantly (post-commit hook)

---

## Querying the graph

On swarmrails:
```bash
python3 -c "
import kuzu
db = kuzu.Database('/data2/swarmdev/hack_graph.kuzu')
conn = kuzu.Connection(db)
q = conn.execute('MATCH (b:Broker {name: \"Ben Deskins\"})-[:WORKS_AT]->(f:Firm) RETURN f.name')
print(q.get_as_df())
"
```

Or hit the served query endpoint (when wired):
- `http://swarmrails:8090/graph/query` with Cypher payload

---

## Connection to other parts of the firm OS

| System          | How it reads the wiki/graph                                |
|-----------------|------------------------------------------------------------|
| `/leads` skill  | Pulls broker history when processing a new lead           |
| `/eval-curator` | Cites the deal context when grading Atlas's responses     |
| Atlas-9B        | Gets context-rich prompts via wiki lookups before answer  |
| AIOV pipeline   | Renders Defendable receipts using canonical wiki entities |
| Hack-STNL-DG    | Cooks from the DG tenant entity + 19K-store knowledge     |
| Honey Ledger    | Cross-references training-data and operational deal data  |
| Hedera anchors  | Each entity gets a Defendable receipt over time           |
| swarmandbee.ai  | Public-safe subset → /research view                       |

---

## Discipline

- **Every dial** generates a 1-line entry in `/conversations/<date>/`
- **Every lead** processed via `/leads` becomes a wiki entry
- **Every deal** evaluated gets a `/deals/YYYY/MM/<slug>.md` file
- **Every closed deal** triggers a steak-dinner postmortem in `/outcomes/closed/`
- **Every dead deal** gets a why-it-died entry in `/outcomes/{passed,declined,ghosted}/`
- **Weekly check** via `/eval-curator` pulls aggregates from the graph
- **Quarterly review** uses graph queries to populate the MAGIC scorecard

---

## Operating doctrine

Sealed lessons live in [`doctrine/`](./doctrine/README.md). Each entry is operational canon — the lesson cost real GPU hours, real broker time, or real LP capital to learn, then was captured here so it does not have to be re-paid.

Active doctrine:
- [Tribunal Before Training](./doctrine/tribunal-before-training.md) — base eval comes before cook · trained model proves itself against base · Curator audits · Tribunal seals · no shortcuts
- [Atlas Quality Stack](./doctrine/atlas-quality-stack.md) — what's actually in the model · the corpus, the discipline, the receipts

---

## Iconic line

> *"The wiki is the firm's institutional memory.*
> *The graph is its nervous system.*
> *Together they're the moat that compounds with every dial."*

— Donovan, 2026-05-07
