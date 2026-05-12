# customs-trade-law

A U.S. customs and trade-law Agent Skill for HTS classification, CROSS ruling research, CIT/CAFC decision briefing, duty compilation, country of origin analysis, and end-to-end import compliance review.

**Author:** M. Onur Kafkas
**License:** AGPL-3.0 (see [`LICENSE`](./LICENSE))
**Version:** 1.0.0

---

## Disclaimer

> **DRAFT WORK PRODUCT.** All outputs produced by this skill are draft work product intended for review by a U.S.-licensed attorney or licensed customs broker. Nothing in this skill or its outputs constitutes legal advice, and no attorney-client or broker-client relationship is created by its use. Users must independently verify all classifications, duty rates, and compliance determinations before relying on them for any import transaction.

---

## What it does

Eight workflows, all triggered automatically from the user's natural-language request:

| # | Workflow | Triggered by |
|---|----------|--------------|
| 1 | Classify a product | GRI 1–6 analysis + CROSS research + CIT/CAFC check + duty compilation |
| 2 | Research CROSS rulings | Search and digest CBP rulings with authority-level assessment |
| 3 | Analyze CIT/CAFC decision | Court decision briefing with precedent mapping |
| 4 | Calculate duty rate | Revision-aware: General + Special + Chapter 99 + AD/CVD + MPF/HMF |
| 5 | Check surcharges | Section 301 / 232 / 201 applicability and exclusion screening |
| 6 | Country of origin analysis | Marking, FTA qualification, TAA compliance, substantial transformation |
| 7 | Full compliance review | Classification + duty + origin + PGA + UFLPA screening |
| 8 | Source / evidence control | HTS Data.gov discovery, evidence ledger, freshness blocks, human-review triggers |

See [`SKILL.md`](./SKILL.md) for the full authority hierarchy, HTS data protocol, and workflow routing.

---

## Required permissions

The skill fetches data from live U.S. government sources. Grant these `WebFetch` domains in your Claude Code settings (`~/.claude/settings.local.json` or project-level) before first use:

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

Automated one-liner (merges into an existing settings file via `jq`):

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

## Install

See the repo-level [README](../../README.md) for clone-based install instructions. Once `skills/customs-trade-law/` is on Claude Code's skill discovery path, the skill auto-triggers when the user mentions HTS, customs, tariff, CROSS, classification, duty, country of origin, or related trade-law topics.

---

## Folder layout

```
customs-trade-law/
├── SKILL.md             # Main instruction file + lq_ai frontmatter
├── README.md            # This file
├── LICENSE              # AGPL-3.0
├── examples/
│   └── output.md        # Worked end-to-end classification example
├── references/          # Methodology, doctrine, source maps, glossary
├── templates/           # Five output templates (classification memo, ruling digest, etc.)
└── scripts/             # Python helpers for HTS JSON discovery and CIT slip-opinion retrieval
```

---

## Domains referenced

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
| `www.trade.gov` | Commerce / ITA AD/CVD source materials |
| `www.cbp.gov` | CBP guidance, UFLPA, WRO, and import requirements |
