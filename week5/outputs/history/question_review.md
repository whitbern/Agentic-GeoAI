# Scientific critique of candidate research questions (review of v3)

Stage: 02 Scientific Critic
Packet: `outputs/prompt_packets/02_scientific_critic.md`
Date: 2026-10-04
Verdict: **REVISE** (rationale in section 7)

## 0. Independence disclosure

This review was written by the same model, in the same conversation, that drafted
`outputs/research_questions.md`. The reviewer therefore shares the author's blind spots and had seen
the author's reasoning before reviewing. Under AGENTS.md rule 4 this is **not independent
validation**. Where the review agrees with the draft, that agreement carries little weight. The
issues below should be judged on their arguments, and the packet should be rerun in a fresh
conversation (ideally a different model) and the two reviews compared.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` (embedded in packet) | Original problem, hypothesis, scope limits, required validity threats |
| `outputs/research_questions.md` v3 (read from disk; the pasted packet copy had this section empty) | Object of review |
| `outputs/history/research_questions_v1.md`, `_v2.md` | Not re-read; trajectory known from section 0 of v3 |
| `inputs/evidence/README.md` | Human claim-to-citation map, as relayed in v3 |

**Evidence status.** No paper full texts, no data, and no measurements were available. Nothing about
either study site has been measured. Reviewer statements about how graphs behave are labelled as
inference and are checkable on synthetic points before any real data is used.

## 2. Assumptions made by the reviewer

- The human decisions quoted in v3 section 0 are binding. Where the review finds a problem with
  them it recommends a decision to the human and does not alter the question.
- "Reach" means the fraction of non-seeded nodes flagged after propagation, as in v3.
- The course's expectation for how much "AI" a GeoAI project needs is unknown. **Missing evidence.**

## 3. Scores (1 = weak, 5 = strong)

| Dimension | RQ1 + RQ2 | RQ3 | RQ4 | RQ5 |
|---|---|---|---|---|
| Importance | 3 | 3 | 2 | 2 |
| Novelty / evidence gap | 2 | 2 | 2 | 1 |
| Testability | 3 | 3 | 4 | 3 |
| Identification strategy | 2 | 3 | 3 | 2 |
| Data adequacy | 3 | 3 | 3 | 3 |
| Spatial validity | 3 | 4 | 2 | 3 |
| Robustness / validation | 3 | 4 | 2 | 3 |
| Feasibility | 4 | 3 | 3 | 4 |

Notes on the scores:

- **Novelty is 2 everywhere because no gap has been shown**, not because the work is known to be
  derivative. No literature search has been done.
- **Identification for RQ1 is 2** because the only test that discriminates between layout types
  rests on one pair of sites (issue 4), and one co-primary test is close to unfalsifiable (issue 2).
- **Data adequacy is 3 pending measurement.** The human's Claim 4 has no number behind it yet.

## 4. Issues

### Issue 1. With a deterministic hop rule there is no experiment, only a graph statistic

- **LOCATION / TARGET.** RQ1 method; section 3; U2.
- **SEVERITY.** Blocking.
- **WHY IT MATTERS.** Section 3 of v3 shows that one-hop reach is arithmetic on degree. The same
  argument holds one level up. *Reviewer inference:* under "flag every node within `k` hops of a
  seed", a non-seeded node with `n` other nodes in its `k`-hop neighbourhood is flagged with
  probability `1 − (1 − p)^n`. Reach at every `p` is then fully determined by the distribution of
  `k`-hop neighbourhood sizes. Three consequences:
  1. Monte Carlo error injection adds nothing; the answer can be computed directly from the graph.
  2. Every reach curve runs from 0 to 1, so the site gap is zero at both ends and largest in the
     middle. "The slope of reach on `p` differs between sites" has no single answer; a linear
     site × `p` interaction would change sign depending on the `p` range chosen.
  3. Path redundancy, the mechanism behind H1-alt, cannot operate. It only matters when
     transmission along a link is uncertain.
- **EVIDENCE NEEDED.** A stated propagation rule. A short synthetic check confirming or refuting
  the closed form above.
- **RECOMMENDED ACTION.** Resolve U2 before the question is accepted. Either (a) keep the
  deterministic rule and restate RQ1 honestly as a comparison of `k`-hop neighbourhood sizes, with
  low-`p` amplification (which equals mean `k`-hop neighbourhood size) as the estimand; or (b) adopt
  a probabilistic rule with per-link transmission probability, which makes simulation necessary and
  lets clustering matter. Whichever is chosen, replace "slope differs" with a defined estimand that
  does not depend on the `p` range.

### Issue 2. The rewiring test (H1-R) is very likely to succeed regardless of the hypothesis

- **LOCATION / TARGET.** H1-R; outcome table rows 3 and 4; "what would refute it".
- **SEVERITY.** Major.
- **WHY IT MATTERS.** *Reviewer inference from geometry, not from supplied evidence.* In a
  proximity graph on a plane, the number of nodes within `k` hops grows roughly with `k²`, because
  hops cover ground. In a randomly rewired graph with the same degrees it grows roughly
  geometrically until it saturates. For any `k ≥ 2`, the rewired graph should show much larger
  reach. H1-R would then "differ" for any spatial point pattern at all, including a perfect grid.
  A test that every possible layout passes cannot support a claim about layout. It also means the
  refutation condition in v3 (rows 3 and 4) is practically unreachable, and the expected direction
  is that spatial arrangement *contains* error relative to the null, which is the opposite of the
  amplification wording in `inputs/problem.md`. v3 notes that the null "is no longer a spatial
  graph" but does not draw this consequence.
- **EVIDENCE NEEDED.** Reach on a regular lattice and on uniformly random points, each against its
  own rewired null, at matched degree. If both differ strongly from their nulls, the concern stands.
- **RECOMMENDED ACTION.** Keep H1-R (the human made it co-primary) but report it as an effect size,
  not a pass/fail test. Add a null that stays spatial: for example rewiring restricted to pairs
  within a fixed radius, or reference point patterns at the same intensity (regular lattice,
  uniformly random). Needs human approval because it changes what "co-primary" delivers.

### Issue 3. Decision 2 may control away the irregularity the hypothesis is about

- **LOCATION / TARGET.** Section 0 decision 2; A6; A7.
- **SEVERITY.** Major. A construct question for the human, not an error.
- **WHY IT MATTERS.** *Reviewer inference.* Matching mean degree with a per-site threshold removes
  uniform density exactly: shrinking a point pattern and shrinking `d` by the same factor gives the
  identical graph. That part of A7 is sound. But an irregular settlement differs from a planned
  grid largely in how unevenly buildings are packed, and uneven packing shows up in a distance
  graph as uneven degree. Decision 2 classes degree heterogeneity as density and controls it. What
  remains as "arrangement" is clustering and connection pattern at fixed degrees, which is narrower
  than the "irregularity" named in decision 1. A null result under decision 2 could then be
  misread as "irregularity does not matter".
- **EVIDENCE NEEDED.** Degree distributions at both sites at matched mean degree. If they are
  similar, the concern is moot.
- **RECOMMENDED ACTION.** Keep decision 2, and report a three-step decomposition so nothing is
  hidden: the unadjusted site gap, the gap after mean-degree matching, and the gap after
  full-degree-sequence control. State in the write-up that local packing unevenness is counted as
  density by definition.

### Issue 4. The only discriminating test has a sample of one pair

- **LOCATION / TARGET.** H1-X; cross-site contrast of arrangement effects; A5; U3.
- **SEVERITY.** Major.
- **WHY IT MATTERS.** Given issue 2, the claim about irregular versus planned layouts rests
  entirely on comparing two places. They differ in mapping method, what counts as a "building",
  topography, plot size, and era, as well as arrangement. v3 acknowledges this but offers no
  design remedy. No number of simulation replicates changes it.
- **EVIDENCE NEEDED.** Variation in arrangement that is not tied to site identity.
- **RECOMMENDED ACTION.** Two options that fit the tools already listed:
  1. *Within-site variation.* Cut each site into sub-windows, measure arrangement in each, and
     relate it to the arrangement effect. This gives a within-site dose-response that site identity
     cannot explain. Sub-windows are spatially dependent, so use spatial blocks, not random splits.
  2. *Synthetic patterns.* Generate point patterns at fixed intensity along a regular-to-irregular
     gradient and run the same experiment. Arrangement is then manipulated directly, and the two
     real sites are placed on the gradient as cases.

  Whether option 2 breaches the "two study areas" scope limit is for the human.

### Issue 5. "Irregular" and "regular" are never measured

- **LOCATION / TARGET.** RQ1 research question; Candidate study areas in `inputs/problem.md`.
- **SEVERITY.** Major.
- **WHY IT MATTERS.** The sites are labelled dense-irregular and sparse-regular by description.
  The human's supporting citation (Boeing 2019, as summarised) concerns street orientation, not
  building point patterns. Without a measured descriptor, a site difference cannot be attributed
  to irregularity even in principle.
- **EVIDENCE NEEDED.** At least one arrangement metric on building footprints per site and per
  sub-window, for example variation in nearest-neighbour distance, a pair-correlation or Ripley's K
  summary, and clustering coefficient at matched degree.
- **RECOMMENDED ACTION.** Name the arrangement metrics in the research question and compute them
  in the descriptive pull before any propagation run.

### Issue 6. The cross-site contrast depends on the scale it is computed on

- **LOCATION / TARGET.** "Arrangement effect" (observed minus rewired); U1.
- **SEVERITY.** Major.
- **WHY IT MATTERS.** The reviewer agrees with v3's reading that H1-X does not control what
  decision 2 calls density, and that a contrast of arrangement effects is the cleaner statistic.
  But the two sites' rewired baselines will differ, because their degree sequences differ. A
  difference of differences and a ratio of ratios can then disagree in sign. Reach is also bounded
  at 1, which compresses differences at high `p`.
- **EVIDENCE NEEDED.** None; this is a specification choice.
- **RECOMMENDED ACTION.** Fix the scale in advance. A ratio of effective neighbourhood sizes
  (observed over rewired) is one bounded-free choice. Ask the human whether the contrast should
  replace H1-X as the second co-primary test, with H1-X kept as the middle step of the
  decomposition in issue 3.

### Issue 7. Simulation replicates are not a sample of places

- **LOCATION / TARGET.** RQ1 method ("fit reach on `p`, site, graph condition"); U5.
- **SEVERITY.** Major.
- **WHY IT MATTERS.** Replicates measure Monte Carlo noise for two fixed graphs. A regression over
  replicates will report tiny standard errors for the site term, which say nothing about whether
  another settlement or another suburb would behave the same. U5 raises effect sizes but still
  frames the comparison as "more than replicate variation", which is the wrong yardstick for a
  claim about layout.
- **EVIDENCE NEEDED.** A measure of spatial variability within each site.
- **RECOMMENDED ACTION.** Report no significance test for the site term. Give Monte Carlo
  intervals for precision, plus spread across spatial blocks within each site as the honest
  measure of variability. Set the minimum effect size of interest before running.

### Issue 8. Nodes may not be comparable units across sites

- **LOCATION / TARGET.** A1; RQ5.
- **SEVERITY.** Major, pending a data check.
- **WHY IT MATTERS.** *Not yet measured.* A mapped polygon in an informal settlement may be a
  single room or a merged row of dwellings; a suburban parcel may contribute a house plus
  outbuildings. If so, degree means different things at each site, and degree matching equalises a
  number without equalising what it represents. This is the zoning form of MAUP at node level.
- **EVIDENCE NEEDED.** Footprint area distributions and a visual check of a sample at each site;
  the OSM versus Google/Microsoft comparison from RQ5.
- **RECOMMENDED ACTION.** Promote RQ5 from optional to a required pre-analysis gate. State a node
  definition (for example a minimum footprint area, or merging touching polygons) and test
  sensitivity to it.

### Issue 9. Edge handling is under-specified and the proposed buffer is site-dependent

- **LOCATION / TARGET.** RQ2 threats.
- **SEVERITY.** Minor to major depending on site extents (not measured).
- **WHY IT MATTERS.** v3 proposes analysing a core inside a buffer of `d` × hop limit. Under degree
  matching `d` differs by site, so the buffer does too, and a small site may have little core left.
  The study boundary is also administrative; buildings continue beyond it.
- **EVIDENCE NEEDED.** Site extents and the matched thresholds.
- **RECOMMENDED ACTION.** Download buildings for the study area plus the buffer, let buffer nodes
  carry propagation, and compute reach only on core nodes. Report the share of nodes in the core.

### Issue 10. RQ1 + RQ2 contains no learning component and skips a required validity threat

- **LOCATION / TARGET.** Section 5 "GeoAI relevance"; recommendation.
- **SEVERITY.** Major for fit to the brief; a judgement the human asked for.
- **WHY IT MATTERS.** The broad problem asks how GeoAI can help. RQ1 + RQ2 is network analysis and
  simulation. `inputs/problem.md` also requires the workflow to address spatial leakage between
  training and test data, which only arises in RQ3. Whether this is acceptable depends on course
  expectations the reviewer cannot see.
- **EVIDENCE NEEDED.** The assignment rubric or instructor guidance.
- **RECOMMENDED ACTION.** Fold a scoped-down RQ3 into the design: predict the per-sub-window
  arrangement effect from the metrics in issue 5, with spatially blocked cross-validation. This
  supplies the learning component, addresses leakage, and is the within-site analysis issue 4
  calls for. It stretches "one primary modelling method"; the human should decide.

### Issue 11. No evidence gap has been established

- **LOCATION / TARGET.** Section 5 "Novelty"; U9.
- **SEVERITY.** Minor now; major before any abstract is written.
- **WHY IT MATTERS.** v3 correctly claims no novelty. But how clustering and spatial embedding
  affect spreading on networks is, to the reviewer's general knowledge, a long-studied topic; the
  human's own Claim 1 citations are in that area. *This is background knowledge, not verified
  against the supplied papers.* If issue 2 holds, H1-R may restate a known result. The possible
  contribution lies in the urban-morphology application and the cross-site contrast.
- **EVIDENCE NEEDED.** Full-text reading of the Claim 1 papers and a targeted search on spreading
  in spatial or geometric graphs and on error propagation in spatial association models.
- **RECOMMENDED ACTION.** Do the search before the abstract stage. The Writer must not use
  novelty language without it and without human approval (AGENTS.md rule 9).

## 5. Required checks

| Check | Assessment |
|---|---|
| What evidence would falsify the claim? | For H1-R, realistically none (issue 2). For the site-type claim, a near-zero cross-site contrast of arrangement effects on a pre-set scale. That contrast is the real test and should be labelled as such. |
| Is the spatial scale aligned with the question? | Partly. Per-site thresholds are a sound response to different building spacing, and reporting them is appropriate. Node comparability (issue 8) and hop depth relative to site extent are unverified. Temporal alignment between OSM snapshot and comparison footprints is unverified. |
| Could spatial autocorrelation or leakage inflate performance? | No predictive performance exists in RQ1 + RQ2, so leakage does not apply there. Autocorrelation would inflate apparent precision if replicates or sub-windows are treated as independent (issue 7). RQ3 handles leakage appropriately in outline. |
| Are causal claims justified? | Within the model, for `p` and for rewiring, yes. For "irregular layout causes more or less spread", no, with two sites. Synthetic patterns (issue 4) would give a causal statement about the model only. Human approval is still required for causal wording. |
| Does the evidence discriminate among alternatives? | Not yet. A site difference could come from arrangement, residual density, node definition, mapping completeness, or the propagation rule. Issues 3, 4, 5, and 8 each remove one alternative. |
| Is a second dataset or benchmark available? | Google Open Buildings and Microsoft footprints as alternative inputs; synthetic lattices and random patterns as benchmarks with known structure. No benchmark exists for the propagation rule itself. |
| Is the question answerable with the proposed data? | A question about these two graphs under a stated rule is answerable. A question about urban layout types is not, with this design alone. |

## 6. Recommendation on the strongest question

**RQ1 remains the strongest candidate**, with RQ2 as its sensitivity analysis, and it should not
proceed to the abstract in its v3 form. RQ4 should be dropped for scope. RQ5 should become a
pre-analysis gate. A reduced RQ3 is worth folding in (issue 10).

Required before acceptance:

1. Fix the propagation rule and define the moderation estimand (issue 1).
2. Add a spatial null or reference patterns so that at least one primary test can fail (issue 2).
3. Name and measure arrangement metrics (issue 5).
4. Fix the scale of the cross-site contrast (issue 6).

Strongly advised: within-site sub-window analysis or synthetic gradient (issue 4), the three-step
decomposition (issue 3), and the node-definition gate (issue 8).

## 7. Verdict

**REVISE.** The question is well motivated, scoped to the tools available, and unusually candid
about its assumptions, and the human's decision to treat density as a confound makes it sharper.
But as written it cannot yet fail in an informative way. If propagation is a deterministic hop
rule, reach is a fixed function of neighbourhood sizes and the stated estimand, a difference in
slopes, is undefined. The within-site rewiring test is expected to succeed for any spatial layout,
so it cannot bear on irregular versus planned form. That leaves a single two-site contrast, with
irregularity unmeasured and node units possibly not comparable, to carry the central claim. Each of
these has a remedy that fits the existing toolset and semester scope. Several remedies touch
decisions the human has already made, so they are recommendations for approval and not changes to
the question. This verdict comes from a non-independent reviewer (section 0) and the geometric
argument in issue 2 is unverified inference.

## 8. Unresolved issues for the human

- **D1.** Propagation rule: deterministic `k`-hop or per-link probabilistic? (Issue 1)
- **D2.** Add a spatial null or reference patterns alongside H1-R? (Issue 2)
- **D3.** Should the cross-site contrast of arrangement effects replace H1-X as co-primary, and on
  what scale? (Issue 6)
- **D4.** Are synthetic point patterns within the "two study areas" scope? (Issue 4)
- **D5.** Fold a reduced RQ3 into the design despite "one primary modelling method"? (Issue 10)
- **D6.** Accept that local packing unevenness is counted as density, with the decomposition
  reported? (Issue 3)

## 9. Suggested next scientific action

1. Run a synthetic check that needs no real data: a regular lattice and a uniformly random point
   pattern, distance graphs at matched mean degree, each against its rewired null, with a `k`-hop
   rule. This tests the reviewer's claims in issues 1 and 2 directly and takes little code.
2. Run the descriptive pull proposed in v3, extended with footprint-area distributions, degree
   distributions at matched mean degree, and the arrangement metrics from issue 5.
3. Have the human rule on D1 to D6, then revise `outputs/research_questions.md` to v4.
4. Rerun this critic packet in a fresh conversation for an independent review.
