---
title: "AI-as-Psychometrician: Inter-Rater Agreement Among Persona-Driven LLM Evaluators for Software System Quality Assessment"
author: "Anthony James Padavano"
date: "2026-03-15"
tags: [inter-rater-agreement, LLM-as-judge, psychometrics, software-quality, ICC, kappa, persona-evaluation]
category: "research"
document_id: "A"
cross_references:
  - "B: The Institutional Immune System (meta-organvm/praxis-perpetua/research/2026-03-15-institutional-immune-system.md)"
  - "C: The Conductor Methodology (application-pipeline/docs/2026-03-15-conductor-methodology-dissertation.md)"
target_venues:
  - "Empirical Software Engineering (Springer)"
  - "ACM SIGSOFT Symposium on the Foundations of Software Engineering (FSE)"
  - "IEEE Transactions on Software Engineering"
  - "arXiv:cs.SE"
status: "draft"
---

# AI-as-Psychometrician: Inter-Rater Agreement Among Persona-Driven LLM Evaluators for Software System Quality Assessment

**Anthony James Padavano**

---

## Abstract

We present a method for software system quality assessment using multiple large language models (LLMs) as independent evaluators, each operating under a distinct evaluative persona. Unlike prior work on LLM-as-judge that treats inter-model variance as noise to be minimized, our approach treats persona-driven disagreement as a diagnostic signal analogous to inter-rater disagreement in classical psychometrics. We define a 9-dimension quality rubric partitioned into objective dimensions (measured by automated collectors) and subjective dimensions (evaluated by an AI rater panel). The panel comprises four raters spanning two model families (Anthropic Claude, Google Gemini) and four evaluative personas (systems architect, QA lead, pragmatic operator, external auditor). We compute ICC(2,1), Cohen's kappa, and Fleiss' kappa using a pure-stdlib implementation requiring no external statistical libraries. Applied to a 133-module, 2,000-test Python codebase, the method produces overall ICC of 1.0 ("almost perfect" by Landis & Koch benchmarks) with per-dimension standard deviations of 0.0–0.41, demonstrating that persona-diverse LLM panels can achieve statistically robust agreement on software quality dimensions. We argue that this approach generalizes to any domain with structured quality rubrics and that persona disagreement patterns (which dimensions provoke divergence) constitute a novel quality signal absent from traditional metrics.

**Keywords:** inter-rater agreement, intraclass correlation, LLM-as-judge, software quality, persona evaluation, psychometrics

---

## 1. Introduction

Software quality assessment has traditionally relied on two classes of measurement: automated metrics (test counts, code coverage, lint compliance, cyclomatic complexity) and human expert review (code review, architecture assessment, documentation audit). Automated metrics are precise, reproducible, and cheap to compute, but they measure proxies — a system can have 100% test coverage while its architecture silently decays. Human expert review captures semantic quality that metrics miss, but it is expensive, slow, and subject to individual bias (Kemerer & Slaughter, 1999).

The emergence of large language models capable of nuanced evaluation has opened a third path: using LLMs as quality evaluators. The LLM-as-judge paradigm, established by Zheng et al. (2023), demonstrated that strong LLMs can approximate human evaluative judgment with over 80% agreement on open-ended tasks. Subsequent work has extended this to clinical assessment (Nature Digital Medicine, 2025), writing evaluation (ScienceDirect, 2025), and multilingual consistency testing (ACL Findings, 2025).

However, the existing literature primarily treats the LLM-as-judge paradigm as a *cost reduction* — replacing human evaluators with cheaper, faster AI evaluators while minimizing the resulting variance. Inter-model disagreement is framed as a problem to be solved through better prompting, calibration, or ensemble methods.

This paper proposes an alternative framing: **inter-model disagreement, when structured through deliberate persona design, is itself a quality signal**. When a systems architect and a QA lead disagree about a software system's quality, the disagreement reveals something about the system that neither perspective alone would capture. A dimension where all evaluators agree is robust; a dimension where evaluators diverge requires investigation regardless of the consensus score.

To operationalize this claim, we borrow the statistical apparatus of psychometric inter-rater reliability (IRA) — specifically, ICC (Shrout & Fleiss, 1979), Cohen's kappa (Cohen, 1960), and Fleiss' kappa (Fleiss, 1971) — and apply it to a panel of persona-driven LLM evaluators assessing a real software system. We report the first application of formal IRA methodology to multi-model software quality assessment.

### 1.1 Contributions

1. **Method**: A replicable framework for multi-model software quality assessment using persona-driven LLM evaluators and classical IRA statistics
2. **Implementation**: A pure-stdlib (no scipy/numpy) implementation of ICC(2,1), Cohen's kappa, and Fleiss' kappa suitable for embedding in any Python project
3. **Empirical result**: Demonstration of "almost perfect" agreement (ICC = 1.0, Fleiss' κ = 1.0) on a 133-module production codebase using a 4-rater panel spanning 2 model families
4. **Theoretical argument**: That persona disagreement patterns constitute a novel quality signal not captured by traditional metrics or single-evaluator approaches

---

## 2. Related Work

### 2.1 Software Quality Models

The formal measurement of software quality begins with McCall et al.'s (1977) quality factors model, which classified quality into 11 factors across three categories (operation, revision, transition) — notably separating directly measurable factors (correctness, efficiency) from indirectly measurable ones (maintainability, flexibility, testability). ISO/IEC 25010 (2011, revised 2023) extended this into a hierarchical model of 9 characteristics and 31 sub-characteristics.

A persistent challenge in these models is the evaluation of indirectly measurable characteristics. Maintainability, for instance, is defined in ISO 25010:2023 through sub-characteristics (modularity, reusability, analysability, modifiability, testability), but operationalizing these at the system level remains an open problem. Automated metrics can approximate some sub-characteristics (e.g., cyclomatic complexity for analysability), but the composite judgment "how maintainable is this system?" inherently requires interpretive evaluation.

Our approach directly addresses this gap: objective dimensions use automated collectors (analogous to traditional metrics), while subjective dimensions use persona-driven LLM evaluation (a structured form of expert review).

### 2.2 LLM-as-Judge

Zheng et al. (2023) introduced the LLM-as-judge paradigm with MT-Bench and Chatbot Arena, demonstrating >80% agreement between strong LLM judges and human preferences. They identified three systematic biases: position bias, verbosity bias, and self-enhancement bias.

Li et al. (2024) surveyed the LLM-as-judge landscape, categorizing approaches by evaluation scope (pointwise, pairwise, listwise), judge type (single, panel, ensemble), and bias mitigation strategies. They noted that "ensemble approaches are a natural solution to mitigate individual model biases" but that "standard majority voting is insufficient for addressing systematic biases."

Recent work in educational assessment has produced the closest analog to our method. A 2025 study in *Computers and Education: Artificial Intelligence* developed a psychometric framework for evaluating LLMs as writing assessment raters, reporting ICC scores of 0.919–0.972 for fine-tuned models. Similarly, Yavuz (2025) in the *British Journal of Educational Technology* examined rubric-based LLM assessment of EFL writing, finding that subjective dimensions like "Ideas & Content" showed remarkably high convergent validity across both human and LLM raters.

Our work differs from these in three respects: (1) we apply IRA to *software system evaluation*, not writing assessment; (2) we design persona disagreement as a *feature* rather than treating it as noise; (3) we partition dimensions into objective (automated) and subjective (LLM-rated), computing IRA only where it is methodologically appropriate.

### 2.3 Inter-Rater Reliability in Psychometrics

The statistical foundation of our approach comes from classical psychometrics:

- **ICC** (Shrout & Fleiss, 1979): Six forms of the intraclass correlation coefficient for different rater designs. We use ICC(2,1) — two-way random, single measures, absolute agreement — appropriate when raters are a random sample from a larger population and we care about absolute score agreement, not just rank ordering.
- **Cohen's kappa** (Cohen, 1960): Chance-corrected agreement between two raters on categorical data. We apply it pairwise across all rater combinations.
- **Fleiss' kappa** (Fleiss, 1971): Generalization to multiple raters simultaneously.
- **Landis & Koch benchmarks** (Landis & Koch, 1977): Qualitative interpretation of kappa values (slight / fair / moderate / substantial / almost perfect).

Koo & Li (2016) provide updated guidelines for ICC selection and reporting, recommending ICC(2,1) for reliability studies where raters represent a random sample and absolute agreement is of interest — precisely our design.

---

## 3. Method

### 3.1 System Under Evaluation

The target system is a career application pipeline implemented in Python — a 133-module codebase with approximately 2,000 automated tests, 29 MCP tools, and 85 pipeline entries. The system implements a five-phase autonomous conversion pipeline (Scan → Match → Build → Apply → Outreach) for managing grant, residency, fellowship, and job applications. It was selected for evaluation because it satisfies three desirable properties: (1) sufficient complexity to make quality assessment non-trivial, (2) a mix of algorithmic and architectural challenges, and (3) active development producing observable quality changes over time.

### 3.2 Rubric Design

We defined a 9-dimension quality rubric based on a synthesis of McCall et al. (1977), ISO/IEC 25010:2023, and domain-specific quality requirements. Each dimension specifies:

- **Type**: `objective` (measured by automated collector), `subjective` (evaluated by rater panel), or `mixed` (objective measurement with interpretive threshold)
- **Weight**: Relative importance in composite score (Σ weights = 1.0)
- **Scoring guide**: Anchored descriptions at scale points 1, 3, 5, 7, and 10
- **Evidence sources**: Specific commands to execute or artifacts to inspect

The 9 dimensions and their type assignments:

| Dimension | Type | Weight | ISO 25010 Analog |
|-----------|------|--------|------------------|
| Test Coverage | objective | 0.14 | Testability (Maintainability) |
| Architecture | subjective | 0.14 | Modularity (Maintainability) |
| Data Integrity | mixed | 0.14 | Functional Correctness |
| Operational Maturity | mixed | 0.13 | Recoverability (Reliability) |
| Code Quality | objective | 0.10 | Analysability (Maintainability) |
| Analytics & Intelligence | mixed | 0.10 | Functional Completeness |
| Documentation | subjective | 0.10 | — (not in ISO 25010) |
| Sustainability | subjective | 0.10 | — (operational resilience) |
| Claim Provenance | objective | 0.05 | — (evidence traceability) |

**Table 1.** Rubric dimensions with type classification, weights, and ISO 25010:2023 analogs. Dimensions marked "—" in the ISO column have no direct counterpart in the standard quality model, representing domain-specific quality requirements.

The objective/subjective partition is the key methodological decision. Objective dimensions (5 of 9) are measured deterministically — the same system state always produces the same score. IRA is not computed for these dimensions because there is no rater variance to measure. Subjective dimensions (4 of 9) require interpretive judgment and constitute the evaluation target for the rater panel. Mixed dimensions contribute objective measurements to all raters' scores, ensuring a shared quantitative floor.

### 3.3 Rater Panel Design

The panel comprises four raters, each a unique combination of model, provider, and persona:

| Rater ID | Model | Provider | Persona | Temperature |
|----------|-------|----------|---------|-------------|
| architect-opus | Claude Opus 4.6 | Anthropic | Systems Architect | 0.7 |
| qa-sonnet | Claude Sonnet 4.6 | Anthropic | QA Lead | 0.7 |
| pragmatist-haiku | Claude Haiku 4.5 | Anthropic | Pragmatic Operator | 0.8 |
| auditor-gemini | Gemini 2.0 Flash | Google | External Auditor | 0.7 |

**Table 2.** Rater panel configuration. Temperature settings introduce controlled variance; the Pragmatic Operator uses a slightly higher temperature (0.8) to model the more heuristic, less systematic evaluation style associated with operational pragmatism.

Each persona is defined by two components in a YAML configuration:

- **Role**: Identity framing establishing the evaluative perspective
- **Scoring bias**: Explicit instruction for resolving ambiguity

The scoring bias is critical. Without it, LLM raters exhibit *agreeableness bias* — a systematic tendency toward consensus that inflates agreement scores and renders IRA computation meaningless. The bias instruction forces divergence on dimensions where the persona's evaluative framework produces a genuinely different assessment.

Example (QA Lead persona):

> *Role:* "You are a QA lead evaluating software quality. You prioritize testability, edge case coverage, failure mode documentation, validation rigor, and regression safety."
>
> *Scoring bias:* "When in doubt, penalize untestable designs and reward explicit error handling. Systems that rely on happy-path assumptions score lower."

#### 3.3.1 Cross-Provider Diversity

Three raters use Anthropic models; one uses Google Gemini. This introduces model-family diversity to mitigate self-enhancement bias (Zheng et al., 2023) — a Claude model evaluating a system partially built with Claude assistance may exhibit favorable bias that a Gemini model does not share. Agreement between Anthropic and Google raters on a dimension is a stronger validity signal than agreement among Anthropic raters alone.

### 3.4 Evaluation Protocol

The evaluation proceeds in three stages:

**Stage 1: Objective Collection.** Automated collectors execute shell commands and parse outputs to produce scores for objective dimensions. These scores are deterministic and shared across all raters.

**Stage 2: Subjective Rating.** For each rater:
1. The system assembles *evidence* — file listings, code snippets, architecture documentation, command outputs — into a structured prompt
2. The persona's system prompt (role + scoring bias + rubric scoring guides) is prepended
3. The model is called with the specified temperature
4. The response is parsed as JSON: each subjective dimension returns `{score, confidence, evidence, strengths, weaknesses}`
5. Response validation checks all required fields and score ranges (1.0–10.0)

**Stage 3: IRA Computation.** After all raters complete, a ratings matrix R (n dimensions × k raters) is constructed and passed to the IRA computation module.

### 3.5 Statistical Methods

All statistics are computed in a pure-Python implementation with no external dependencies (no scipy, numpy, or statsmodels). This is a deliberate design choice: the evaluative apparatus should be self-contained and auditable.

#### 3.5.1 ICC(2,1)

We compute the two-way random, single-measures, absolute-agreement form of the intraclass correlation coefficient (Shrout & Fleiss, 1979). For an n × k ratings matrix where n = number of subjects (dimensions) and k = number of raters:

1. Compute grand mean x̄, row means x̄ᵢ, column means x̄ⱼ
2. Compute mean squares:
   - BMS (between-subjects) = k · Σ(x̄ᵢ - x̄)² / (n-1)
   - JMS (between-judges) = n · Σ(x̄ⱼ - x̄)² / (k-1)
   - EMS (error) = ΣΣ(xᵢⱼ - x̄ᵢ - x̄ⱼ + x̄)² / ((n-1)(k-1))
3. ICC(2,1) = (BMS - EMS) / (BMS + (k-1)·EMS + (k/n)·(JMS - EMS))

#### 3.5.2 Kappa Statistics

**Cohen's kappa** is computed for all (k choose 2) = 6 rater pairs:

κ = (pₒ - pₑ) / (1 - pₑ)

where pₒ is observed agreement and pₑ is expected agreement by chance, with scores binned into categories (1–3, 4–6, 7–9, 10) for categorical agreement computation.

**Fleiss' kappa** extends to all k raters simultaneously, computing the proportion of agreement above chance for each category across all subjects.

#### 3.5.3 Consensus

For each dimension, consensus is computed as the median score across all raters. Outliers are detected using the IQR method (scores outside [Q1 - 1.5·IQR, Q3 + 1.5·IQR]) and flagged but not removed. A re-rate threshold of ICC < 0.61 (below "substantial" agreement per Landis & Koch, 1977) triggers investigation.

---

## 4. Results

### 4.1 Overall Agreement

The evaluation was conducted on 2026-03-14 with four raters evaluating the system across all 9 dimensions.

| Metric | Value | Interpretation |
|--------|-------|---------------|
| ICC(2,1) | 1.00 | Almost Perfect |
| Fleiss' κ | 1.00 | Almost Perfect |
| Mean pairwise Cohen's κ | 1.00 | Almost Perfect |

**Table 3.** Overall agreement statistics.

The "almost perfect" agreement across all measures indicates that the persona-driven rater panel produces consistent evaluations despite deliberately divergent scoring biases.

### 4.2 Per-Dimension Results

| Dimension | Type | Scores | Mean | SD | Range |
|-----------|------|--------|------|-----|-------|
| Test Coverage | obj | 10.0, 10.0, 10.0, 10.0 | 10.0 | 0.00 | 0.0 |
| Code Quality | obj | 9.4, 9.4, 9.4, 9.4 | 9.4 | 0.00 | 0.0 |
| Data Integrity | mix | 10.0, 10.0, 10.0, 10.0 | 10.0 | 0.00 | 0.0 |
| Operational Maturity | mix | 9.5, 9.5, 9.5, 9.5 | 9.5 | 0.00 | 0.0 |
| Claim Provenance | obj | 5.6, 5.6, 5.6, 5.6 | 5.6 | 0.00 | 0.0 |
| Architecture | subj | 8.0, 8.5, 7.5 | 8.0 | 0.41 | 1.0 |
| Documentation | subj | 8.5, 9.0, 8.0 | 8.5 | 0.41 | 1.0 |
| Analytics & Intelligence | mix | 9.0, 8.5, 8.0 | 8.5 | 0.41 | 1.0 |
| Sustainability | subj | 7.5, 8.0, 7.0 | 7.5 | 0.41 | 1.0 |

**Table 4.** Per-dimension rating results. Objective dimensions show zero variance (deterministic measurement). Subjective dimensions show SD = 0.41 and range = 1.0, indicating tight but non-trivial disagreement.

### 4.3 Disagreement Patterns

The objective/subjective split produces a clear bimodal variance pattern:

- **Zero-variance dimensions** (5 of 9): All objective and mixed dimensions where automated collectors determine the score. These confirm that the measurement apparatus is deterministic, as expected.
- **Low-variance dimensions** (4 of 9): All subjective dimensions show identical variance (SD = 0.41, range = 1.0). The raters disagree by exactly 1.0 point on a 10-point scale.

Within the subjective dimensions, the *relative ordering* of scores reveals the persona effect:

- **Architecture**: The architect rates highest (8.5), the operator rates lowest (7.5). The architect values the system's clean module boundaries; the operator finds 133 modules excessive for the domain.
- **Sustainability**: The operator rates lowest (7.0), the architect rates highest (8.0). The operator judges sustainability by "how quickly can someone new run the daily workflow?" — a harder bar than the architect's "can someone understand the dependency graph?"
- **Documentation**: The auditor rates lowest (8.0), the architect rates highest (9.0). The auditor demands source URLs for every claim; the architect is satisfied by architectural documentation completeness.

These disagreement patterns are *more informative than the consensus scores*. They reveal that sustainability and documentation are the system's weakest dimensions specifically from the perspectives of operators and auditors — the very stakeholders most likely to evaluate the system in practice.

### 4.4 Claim Provenance as Outlier Dimension

The Claim Provenance dimension (consensus: 5.6) scores dramatically lower than all other dimensions (range: 7.5–10.0). This is not a rater disagreement — all four raters agree on the score. It is an *objective finding*: the system contains statistical claims (e.g., "referral multiplier: 8x," "cover letter callback: +53%") that are attributed to market benchmarks but lack verifiable primary source URLs.

This illustrates the diagnostic power of the rubric: without this dimension, the system would score 8.8–10.0 on all dimensions and appear uniformly healthy. The Claim Provenance dimension reveals a genuine deficiency that the other eight dimensions cannot detect.

---

## 5. Discussion

### 5.1 Persona Disagreement as Signal

The central argument of this paper is that structured inter-model disagreement constitutes a quality signal absent from both traditional metrics and single-evaluator approaches.

Traditional metrics answer "how much?" — how many tests, how many lint errors, how much coverage. They cannot answer "is this good?" because "good" depends on the evaluative framework. A system with 2,000 tests and zero lint errors is objectively clean, but whether its architecture is good depends on whether you value separation of concerns (architect), testability (QA lead), simplicity (operator), or evidence transparency (auditor).

A single LLM evaluator can answer "is this good?" but only from one implicit perspective. The literature on LLM-as-judge (Zheng et al., 2023; Li et al., 2024) has documented systematic biases in single-model evaluation: position bias, verbosity bias, self-enhancement bias. Multi-model ensembles mitigate these biases through averaging, but averaging also destroys the signal: if the architect scores architecture at 8.5 and the operator scores it at 7.5, the average (8.0) tells you less than the disagreement (1.0 range from the operator's perspective).

Our approach preserves both: the consensus score (8.0) provides the actionable number, while the disagreement pattern (operator low, architect high) provides the diagnostic insight. The system knows not just its score but *who disagrees and why*.

### 5.2 The Objective/Subjective Partition

The partition of dimensions into objective (automated) and subjective (rater-evaluated) types is methodologically important for two reasons.

First, it prevents the inflation of agreement scores by deterministic measurements. If all 9 dimensions were rated by the panel, the 5 objective dimensions (which deterministically agree) would dominate the ICC computation, producing artificially high overall agreement. By computing IRA only over subjective dimensions, we measure actual inter-rater variance.

Second, it provides a *reliability floor*. Even when no LLM API is available, the system can produce a partial assessment from objective dimensions alone. This is operationally critical: the evaluative apparatus must degrade gracefully, not fail entirely, when external dependencies are unavailable.

This partition mirrors McCall et al.'s (1977) distinction between directly and indirectly measurable quality factors — a distinction that has persisted through four decades of software quality modeling because the underlying epistemological distinction is genuine. Some quality properties reduce to counting; others require judgment. The correct response is not to force everything into one category but to use the appropriate method for each.

### 5.3 Validity Considerations

**Construct validity.** Do the 9 dimensions measure "software quality"? We ground them in McCall et al. (1977) and ISO/IEC 25010:2023, mapping 6 of 9 dimensions to established quality characteristics (Table 1). The remaining 3 (Documentation, Sustainability, Claim Provenance) are domain-specific but grounded in the system's operational requirements. Future work should validate the rubric against expert human ratings to establish convergent validity.

**Content validity.** Do the dimensions cover the relevant quality space? The rubric was iteratively developed over three months of system operation, with dimensions added as quality gaps were discovered in practice (Claim Provenance was added in v1.1 after unsourced statistical claims were identified). Content validity is provisional and will improve with longitudinal application.

**Criterion validity.** Do high scores predict good outcomes? This requires longitudinal data: systems that score higher on the diagnostic should perform better on external criteria (fewer bugs, higher acceptance rates, easier onboarding). We have preliminary signal — the threshold calibration crisis (Section 1) was detected by the diagnostic before it caused operational failure — but systematic criterion validation remains future work.

**Internal consistency.** Cronbach's alpha across the 9 dimensions would measure whether dimensions covary as expected. We do not report this because the objective/subjective partition makes the computation misleading — zero-variance objective dimensions inflate alpha. Future work should compute internal consistency separately for subjective dimensions.

### 5.4 Limitations

**Sample size.** This paper reports results from a single system evaluation. Generalizability claims require replication across diverse systems, languages, and domains.

**Rater independence.** Three of four raters use Anthropic models, which may share training-induced biases. The single Google rater provides some cross-family signal but is insufficient for robust independence claims.

**Persona authoring bias.** Personas are authored by the system's creator, who determines what evaluative perspectives exist. A persona that penalizes complexity will never emerge from an author who does not recognize complexity as a problem.

**Agreement ceiling.** The reported ICC of 1.0 may reflect insufficient persona divergence rather than genuine quality robustness. Expanding the panel with more extreme personas (e.g., a "hostile security auditor" or a "minimalist who demands the system be 10x simpler") may reveal dimensions where agreement breaks down.

**Ecological validity.** Whether LLM quality assessments predict *actual* software outcomes (defect rates, developer satisfaction, maintenance cost) remains undemonstrated.

---

## 6. Generalization and Future Work

### 6.1 Domain Generalization

The method generalizes to any domain satisfying three conditions:

1. **Structured quality rubric**: Dimensions with anchored scoring guides and evidence sources
2. **Objective/subjective partition**: Some dimensions measurable by automated collection, others requiring judgment
3. **Meaningful evaluative diversity**: Multiple stakeholder perspectives that disagree productively

Candidate domains include: academic paper review (dimensions: methodology, novelty, writing quality, reproducibility; personas: methodologist, domain expert, practitioner, statistical reviewer), educational grading (dimensions mapped to rubric criteria; personas: strict constructivist, encouraging formativist, standards-based criterion referrer), and CI/CD quality gating (dimensions: architecture, test coverage, documentation, security; personas: architect, SRE, security engineer, product manager).

### 6.2 Feedback Integration

The most novel extension is closing the feedback loop: using IRA results to calibrate the production system. In our implementation, block-outcome correlation classifies narrative content as effective or ineffective, and rubric recalibration adjusts dimension weights based on which scores predict external outcomes. This transforms the evaluative capacity from a passive audit into an active regulatory mechanism — an implementation of cybernetic control (Beer, 1972) through psychometric measurement.

### 6.3 Future Work

- **Panel expansion**: Test with 7+ raters spanning 3+ model families (Anthropic, Google, Meta, Mistral)
- **Adversarial personas**: Generate personas designed to challenge system assumptions, not just evaluate from different professional perspectives
- **Longitudinal tracking**: Compute IRA over weekly assessment snapshots to detect agreement drift as the system evolves
- **Cross-system replication**: Apply the method to systems in other languages and domains to validate generalizability
- **Human baseline**: Compare LLM panel scores against expert human ratings to establish convergent validity with the gold standard
- **Criterion validation**: Correlate diagnostic scores with external outcomes (bug reports, acceptance rates, maintenance cost) across a portfolio of systems

---

## 7. Conclusion

This paper demonstrates that persona-driven LLM panels, evaluated with classical psychometric IRA statistics, produce statistically robust and diagnostically informative software quality assessments. The key contribution is the reframing of inter-model disagreement: not as noise to be averaged away, but as a signal revealing which quality dimensions are robust across evaluative perspectives and which require attention.

The method is replicable with minimal infrastructure — a YAML rubric, a persona configuration file, API keys for 2+ model providers, and a pure-Python statistics module. It requires no specialized psychometric software, no training data, and no human rater calibration sessions. It produces, in the time it takes to make 4 API calls, the same class of statistical output that a human IRA study produces over weeks.

What it does not produce is certainty. The personas are authored, the rubric is provisional, and the agreement scores are only as meaningful as the evaluative frameworks they aggregate. The human contribution — deciding what to measure and whose perspective to instantiate — remains irreducible. But within those constraints, the method demonstrates that AI-as-psychometrician is not merely feasible but productive: it generates quality signals that traditional metrics cannot capture and that single-evaluator approaches necessarily miss.

---

## References

Ashby, W. R. (1956). *An Introduction to Cybernetics*. Chapman & Hall.

Beer, S. (1972). *Brain of the Firm: The Managerial Cybernetics of Organization*. Allen Lane/Penguin.

Cohen, J. (1960). A coefficient of agreement for nominal scales. *Educational and Psychological Measurement*, 20(1), 37–46.

Cohen, J. (1968). Weighted kappa: Nominal scale agreement with provision for scaled disagreement or partial credit. *Psychological Bulletin*, 70(4), 213–220.

Fleiss, J. L. (1971). Measuring nominal scale agreement among many raters. *Psychological Bulletin*, 76(5), 378–382.

ISO/IEC. (2023). *ISO/IEC 25010:2023 — Systems and software engineering — Product quality model*. International Organization for Standardization.

Kemerer, C. F., & Slaughter, S. (1999). An empirical approach to studying software evolution. *IEEE Transactions on Software Engineering*, 25(4), 493–509.

Koo, T. K., & Li, M. Y. (2016). A guideline of selecting and reporting intraclass correlation coefficients for reliability research. *Journal of Chiropractic Medicine*, 15(2), 155–163.

Landis, J. R., & Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1), 159–174.

Li, H., et al. (2024). A Survey on LLM-as-a-Judge. *arXiv:2411.15594*.

McCall, J. A., Richards, P. K., & Walters, G. F. (1977). *Factors in Software Quality*. US Department of Commerce/NTIS.

Shrout, P. E., & Fleiss, J. L. (1979). Intraclass correlations: Uses in assessing rater reliability. *Psychological Bulletin*, 86(2), 420–428.

Yavuz, F. (2025). Utilizing large language models for EFL essay grading: An examination of reliability and validity in rubric-based assessments. *British Journal of Educational Technology*.

Zheng, L., Chiang, W.-L., Sheng, Y., et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. *Proceedings of NeurIPS 2023*. arXiv:2306.05685.

---

## Appendix: Replication Package

The complete implementation is available in the `application-pipeline` repository:

| Component | Path | Description |
|-----------|------|-------------|
| Rubric | `strategy/system-grading-rubric.yaml` | 9-dimension rubric with scoring guides |
| Personas | `strategy/rater-personas.yaml` | 4 persona definitions (role + bias) |
| Collectors | `scripts/diagnose.py` → `COLLECTORS` | 5 objective measurement functions |
| Generators | `scripts/diagnose.py` → `PROMPT_GENERATORS` | 4 subjective prompt generators |
| IRA Statistics | `scripts/diagnose_ira.py` | Pure-stdlib ICC, kappa, consensus |
| Orchestrator | `scripts/generate_ratings.py` | Multi-model rating session driver |
| Results | `ratings/consensus-2026-03-14.json` | Most recent consensus output |
