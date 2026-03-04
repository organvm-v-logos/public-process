---
title: "SOP: Doctoral Dissertation Production"
author: "@4444J99"
date: "2026-03-04"
version: "1.0"
status: "active"
---

# SOP: Doctoral Dissertation Production for ORGANVM Projects

## 1. Purpose & Scope

This Standard Operating Procedure codifies the process for producing doctoral-length dissertations (~50,000 words) from mature ORGANVM projects. Each dissertation applies rigorous academic methodology to a production system, transforming operational knowledge into scholarly contribution.

**Target:** One doctoral dissertation per major ORGANVM project that has achieved sufficient maturity.

**Output:** An 8-chapter academic thesis published as individual chapter pages on the public-process site, plus archived source files in the originating project's `docs/thesis/` directory.

## 2. Prerequisites

Before beginning a dissertation, the source project must have:

- [ ] **Production codebase** — at least 6 months of documented development history
- [ ] **Measurable outcomes** — quantitative data from real usage (not hypothetical)
- [ ] **Decision log** — documented architectural decisions and their rationale
- [ ] **Multiple subsystems** — enough complexity for interdisciplinary theoretical treatment
- [ ] **Literature touchpoints** — the project connects to at least 3 established research traditions
- [ ] **Seed contract** — valid `seed.yaml` declaring organ membership and dependencies

## 3. Phase 1: Research Notes

**Duration:** 1-2 weeks | **Output:** Dated plan file

1. Conduct systematic literature review organized by domain
2. Identify 4-6 research traditions relevant to the project
3. Map project features to theoretical frameworks
4. Document research gaps the project addresses
5. Create initial bibliography (target: 60+ sources)

**Artifact:** `.claude/plans/YYYY-MM-DD-{project}-dissertation-research.md`

## 4. Phase 2: Mathematical Foundations

**Duration:** 1-2 weeks | **Output:** Revised plan file (v2)

1. Formalize the project's core algorithms as mathematical objects
2. Identify relevant theorems from each research tradition
3. Draft formal proofs of optimality or bounded performance
4. Map proofs to specific system components
5. Validate proof logic with standard mathematical conventions

**Artifact:** `.claude/plans/YYYY-MM-DD-{project}-dissertation-foundations-v2.md`

## 5. Phase 3: Thesis Drafting

**Duration:** 2-4 weeks | **Output:** 8 chapter files

Draft chapters in order, following the standard academic structure:

| Chapter | Title | Target Words | Content |
|---------|-------|-------------|---------|
| 0 | Preliminary Pages | 2,000-3,000 | Title page, committee page, declaration, abstract, TOC, list of figures/tables, acknowledgments, dedication |
| 1 | Introduction | 4,000-6,000 | Background, problem statement, research questions, hypotheses, scope, significance, definitions |
| 2 | Literature Review | 12,000-16,000 | Review of each research tradition, competitive landscape, gap analysis, integrated framework |
| 3 | Methodology | 6,000-9,000 | Research design, data collection, analysis methods, validity, ethical considerations |
| 4 | Results | 3,000-5,000 | Mathematical proofs, competitive analysis, empirical findings (organized by research question) |
| 5 | Discussion | 7,000-10,000 | Interpretation, implications, conclusions, future research, contributions, reflexive account |
| 6 | References | 1,500-2,500 | APA 7th edition bibliography |
| 7 | Appendices | 4,000-6,000 | Architecture diagrams, scoring rubrics, configuration schemas, data tables |

**Total target:** 40,000-57,500 words (~50,000 typical)

### Writing Standards

- **Voice:** Third person academic, active voice where natural
- **Citations:** APA 7th edition throughout
- **Formulas:** Standard mathematical notation; number equations sequentially per chapter
- **Tables:** Number sequentially per chapter (Table 3.1, Table 3.2, etc.)
- **Figures:** Number sequentially per chapter (Figure 4.1, Figure 4.2, etc.)
- **Cross-references:** "See Section 2.3" / "as shown in Table 4.2"
- **Proofs:** Label as Theorem/Lemma/Corollary; end with QED (or square symbol)

## 6. Phase 4: Integration

**Duration:** 1-2 days | **Output:** Files in source project

1. Create `docs/thesis/` directory in the source project
2. Save each chapter as `NN-{slug}.md` (e.g., `00-preliminary-pages.md`, `01-introduction.md`)
3. Create unified version: `{project-slug}-thesis.md` (all chapters concatenated)
4. Verify word counts per chapter match targets
5. Commit to source project with message: `docs: add doctoral thesis ({word count} words)`

### File Naming Convention

```
docs/thesis/
  00-preliminary-pages.md
  01-introduction.md
  02-literature-review.md
  03-methodology.md
  04-results.md
  05-discussion.md
  06-references.md
  07-appendices.md
  {project-slug}-thesis.md    # unified version
```

## 7. Phase 5: Publication

**Duration:** 1 day | **Output:** Live pages on public-process site

1. **Create chapter directory:** `dissertations/{project-slug}/` in public-process
2. **Add frontmatter** to each chapter file:

```yaml
---
title: "Chapter N: Title"
dissertation: "{project-slug}"
dissertation_title: "Full Dissertation Title"
chapter: N
author: "@4444J99"
date: "YYYY-MM-DD"
tags: [relevant, tags, here]
category: "dissertation"
word_count: NNNN
reading_time: "NN min"
related_repos:
  - org/repo-name
---
```

3. **Create landing page:** `dissertations.md` (or update existing) with abstract, TOC, stat grid
4. **Create announcement post:** `_posts/YYYY-MM-DD-{slug}.md` for RSS feed and index
5. **Update navigation:** Add "Dissertations" link to `_includes/header.html` (if first dissertation)
6. **Update index.md:** Add dissertations section (if first dissertation)
7. **Verify build:** `bundle exec jekyll build` completes without errors
8. **Commit and push** to public-process repo

## 8. Quality Gates

### Completeness Checklist

- [ ] All 8 chapters present with content
- [ ] Total word count >= 40,000
- [ ] Bibliography has >= 50 unique references
- [ ] All mathematical proofs are complete (statement, proof body, QED)
- [ ] Each chapter has proper heading hierarchy (H1 for title, H2 for sections, H3 for subsections)
- [ ] Cross-references are consistent (no dangling references)
- [ ] Tables and figures are numbered and captioned
- [ ] Abstract is <= 350 words
- [ ] All frontmatter fields populated in published chapters

### Publication Checklist

- [ ] Jekyll build succeeds
- [ ] Each chapter page renders correctly
- [ ] Chapter navigation (prev/next) works
- [ ] Landing page TOC links resolve
- [ ] Announcement post appears in main index
- [ ] RSS feed includes announcement
- [ ] Navigation header link works

## 9. Naming Conventions

| Context | Pattern | Example |
|---------|---------|---------|
| Source directory | `docs/thesis/` | `docs/thesis/` |
| Chapter files | `NN-{slug}.md` | `03-methodology.md` |
| Unified file | `{project-slug}-thesis.md` | `precision-pipeline-thesis.md` |
| Site directory | `dissertations/{project-slug}/` | `dissertations/precision-pipeline/` |
| Landing page | `dissertations.md` | (site root) |
| Announcement | `_posts/YYYY-MM-DD-{slug}.md` | `_posts/2026-03-04-precision-over-volume-doctoral-thesis.md` |
| Plan files | `.claude/plans/YYYY-MM-DD-{slug}.md` | `.claude/plans/2026-03-04-precision-pipeline-dissertation.md` |

## 10. Template: Chapter Frontmatter

```yaml
---
title: "Chapter N: Title Here"
dissertation: "project-slug"
dissertation_title: "Full Title of Dissertation"
chapter: N
author: "@4444J99"
date: "YYYY-MM-DD"
tags: [tag1, tag2, tag3]
category: "dissertation"
word_count: NNNN
reading_time: "NN min"
related_repos:
  - org/repo-name
---
```

Reading time formula: `ceil(word_count / 250)` minutes.

## 11. Candidate Projects for Future Dissertations

Projects assessed as dissertation-ready based on maturity, data availability, and theoretical richness:

| Project | Organ | Proposed Dissertation Topic | Research Traditions |
|---------|-------|----------------------------|---------------------|
| `recursive-engine--generative-entity` | I | Recursive Epistemological Computing: Self-Modifying Knowledge Systems | epistemology, category theory, recursive function theory, computational ontology |
| `metasystem-master` | II | Generative Art as Systems Architecture: Computational Aesthetics at Scale | aesthetic theory, generative systems, complexity science, human-computer interaction |
| `public-record-data-scrapper` | III | Web-Scale Data Collection: Ethics and Engineering of Automated Information Retrieval | information science, web architecture, data ethics, distributed systems |
| `agentic-titan` | IV | Multi-Topology Agent Orchestration: Governance Models for Autonomous AI Swarms | multi-agent systems, organizational theory, game theory, control theory |
| `public-process` | V | Public Discourse as Creative Practice: Building in Public as Scholarly Method | rhetoric, digital humanities, practice-based research, media theory |
| `organvm-corpvs-testamentvm` | META | Institutional Memory at Scale: Governance Corpora for Creative Systems | archival science, institutional theory, knowledge management, digital preservation |

### Readiness Criteria

A project is dissertation-ready when it scores >= 4 on these 6 criteria:

1. **Code maturity** — Production-quality codebase with tests and CI (0-2)
2. **Data availability** — Quantitative outcomes from real usage (0-2)
3. **Decision history** — Documented architectural decisions over time (0-2)
4. **Interdisciplinary reach** — Connects to 3+ research traditions (0-2)
5. **System complexity** — Multiple interacting subsystems (0-2)
6. **Novelty claim** — Does something unprecedented in its domain (0-2)

**Minimum score for dissertation:** 8/12

---

*This SOP was established following the production of the first ORGANVM dissertation: "Precision Over Volume" (March 2026, application-pipeline project, ~50,000 words).*
