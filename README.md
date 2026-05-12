# customs-trade-law — U.S. Customs and Trade Law Skill

An Agent Skill for U.S. trade & customs classification, CROSS ruling research, CIT/CAFC decision analysis, duty rate compilation, country of origin determination, and compliance review.

> **Disclaimer.** All outputs produced by this skill are **draft work product** intended for review by a U.S.-licensed attorney or licensed customs broker. Nothing in this skill or its outputs constitutes legal advice, and no attorney-client or broker-client relationship is created by its use. Users must independently verify all classifications, duty rates, and compliance determinations before relying on them for any import transaction.

**Author:** M. Onur Kafkas
**License:** [AGPL-3.0](./LICENSE)
**Skill version:** 1.0.1

---

## Install

Use Claude Code's marketplace install (two commands):

```
/plugin marketplace add onurkafk/customs-trade-law
/plugin install customs-trade-law@onurkafk
```

After install, restart Claude Code (or start a new session). The skill auto-triggers when you mention HTS classification, tariff lookups, CROSS rulings, CIT/CAFC decisions, duty calculation, country of origin, Section 301/232/201, AD/CVD, PGA, or UFLPA topics. No slash command needed.

### Alternative: clone directly

```sh
git clone https://github.com/onurkafk/customs-trade-law.git /tmp/customs-trade-law
cp -R /tmp/customs-trade-law/skills/customs-trade-law ~/.claude/skills/
```

> The repo was previously published at `onurkafk/trade-law`. GitHub auto-redirects the old URL, so existing clones keep working — but new installs should use the URL above.

---

## Required permissions

The skill fetches data from live U.S. government sources. Before first use, add these `WebFetch` domains to your Claude Code settings (`~/.claude/settings.local.json` or project-level):

```json
{
  "permissions": {
    "allow": [
      "WebFetch(hts.usitc.gov/*)",
      "WebFetch(www.usitc.gov/*)",
      "WebFetch(catalog.data.gov/*)",
      "WebFetch(search.uscourts.gov/*)",
      "WebFetch(www.cit.uscourts.gov/*)",
      "WebFetch(law.justia.com/*)",
      "WebFetch(www.federalregister.gov/*)",
      "WebFetch(rulings.cbp.gov/*)",
      "WebFetch(ustr.gov/*)",
      "WebFetch(www.trade.gov/*)",
      "WebFetch(www.cbp.gov/*)"
    ]
  }
}
```

Automated merge into an existing settings file:

```sh
jq '.permissions.allow += [
  "WebFetch(hts.usitc.gov/*)",
  "WebFetch(www.usitc.gov/*)",
  "WebFetch(catalog.data.gov/*)",
  "WebFetch(search.uscourts.gov/*)",
  "WebFetch(www.cit.uscourts.gov/*)",
  "WebFetch(law.justia.com/*)",
  "WebFetch(www.federalregister.gov/*)",
  "WebFetch(rulings.cbp.gov/*)",
  "WebFetch(ustr.gov/*)",
  "WebFetch(www.trade.gov/*)",
  "WebFetch(www.cbp.gov/*)"
] | .permissions.allow |= unique' ~/.claude/settings.local.json > /tmp/s.json && mv /tmp/s.json ~/.claude/settings.local.json
```

---

## What the skill does

| # | Workflow | Description |
|---|----------|-------------|
| 1 | Classify a product | Full GRI 1–6 analysis with CROSS research, CIT/CAFC check, duty compilation |
| 2 | Research CROSS rulings | Search and digest CBP rulings with authority-level assessment |
| 3 | Analyze CIT/CAFC decision | Court decision briefing with precedent mapping and strategic analysis |
| 4 | Calculate duty rate | Revision-aware: General + Special + Chapter 99 + AD/CVD + MPF/HMF |
| 5 | Check surcharges | Section 301 / 232 / 201 applicability and exclusion screening |
| 6 | Country of origin analysis | Marking, FTA qualification, TAA compliance, substantial transformation |
| 7 | Full compliance review | Classification + duty + origin + PGA + UFLPA screening |
| 8 | Source / evidence control | HTS Data.gov discovery, evidence ledger, freshness blocks, human-review triggers |

The full authority hierarchy (HTSUS legal text > CAFC > CIT > CBP HQ > CBP NY > ICPs > secondary), the HTS data discovery protocol, and the workflow router live in [`skills/customs-trade-law/SKILL.md`](./skills/customs-trade-law/SKILL.md).

A worked end-to-end example is at [`skills/customs-trade-law/examples/output.md`](./skills/customs-trade-law/examples/output.md).

---

## Domains the skill accesses

| Domain | Purpose |
|--------|---------|
| `hts.usitc.gov` | USITC HTS REST API (tariff data) |
| `www.usitc.gov` | USITC website (general reference) |
| `catalog.data.gov` | Data.gov HTS catalog metadata and JSON discovery |
| `search.uscourts.gov` | Federal court decision search |
| `www.cit.uscourts.gov` | Court of International Trade slip opinions |
| `law.justia.com` | CIT/CAFC full-text decisions |
| `www.federalregister.gov` | AD/CVD orders and trade actions |
| `rulings.cbp.gov` | CBP CROSS rulings |
| `ustr.gov` | Section 301 official source materials |
| `www.trade.gov` | Commerce/ITA AD/CVD source materials |
| `www.cbp.gov` | CBP guidance, UFLPA, WRO, and import requirements |

---

## Repository layout

```
customs-trade-law/
├── .claude-plugin/
│   └── marketplace.json            # Marketplace manifest (owner: onurkafk; one skill)
├── README.md                       # This file
├── LICENSE                         # AGPL-3.0
├── CHANGELOG.md                    # Version history
├── .gitignore
└── skills/
    └── customs-trade-law/
        ├── SKILL.md                # Skill manifest + workflow router (lq_ai frontmatter)
        ├── examples/output.md      # Worked classification example
        ├── references/             # Methodology, doctrine, source maps, glossary (23 files)
        ├── templates/              # Five output templates
        └── scripts/                # Python helpers (HTS resolver, CIT fetcher, hierarchy builder)
```

---

## Roadmap

- **v1.1** — Customs Valuation (transaction value, assists, royalties, first-sale)
- **v1.2** — Entry & Post-Entry (entry types, protests, reconciliation, prior disclosure)
- **v1.3** — AD/CVD Deep Dive (scope rulings, circumvention, EAPA)
- **v1.4** — Quota & TRQ
- **v1.5** — Foreign Trade Zones
- **v1.6** — Entry Document Review

---

## License

AGPL-3.0 — see [`LICENSE`](./LICENSE).
