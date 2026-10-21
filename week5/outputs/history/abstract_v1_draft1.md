# Proposed-study abstract (v1)

Stage: 03 Scientific Writer Agent
Packet: `outputs/prompt_packets/03_writer_abstract.md`
Date: 2026-10-04
Source question: `outputs/research_questions.md` (v8, frozen by human decision 7)

Nothing in this document is a finding. No data has been collected and no code has been run.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` | Broad problem, boundary conditions, required validity threats |
| `outputs/research_questions.md` v8 | Question (4.1), estimands (4.2), hypotheses (4.3), decision rule (4.4), contribution wording (4.6), design (section 5), threats (section 6) |
| `outputs/question_review.md` | The independent review of v7 (verdict REVISE). Its resolutions are taken from v8 section 9.1 |
| `outputs/evidence_notes.md` | The evidence base. Every citation in the abstract is limited to what this file records the paper as saying, on the pages it lists |
| Human instruction for this stage | No novelty wording; describe the contribution as an application of established spatial-network methods |

The evidence PDFs were not opened at this stage. Citations rest on the notes, not on a fresh
reading.

## 2. Assumptions

- **W1.** The abstract describes the frozen v8 design. Where v8 leaves an operating value to the
  pilot, the abstract names the component and gives no value.
- **W2.** Following v8 M15, the abstract says "injected label errors under an independent-cascade
  rule" where the frozen question in 4.1 says "injected classification errors ... under
  probabilistic propagation". The two describe the same rule (A3). The question itself is not
  reworded in v8.
- **W3.** The fallback learning component is described, because decision 7 makes it the expected
  path. The sub-window form is left out for length.
- **W4.** The suburb is not named, because none has been selected or approved.
- **W5.** The word limit is 200–300 words. The abstract body is 299 words including the bold
  labels.

## 3. Abstract

<!-- abstract:start -->

**Background.** Association-based scoring can carry erroneous seed labels through a network: one
research system, tested on synthetic data, degraded sharply when most initial labels were wrong
(Macskassy & Provost 2005). Where links represent spatial proximity, building layout may shape
how far such errors reach. In model networks with the same degree distribution, spatial
structure reduces onward transmission through short loops (Ghadiri et al. 2026, preprint).

**Question.** With density held fixed, does the arrangement of connections in building-level
proximity graphs change how far injected label errors spread under an independent-cascade rule,
and does that effect differ between Kibera and a planned US suburb?

**Proposed data and methods.** Distance-band graphs will be built on building-footprint
centroids at both sites, matched on building count and mean degree, and on synthetic point
patterns from regular lattice through uniform random to clustered. Each graph is
compared with degree-preserving rewirings of itself. The sole primary estimand is the cross-site
contrast in the observed-to-rewired ratio of critical transmission probability, judged on a
block-bootstrap interval against a pre-specified ratio margin of 1.25, a convention.
The direction is not assumed, and an inconclusive result is an accepted outcome. Sub-threshold
error amplification is reported descriptively.

**Validation.** The threshold estimator must first recover the square-lattice bond percolation
threshold of 1/2 (Barthélemy 2011). A pre-analysis gate compares footprint sources at each site,
spot-checks sampled buildings, and maps completeness within Kibera. Sensitivity runs vary node
definition, target degree, and edge rule. A linear model trained on the synthetic patterns
predicts each site's ratio; this does not exercise spatial-leakage control.

**Expected contribution.** The study applies established spatial-network methods,
degree-preserving rewiring (Iotti et al. 2017) and percolation-threshold estimation on building
locations (Behnisch et al. 2019), to two cases. The cascade is a stylised abstraction, not a
published scoring method. No data have been collected.

<!-- abstract:end -->

## 4. Traceability of claims

**Evidence** (what the notes record a paper as saying)

| Statement in the abstract | Source, as recorded in `outputs/evidence_notes.md` | Limit recorded there |
|---|---|---|
| One association-scoring system degraded sharply when most initial labels were wrong; tested on synthetic data | Macskassy & Provost (2005), pp. 3–4; section 10 | People linked by communications and co-presence, not buildings. Non-spatial network. The rule averages over neighbours and is not the cascade used here |
| With the same degree distribution, spatial structure reduces onward transmission through short loops | Ghadiri et al. (2026), pp. 3–6; section 12 | Preprint. Random geometric against Erdős–Rényi graphs, uniform random points. Shown for a reproduction number, not a threshold |
| The square-lattice bond percolation threshold is 1/2 | Barthélemy (2011), pp. 85–86; section 2 | None for this value |
| Degree-preserving rewiring of a proximity graph is an established comparison | Iotti et al. (2017), p. 3; section 7 | SIS model, not a one-shot cascade. Random geometric graphs |
| Percolation-threshold estimation on building locations is established | Behnisch et al. (2019), pp. 2, 4; section 8 | Threshold is in clustering distance over a whole country. Its use for transmission probability is an agent inference in v8 |

**Inference and design statements** (from v8, not from any paper)

| Statement in the abstract | Basis |
|---|---|
| "building layout may shape how far such errors reach" | The motivation in `inputs/problem.md`. An untested expectation, worded as a possibility |
| The question | v8 section 4.1, with the M15 wording (W2) |
| Sites matched on building count and mean degree; three-segment synthetic gradient | Decision 7; v8 A11, 5.2, 5.4 |
| Sole primary estimand, block-bootstrap interval, margin of 1.25 as a convention | Decision 7; v8 4.2, 4.4 |
| "The direction is not assumed" | v8 section 2, "mixed or unsettled": the supplied lattice and SIS results point different ways |
| Amplification is descriptive | Decision 7; v8 4.2 |
| Gate, spot check, within-site completeness, sensitivity runs | v8 5.2, 5.6 |
| Linear model on synthetic patterns; no leakage control exercised | v8 5.7, fallback form; decision 5 |
| "applies established spatial-network methods ... to two cases" | v8 4.6 and A6 |
| "a stylised abstraction, not a published scoring method" | v8 A4 |

**Claims deliberately not made**

- No novelty or evidence-gap claim. No literature search has been done (v8 M14).
- No causal claim about layout type. Two sites are two cases (v8 A6).
- No claim about any deployed system, policy relevance, or external validity (AGENTS.md rule 9).
- No statement that the two sites differ in building arrangement, or that either site's data is
  adequate. v8 section 2 lists both as unsupported.
- No expected sign for the primary contrast.

## 5. Unresolved issues

- **X1. Compression of the Ghadiri et al. result.** "In model networks" stands in for three
  limits in the notes: uniform random points only, a reproduction number and not a threshold,
  and a preprint. A reader of the abstract alone could take the sentence as support for the
  study's threshold estimand. The notes say it is not.
- **X2. Compression of A4.** The abstract opens with Macskassy & Provost and closes by saying the
  cascade is not a published method. It does not say that the cited system's averaging rule may
  respond to degree in the opposite direction. That point is in v8 A4 and section 12.
- **X3. Threats omitted for length.** The abstract does not mention that the block bootstrap of
  a critical value is undefined in detail (v8 M1), that arrangement as defined includes the
  clumping of dense patches (A5), that the two footprint sources at the suburb may share an
  origin (M10), or that the fallback model has two test cases (M12). The methodology stage
  should not read their absence as resolution.
- **X4. Required validity threats.** `inputs/problem.md` lists four. The abstract names
  spatial-leakage control (described, not demonstrated) and covers MAUP only as "sensitivity
  runs". Edge effects and spatial dependence are handled in v8 5.8 and are not in the abstract.
- **X5. Emphasis.** The independent review recommended leading with the synthetic gradient. No
  ruling was made (v8 M16), so the abstract leads with the cross-site contrast, which is the
  frozen primary estimand.
- **X6. Citations were not re-verified.** They are as reliable as `outputs/evidence_notes.md`,
  which records that several papers were read only in part.
- **X7. Same-model authorship.** This abstract, v8, and the review come from the same model
  family. Their agreement is not independent validation (AGENTS.md rule 4).

## 6. Suggested next scientific action

1. **Human check of the abstract** against X1 to X5, in particular whether the Ghadiri et al.
   sentence should carry its limits or be cut.
2. **Regenerate packet 04** so it embeds v8 and this file, and give it
   `outputs/evidence_notes.md` by hand (v8 section 11, item 6).
3. **Validate the critical-value estimator on the square lattice** (v8 5.1, step A). It needs no
   data and gates every other analysis.
4. **Literature search** before any later version of the abstract goes beyond "application".
