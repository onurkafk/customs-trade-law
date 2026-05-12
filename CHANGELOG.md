# Changelog

All notable changes to this skill will be documented in this file. The format is loosely based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versions follow semver per the Agent Skills authoring conventions.

## [1.0.0] — 2026-05-12

First stable release as an Agent Skill.

### Added

- Agentic research protocol: evidence ledger, source-label discipline (Retrieved / Verified / Identified / Unverified), authority hierarchy enforcement.
- HTS data protocol with Data.gov bulk JSON discovery, current-revision selection, and recorded source-of-truth blocks.
- Helper scripts:
  - `scripts/resolve-latest-hts-json.py` — resolves the latest HTS JSON via Data.gov catalog metadata.
  - `scripts/cit-opinion-fetcher.py` — fetches and reads CIT slip opinion PDFs by docket number.
  - `scripts/hts-hierarchy-builder.py` — converts the flat HTS JSON array into an indent-based hierarchy for GRI 6 analysis.
- Eight-workflow router: classification, CROSS research, CIT/CAFC briefing, duty compilation, Chapter 99 surcharge screening, country of origin analysis, full compliance review, source/evidence control.
- Reference library: 23 doctrine and source-map files in `references/` (GRI analysis, essential-character doctrine, additional U.S. rules, Chapter 99 surcharges, CROSS research methodology, CIT decision analysis, country-of-origin analysis, duty rate compilation, special programs decoder, classification confidence, interpretive frameworks, current source map, HTS data sources, human-review triggers, formatting standards, disclaimers, concepts glossary, scope roadmap, search strategies, CIT court info, FTA program codes, section/chapter map, agentic research protocol).
- Five output templates: classification memo, CIT decision brief, compliance review, duty rate summary, ruling digest.
- Worked end-to-end example at `examples/output.md`.

### Changed

- **Packaging.** Restructured from a Claude Code *plugin* (with `plugins/trade-law/` + `.claude-plugin/marketplace.json` + `/trade-law` slash command) into an *Agent Skill* at `skills/customs-trade-law/` following the lq_ai frontmatter convention.
- **Authority.** Frontmatter rewritten to the full `lq_ai:` schema (title, version, author, tags, jurisdiction, trigger_examples, inputs, output_format, etc.).
- **Triggering.** The `/trade-law` slash command was removed. The skill now auto-triggers from its description when the user mentions HTS / customs / tariff / CROSS / CIT / CAFC / Section 301 / duty / origin / UFLPA / PGA topics.
- **License.** AGPL-3.0 applied at both repo level and skill level.
- **Folder semantics.** `methodology/` merged into `references/` (lq_ai convention is a single `references/` folder).

### Removed

- `.claude-plugin/marketplace.json` and `plugins/trade-law/.claude-plugin/plugin.json` (no longer needed in skill format).
- `plugins/trade-law/commands/trade-law.md` (`/trade-law` slash command).
- The `plugins/` tree (content migrated to `skills/customs-trade-law/`).
