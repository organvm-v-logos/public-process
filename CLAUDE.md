# CLAUDE.md — public-process

**ORGAN V** (Public Process) · `organvm-v-logos/public-process`
**Status:** ACTIVE · **Branch:** `main`

## What This Repo Is

Essays, case studies, methodology documentation, RSS feed, newsletter integration

## Stack

**Languages:** CSS, HTML, Ruby
**Build:** Ruby (bundler)

## Directory Structure

```
📁 .github/
📁 .meta/
📁 _includes/
📁 _layouts/
📁 _posts/
📁 assets/
📁 data/
📁 docs/
    adr
    seed-automation-contract.yaml
    staging-reference.md
📁 essays/
  .gitignore
  CHANGELOG.md
  Gemfile
  LICENSE
  README.md
  _config.yml
  about.md
  index.md
  seed.yaml
  tags.html
```

## Key Files

- `README.md` — Project documentation
- `seed.yaml` — ORGANVM orchestration metadata

## Development

```bash
bundle install  # Install dependencies
bundle exec jekyll serve  # Local server (if Jekyll)
```

## ORGANVM Context

This repository is part of the **ORGANVM** eight-organ creative-institutional system.
It belongs to **ORGAN V (Public Process)** under the `organvm-v-logos` GitHub organization.

**Dependencies:**
- organvm-i-theoria/recursive-engine--generative-entity
- organvm-ii-poiesis/metasystem-master
- organvm-iii-ergon/public-record-data-scrapper
- organvm-iv-taxis/agentic-titan

**Registry:** [`registry-v2.json`](https://github.com/meta-organvm/organvm-corpvs-testamentvm/blob/main/registry-v2.json)
**Corpus:** [`organvm-corpvs-testamentvm`](https://github.com/meta-organvm/organvm-corpvs-testamentvm)

<!-- ORGANVM:AUTO:START -->
## System Context (auto-generated — do not edit)

**Organ:** ORGAN-V (Public Process) | **Tier:** flagship | **Status:** GRADUATED
**Org:** `organvm-v-logos` | **Repo:** `public-process`

### Edges
- **Produces** → `ORGAN-VI, ORGAN-VII`: essay-markdown
- **Produces** → `ORGAN-V`: essays-index
- **Produces** → `EXTERNAL`: rss-feed
- **Consumes** ← `organvm-i-theoria/recursive-engine--generative-entity`: dependency
- **Consumes** ← `organvm-ii-poiesis/metasystem-master`: dependency
- **Consumes** ← `organvm-iii-ergon/public-record-data-scrapper`: dependency
- **Consumes** ← `organvm-iv-taxis/agentic-titan`: dependency

### Siblings in Public Process
`.github`

### Governance
- *Standard ORGANVM governance applies*

*Last synced: 2026-03-08T20:11:35Z*

## Session Review Protocol

At the end of each session that produces or modifies files:
1. Run `organvm session review --latest` to get a session summary
2. Check for unimplemented plans: `organvm session plans --project .`
3. Export significant sessions: `organvm session export <id> --slug <slug>`
4. Run `organvm prompts distill --dry-run` to detect uncovered operational patterns

Transcripts are on-demand (never committed):
- `organvm session transcript <id>` — conversation summary
- `organvm session transcript <id> --unabridged` — full audit trail
- `organvm session prompts <id>` — human prompts only


## Active Directives

| Scope | Phase | Name | Description |
|-------|-------|------|-------------|
| system | any | prompting-standards | Prompting Standards |
| system | any | research-standards-bibliography | APPENDIX: Research Standards Bibliography |
| system | any | research-standards | METADOC: Architectural Typology & Research Standards |
| system | any | sop-ecosystem | METADOC: SOP Ecosystem — Taxonomy, Inventory & Coverage |
| system | any | autopoietic-systems-diagnostics | SOP: Autopoietic Systems Diagnostics (The Mirror of Eternity) |
| system | any | cicd-resilience-and-recovery | SOP: CI/CD Pipeline Resilience & Recovery |
| system | any | cross-agent-handoff | SOP: Cross-Agent Session Handoff |
| system | any | document-audit-feature-extraction | SOP: Document Audit & Feature Extraction |
| system | any | essay-publishing-and-distribution | SOP: Essay Publishing & Distribution |
| system | any | market-gap-analysis | SOP: Full-Breath Market-Gap Analysis & Defensive Parrying |
| system | any | pitch-deck-rollout | SOP: Pitch Deck Generation & Rollout |
| system | any | promotion-and-state-transitions | SOP: Promotion & State Transitions |
| system | any | repo-onboarding-and-habitat-creation | SOP: Repo Onboarding & Habitat Creation |
| system | any | research-to-implementation-pipeline | SOP: Research-to-Implementation Pipeline (The Gold Path) |
| system | any | security-and-accessibility-audit | SOP: Security & Accessibility Audit |
| system | any | session-self-critique | session-self-critique |
| system | any | source-evaluation-and-bibliography | SOP: Source Evaluation & Annotated Bibliography (The Refinery) |
| system | any | stranger-test-protocol | SOP: Stranger Test Protocol |
| system | any | strategic-foresight-and-futures | SOP: Strategic Foresight & Futures (The Telescope) |
| system | any | typological-hermeneutic-analysis | SOP: Typological & Hermeneutic Analysis (The Archaeology) |
| unknown | any | doctoral-dissertation | SOP: Doctoral Dissertation Production for ORGANVM Projects |

Linked skills: evaluation-to-growth


**Prompting (Anthropic)**: context 200K tokens, format: XML tags, thinking: extended thinking (budget_tokens)

<!-- ORGANVM:AUTO:END -->


## ⚡ Conductor OS Integration
This repository is a managed component of the ORGANVM meta-workspace.
- **Orchestration:** Use `conductor patch` for system status and work queue.
- **Lifecycle:** Follow the `FRAME -> SHAPE -> BUILD -> PROVE` workflow.
- **Governance:** Promotions are managed via `conductor wip promote`.
- **Intelligence:** Conductor MCP tools are available for routing and mission synthesis.
