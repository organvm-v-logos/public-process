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
