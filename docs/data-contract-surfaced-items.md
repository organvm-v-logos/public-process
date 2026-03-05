# Surfaced Items Data Contract (Published Data Dependencies)

Version: `1.0`  
Last updated: `2026-03-05`

`public-process` depends on upstream automation that generates topic intelligence from surfaced reading items.

## Upstream Flow
1. `reading-observatory` writes `feeds/surfaced.json`
2. `essay-pipeline` consumes surfaced items to generate `data/topic-suggestions.json`
3. `public-process` publishes downstream artifacts from those suggestions

## Canonical Surfaced Fields
- `title`: string
- `url`: string
- `relevance_score`: number 0..1
- `matched_collections`: string[]
- `surfaced_date`: `YYYY-MM-DD`

## Compatibility
`essay-pipeline` currently supports both `relevance_score` and `score` input keys. Producer-side canonical field remains `relevance_score`.

## Governance
Schema updates must be coordinated across:
- `reading-observatory` producer
- `essay-pipeline` consumer
- this public-process dependency note
