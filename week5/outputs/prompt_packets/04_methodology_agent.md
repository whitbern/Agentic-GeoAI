# AGENT ROLE

# 🧭 Methodology Agent

## Role
You are a GeoAI methodologist.

## Goal
Design a methodology that can actually test the approved research question/hypothesis.

## Required components
1. unit of analysis;
2. study area and temporal scope;
3. dependent/target variables;
4. explanatory/predictor variables;
5. data provenance and resolution;
6. preprocessing;
7. model/algorithm;
8. baseline/comparator;
9. spatial train/test or cross-validation strategy;
10. uncertainty/sensitivity analysis;
11. external validation;
12. stopping/decision criteria.

## GeoAI-specific checks
Explicitly consider:
- CRS and units;
- spatial autocorrelation;
- spatial/temporal leakage;
- scale/resolution mismatch;
- MAUP/ecological inference where relevant;
- spatial sampling bias;
- transferability beyond the study area.

## Output
Write `methodology_v1.md` plus a short list of assumptions most likely to change the conclusion.


# CURRENT TASK

Design a GeoAI methodology capable of testing the approved hypothesis.

# CURRENT RESEARCH STATE / EVIDENCE

## FILE: inputs/problem.md

# 🌍 Broad research problem

How can GeoAI help us understand how urban spatial structure affects the spread of
classification errors in proximity-based spatial association networks?

## Initial boundary conditions

- The problem must contain a meaningful spatial component.
- The investigation should be achievable with public or research-accessible data.
- The aim is to produce a testable research question, not merely a descriptive mapping exercise.
- The methodology must contain an explicit validation strategy.
- All "individuals" in the model are synthetic agents or building-level nodes. No real persons,
  identities, or individual-level records are used or inferred.
- No real targeting, strike, or casualty data is used. The study measures model reliability
  under controlled synthetic error, not real-world outcomes.
- Scope is a single-semester class project: two study areas, one primary modeling method,
  one controlled error-injection experiment.

## Human notes

### Domain knowledge
- Pattern-of-life and association-based decision support tools infer relationships partly from
  spatial proximity (who lives near whom, who shares a building or block). A classification error
  on one node can propagate to spatially associated nodes.
- Hypothesis: the same error rate produces different propagation patterns depending on urban
  spatial structure. Dense, irregular, highly connected layouts are expected to amplify and spread
  errors. Sparse, regular grids are expected to contain them. The direction and size of the
  effect are the empirical question.
- Statistically this is a moderation question: spatial structure moderates the relationship
  between injected error rate and error reach.
- Known spatial threats to validity that the workflow must address explicitly:
  - IID violations / spatial autocorrelation
  - Spatial leakage between training and test data
  - Edge effects at study-area boundaries
  - MAUP (sensitivity to proximity thresholds and aggregation units)


### Candidate study areas
- Dense, irregular: Kibera, Nairobi. Among the best-mapped informal settlements in OSM
  (community-mapped through the Map Kibera project). Cross-check building completeness
  against Google Open Buildings and document the OSM snapshot date.
- Sparse, regular baseline: a US planned suburban grid (e.g., a Phoenix or Las Vegas suburb).


### Constraints
- Python only. Libraries: OSMnx, GeoPandas, Shapely, NetworkX, PySAL (libpysal, esda, spreg).
  ArcGIS Pro is available for visualization and checks.
- First time using OSMnx and NetworkX together, so the workflow should be well commented
  and modular.
- Prefer explainable methods (spatial autoregressive models, network metrics, tree-based
  models) over deep learning. A GNN is optional and only if time allows.
- Fully reproducible: fixed random seeds, pinned package versions, a dated data snapshot,
  and a version-controlled GitHub repository.


### Datasets I already know
- OpenStreetMap building footprints and street networks (via OSMnx; historical via ohsome)
- Microsoft Global ML Building Footprints / Google Open Buildings (cross-check OSM completeness)
- WorldPop or GHSL population grids (optional, to weight nodes by population)

### Papers to treat as evidence
- Boeing (2017), OSMnx — methods for acquiring and analyzing street networks
- Zhang, Song, Luo & Wu (2023), "Geocomplexity explains spatial errors," IJGIS
- Janowicz et al. (2020), "GeoAI: spatially explicit artificial intelligence techniques…," IJGIS
- Li et al. (2024), "GeoAI for Science and the Science of GeoAI," JOSIS
- Roberts et al. (2017), cross-validation strategies for data with spatial structure, Ecography
- Anselin (1995), Local Indicators of Spatial Association (LISA)
- Openshaw (1984), The Modifiable Areal Unit Problem
- Bhavnani et al. (2014), "Group Segregation and Urban Violence," AJPS (spatial ABM precedent)
- Weidmann & Salehyan (2013), "Violence and Ethnic Segregation," ISQ (GIS-coded ABM precedent)
- Weber (2016), "Keep adding," Environment and Planning D (databases and targeting)
- Suchman (2020), "Algorithmic warfare and the reinvention of accuracy," Critical Studies on Security



## FILE: outputs/research_questions.md

# Research question and hypotheses (v8, frozen)

Stage: 01 Research Question Agent (revision after the independent review)
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous versions: `outputs/history/research_questions_v1.md` to `_v7.md`
Status: **frozen** by human decision 7. No rulings are requested in this document. Remaining
concerns are listed for the methodology stage in section 10.
Responds to: `outputs/scientific_critic_independent.md` (verdict REVISE)

Nothing in this document is a finding. No data has been collected and no code has been run.

## 0. Human decisions recorded

**Decision 1 (after v1): density is a confound.**

> Density is a confound, not part of spatial structure. The central hypothesis is that spatial
> arrangement (irregularity, clustering, connectivity pattern) affects error spread independently of
> density. Use the degree-matched control as the primary test. Report the unadjusted
> density-inclusive comparison as a secondary result.

**Decision 2 (after v2): strictness of the density control; thresholds.**

> U1 decision: make degree-preserving rewiring a co-primary test alongside cross-site mean-degree
> matching. Degree distribution (including heterogeneity) is treated as part of density and is
> controlled; "arrangement" means the pattern of who connects to whom given fixed degrees. Use
> k-nearest-neighbour graphs as a robustness check, not a primary test.
>
> Threshold issue: accepted as expected. Report the distance threshold needed at each site to reach
> the target mean degree as a descriptive result; differences in what "proximity" means across sites
> are substantively relevant, not a limitation to minimize.

**Decision 3 (after the review): rulings on D1 to D6.**

> D1: Probabilistic propagation with per-link transmission probability t. Primary estimand:
> amplification factor (expected number of non-seeded nodes flagged per seeded error) at
> pre-specified values of t and p, reported with Monte Carlo intervals.
> D2: Yes. Add spatial reference patterns at matched intensity and mean degree (regular lattice,
> uniform random points). Report H1-R as an effect size, not pass/fail.
> D3: Yes. The cross-site contrast of arrangement effects becomes co-primary, on the ratio scale
> (observed/rewired effective neighbourhood size). H1-X is kept as the middle step of the
> decomposition.
> D4: Yes. Synthetic patterns are benchmarks, not study areas, and are within scope. Use a
> regular-to-irregular gradient and place both real sites on it.
> D5: Yes. Fold in a reduced RQ3: predict per-sub-window arrangement effects from arrangement
> metrics using spatially blocked cross-validation. This is the learning component.
> D6: Accept. Report the three-step decomposition (unadjusted, mean-degree matched, full
> degree-sequence controlled) and state that local packing unevenness is counted as density by
> definition.
> Also: promote RQ5 / node definition to a required pre-analysis gate (issue 8). Drop RQ4.

**Decision 4 (after v4): thresholds, hop limit, margin, suburb, scope.**

> Threshold: locate critical t for each graph on synthetic patterns first. Primary analysis uses t
> values below the lowest critical t of all compared graphs. Report each graph's estimated critical
> t as a secondary, size-robust arrangement measure.
> Hop limit: none in the primary analysis. Run a 2-hop-limited version as a sensitivity analysis.
> Minimum contrast of interest: ratio of 1.25 in amplification factor, fixed now.
> Accept all three stated readings of D1–D6. Select the suburb using measured neighbourhood
> indicators (low dead-end share, high orientation order).
> Scope: the sub-window model is conditional. Run it only if the measured site extents give enough
> spatial blocks; otherwise state it as future work. Do not expand scope further.

**Decision 5 (after v5): co-primary estimand, sub-window condition, suburb rule, fallback.**

> Pre-specified now, not after the pilot: estimated critical t is co-primary with the cross-site
> arrangement-effect contrast. Rationale: the low-t ratio is forced toward 1 by construction, and
> choosing the main estimand after seeing pilot results would be post hoc.
> Sub-window model runs only with at least 20 spatial blocks per site, each wider than the typical
> cascade extent, allowing 5-fold blocked cross-validation.
> Suburb selection: 5 candidate neighbourhoods of comparable extent to the Kibera study area; rank
> by orientation order (descending) and dead-end share (ascending); select the lowest summed rank.
> Fallback learning component if the sub-window model cannot run: train on the synthetic gradient
> to predict critical t from arrangement metrics; test on the two real sites as out-of-sample
> cases. State that this does not exercise spatial-leakage control.
> I have revised the README summary of the suspicion-scoring paper. Record the
> averaging-versus-cascade difference as future work only.
> Accept your three readings of my earlier rulings. Revise to v6; preserve v5.

**Decision 6 (after v6): final rulings before the pilot; freeze.**

> Critical-t contrast margin: 1.25 / 0.80 on the cross-site ratio of observed-to-rewired critical-t
> ratios, matching the amplification margin.
> Accept the decision table in section 4.4 as proposed.
> Critical-t uncertainty: use many realizations and bootstrap over spatial blocks; report
> intervals. An inconclusive result under the table is an acceptable outcome.
> Fallback target: observed-to-rewired critical-t ratio, not raw critical t. Real-site predictions
> count as in-sample only if their arrangement metrics fall within the synthetic gradient's range;
> otherwise report them as extrapolation.
> Sub-window model: use linear or spatial regression, not a tree-based model, given n = 40.
> Suburb candidates: nominate 5 planned suburban neighbourhoods in one metro, of comparable extent
> to the Kibera study area, with a one-line justification each, for my approval before any
> indicators are computed. Break ties by orientation order.
> Data gate: if OSM building counts differ from Microsoft or Google footprints by more than 30% at
> a site, switch that site to the better-matching source. Stop only if no source passes a visual
> spot check of 50 randomly sampled buildings.
> Accept your three readings. Revise to v7 and preserve v6. After v7, the research question is
> frozen pending the independent review; do not open new rulings.

**Decision 7 (after the independent review of v7): rulings on the review; final freeze.**

> Match the suburb to Kibera on building count (within ±10%), not ground area. Synthetic patterns
> are generated at the same node count.
> Section 4.4 uses the block-bootstrap interval. Monte Carlo replicates are run until MC error is
> negligible relative to it and reported as a precision check only.
> Estimated critical t (observed-to-rewired ratio, cross-site) is the sole primary estimand.
> Amplification is secondary and descriptive and does not count as corroboration.
> State explicitly that the arrangement effect includes spatial concentration of high-degree
> nodes; report degree assortativity as a diagnostic. No additional test.
> Estimate critical t on the largest connected component and report its share of nodes. Validate
> the estimator on a square lattice, where the bond percolation threshold is exactly 0.5; it must
> recover this within a stated tolerance before use on any other graph.
> Footprint gate: OSM, Microsoft, and Google at Kibera; OSM and Microsoft at the suburb; visual
> spot check of 50 buildings at both. Check completeness within the Kibera site, not only in
> total (Yeboah et al. 2021).
> Extend the synthetic gradient: regular lattice → uniform random → clustered (Thomas or Matérn
> cluster process).
> Edge handling: rewire the full graph including the buffer; compute all outcomes on core nodes
> only. Same rule for every graph.
> The fallback learning component is the expected path.
> No novelty wording; describe the contribution as an application.
>
> After v8 the research question is frozen. Do not open new rulings; list any remaining concerns
> for the methodology stage instead.

Standing effects of these decisions:

- **The research question, the primary estimand, the hypotheses, the margin, and the decision
  rule are frozen** as of v8. The question in section 4.1 is worded exactly as in v7.
- Decision 3 supersedes decision 2 on one point: the mean-degree-matched comparison is the middle
  step of the decomposition, not a co-primary test.
- **Decision 7 supersedes decision 5 on two points.** The critical-value contrast is the sole
  primary estimand, not co-primary. The suburb is matched to Kibera on building count, not on
  extent.
- **Decision 7 supersedes decision 4 on one point.** Amplification is no longer a primary
  estimand. The restriction to sub-critical `t` still applies wherever amplification is reported.
- **Decision 7 supersedes decision 6 on two points.** The two-contrast decision table is replaced
  by a rule on one contrast (4.4). The suburb gate uses two footprint sources, not three.
- **Decision 7 qualifies decision D6 on one point.** The degree sequence is still density and is
  still controlled. Where the high-degree nodes sit, and that they are linked to each other, is
  part of the arrangement effect (A5).
- Decisions 4, 5 and 6 confirm the agent's readings recorded in v4, v5 and v6, except where
  decision 7 supersedes them.
- **Scope is frozen** (decision 4). Decision 7 adds a clustered end to the synthetic gradient and
  one diagnostic; it adds no test.
- Points that decision 7 leaves operationally open are recorded as stated agent readings (A11),
  as values the pilot fixes, or as concerns for the methodology stage (section 10). None is a
  request for a ruling.

Changes in v8: decision 7 applied; sole primary estimand and one-contrast rule (4.2 to 4.4);
node-count matching (A11, 5.2); arrangement redefined to include spatial concentration of
high-degree nodes, with an assortativity diagnostic (A5, 5.5); largest-component rule and
estimator validation (5.1, 5.3); footprint gate by site and within-site completeness (5.2);
gradient extended to clustered patterns (5.4); edge rule (5.8); fallback as the expected path
(5.7); contribution statement (4.6); eight new papers (section 2); response to the independent
review (section 9); section 10 rewritten.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` | Broad problem, scope limits, required validity threats |
| `inputs/evidence/README.md` (revised by the human) | Claim-to-citation map |
| `inputs/evidence/*.pdf` (nineteen papers; several read only in part) | See `outputs/evidence_notes.md` for pages read, what each says, and checks on the README summaries |
| `outputs/scientific_critic_independent.md` | Issues 1 to 12 and the REVISE verdict |
| `outputs/history/question_review.md` | The earlier, non-independent review (issues 1 to 11) |
| `outputs/history/research_questions_v7.md` | The version revised here |
| Human decisions 1 to 7 | Quoted above |

**Limits on the review.** The independent review worked from the packet only and did not read
the papers. It is written by a model of the same family as this document's author. Under
AGENTS.md rule 4, agreement between the two is not independent scientific validation. Several of
the review's technical statements were marked as unverified background; their status is tabulated
in `outputs/evidence_notes.md` section 23.

## 2. What the evidence supports

Page references are in `outputs/evidence_notes.md`. None of the papers studies label-error
propagation on building-proximity graphs, so every point applies here by analogy.

**Supported**

- **Building-centroid proximity graphs with a distance threshold have one precedent**, including
  a minimum-area filter and a check of sensitivity to it (Behnisch et al. 2019). Three GeoAI
  papers also use building centroids as nodes, with k-nearest-neighbour or Delaunay edges (Lei et
  al. 2024; Jia et al. 2024; Liu & Song 2025).
- **Comparing a proximity graph with a degree-preserving rewired version has a precedent** (Iotti
  et al. 2017). That comparison is established and is not a finding of this project.
- **A per-link transmission rule has a precedent** (Watts & Strogatz 1998) and behaves as a
  percolation process with a threshold (Barthélemy 2011).
- **The bond percolation threshold of the square lattice is 1/2** (Barthélemy 2011). This is the
  known answer used to validate the estimator.
- **At the same degree distribution, spatial structure reduces onward transmission through short
  loops** (Ghadiri et al. 2026, preprint). Shown for uniform random points against Erdős–Rényi
  graphs, and for a reproduction number, not a threshold.
- **Proximity graphs are highly clustered**, and uneven point density becomes uneven degree
  (Barthélemy 2011; Iotti et al. 2017).
- **Degree-preserving rewiring leaves assortativity free to change** (Iotti et al. 2017).
- **Association-based scoring propagates erroneous seed labels.** One research system, tested on
  synthetic data, degrades sharply when many initial labels are wrong (Macskassy & Provost 2005).
- **Random cross-validation overstates performance on spatially dependent data**; blocked or
  spatial cross-validation is the remedy when predicting into new areas (Roberts et al. 2017; Sun
  et al. 2023).
- **Footprint sources disagree in Nairobi.** City-wide, most OSM polygons have no matching
  Microsoft or Google polygon, and the Microsoft count is about 35% below the OSM count (Okyere
  et al. 2025, preprint).
- **Machine-derived footprints are weakest in dense contiguous settlement** (Sirko et al. 2021).
- **Site or city totals hide unmapped patches in OSM.** Las Vegas is described as a few
  well-mapped neighbourhoods among unmapped areas (Herfort et al. 2023).
- **Remote-mapping completeness in slums depends on building density and roof form**, across
  seven sites (Yeboah et al. 2021).
- **Critical values estimated on small areas are noisy** (Behnisch et al. 2019; Fagundes et al.
  2025).

**Mixed or unsettled**

- **Which graph has the lower critical value.** Lattice percolation results say rewiring lowers
  the threshold (Barthélemy). An SIS study on random geometric graphs reports the estimated
  critical value rising under heavy rewiring, while outbreaks become easier (Iotti et al.).
  Ghadiri et al. give no threshold result. The direction of `Q` is not assumed.
- **Whether completeness varies within the Kibera site.** Yeboah et al. report one figure per
  site, and their sites are anonymised. Within-site variation is an inference.

**Not supported by anything read**

- That the two candidate sites differ in building arrangement.
- That either site's data is adequate.
- That the simulated rule matches any deployed or published scoring method (A4).
- That an evidence gap exists. No literature search has been done.
- Several background statements in the independent review: the size-scaling exponents, the
  threshold formula for rewired graphs, the effect of assortativity on thresholds, and the
  coverage and import history of footprint sources in the United States.

## 3. Assumptions

- **A1. Nodes are buildings**, under a node definition fixed at the pre-analysis gate (5.2).
- **A2. Edges are proximity** within distance `d` in a projected CRS, with `d` set per graph to
  reach a common target mean degree.
- **A3. Propagation rule (decisions D1 and 4).** A fraction `p` of nodes is seeded as erroneous.
  Each flagged node gets one attempt to flag each unflagged neighbour, succeeding with probability
  `t` independently per link. Spread continues until no new node is flagged. No hop limit, except
  in the 2-hop sensitivity run on amplification.
- **A4. The rule is a stylised abstraction, not a published method.** The one association-scoring
  system in the evidence (Macskassy & Provost 2005) averages scores over a node's neighbours,
  where the cascade gives each link an independent chance to transmit. Results under A3 cannot be
  read as results for that system. The difference is recorded as future work only (decision 5).
- **A5. Density, and what arrangement includes (decisions 1, 2, D6, 7).**
  - Density is a confound. It includes the whole degree sequence, which rewiring holds fixed.
  - Arrangement is who connects to whom at fixed degrees.
  - **Arrangement includes the spatial concentration of high-degree nodes** (decision 7). In a
    distance-band graph a tightly packed patch produces high-degree nodes that are linked to
    each other. Rewiring keeps each node's degree and breaks that linking. The
    observed-to-rewired ratio therefore contains both the local pattern of connections and the
    clumping of dense patches. The study does not separate the two.
  - Degree assortativity is reported for every observed graph as a diagnostic of that clumping.
    It is not tested.
- **A6. Two sites are two cases**, not a sample of layout types. The suburb is chosen as the most
  grid-like of five candidates, so it is an extreme case by design.
- **A7. Arrangement effect on amplification** is the ratio of amplification factors, observed
  over rewired. It is now a secondary, descriptive quantity.
- **A8. Readings of decision 4, confirmed by decision 5.**
  - "All compared graphs" means both real sites, their rewired ensembles, and the synthetic
    patterns with theirs.
  - The critical-`t` estimation method is fixed on synthetic patterns before it is applied to a
    real graph.
  - The third reading, which placed the 1.25 margin on the amplification contrast, no longer
    applies. Amplification has no margin (decision 7).
- **A9. Readings of decision 5, confirmed by decision 6.**
  - Raw critical `t` is reported for every graph. The cross-site comparison uses each site's
    observed-to-rewired ratio of critical `t`.
  - A spatial block is the unit of the sub-window model, if that model runs.
- **A10. Agent operational readings of decision 6, as amended by decision 7.**
  - *"Better-matching source" at Kibera* (three sources): the source whose building count agrees
    most closely with a third source. The two that agree identify the outlier.
  - *"Better-matching source" at the suburb* (two sources): no third source exists. If OSM and
    Microsoft differ by more than 30%, the source with the higher spot-check tally is used. A
    tie goes to the source with more footprints, because the spot check tests for false
    footprints and cannot test for omissions.
  - *"Passes a visual spot check"*: at least 45 of the 50 sampled footprints correspond to one
    real structure in current imagery. The level is written into the gate protocol before the
    sample is drawn.
  - *"Within the synthetic gradient's range"*: every arrangement metric of the real site lies
    between the minimum and maximum of that metric across the synthetic patterns.
  - *"Stop"* means the study does not proceed at that site with any footprint source.
- **A11. Agent operational readings of decision 7.** Stated so the work can proceed; not requests
  for rulings. Each is open to challenge at the methodology stage.
  - *"Building count"* is the number of nodes in the core area after the node definition is
    applied. Kibera's value is written `N*`. The suburb's core must hold between 0.9 `N*` and
    1.1 `N*` nodes. Every synthetic pattern has `N*` core nodes, as near as its construction
    allows.
  - *Suburb window.* Each candidate's window is a square centred on the nominated neighbourhood,
    grown until its node count is in range. The shape and centring rule are fixed before any
    indicator is computed. Ground area is then whatever results, and is reported.
  - *"Sole primary"* turns the two-contrast table approved in decision 6 into a rule on one
    contrast (4.4).
  - *"Negligible" Monte Carlo error*: the Monte Carlo standard error of `D` is at most one tenth
    of the half-width of its block-bootstrap interval. The fraction is a convention.
  - *"Largest connected component"* is taken on the full graph, core plus buffer, with every link
    present. Each rewired graph has its own largest component. The share of nodes in it is
    reported for every graph. Cluster sizes are counted over core nodes inside that component.
  - *"Stated tolerance"*: the estimator passes if its mean over at least 100 realisations lies
    within 0.50 ± 0.02 on a four-neighbour square lattice of at least 250,000 nodes. The gap from
    0.50 on lattices of `N*` nodes is then reported as the estimator's size bias. Both numbers
    are conventions set by the agent, with no evidence basis.
  - *Clustered end of the gradient*: a Thomas process, at two or more clustering strengths taken
    from a grid fixed in the pilot. A Matérn cluster process is the substitute if the Thomas
    process cannot be held to `N*` nodes.
  - *Position on the gradient* is the coefficient of variation of nearest-neighbour distance. It
    is 0 for a lattice, about 0.52 for uniform random points (agent arithmetic for a Poisson
    pattern), and larger for clustered patterns, so it orders all three segments on one axis.
  - *"Completeness within the Kibera site"*: the site is divided into grid cells, and building
    counts and total footprint area are compared across the three sources cell by cell. A cell
    is flagged when the chosen source falls more than 30% below both others on either measure.
  - *"Expected path"*: planning, packets, and the write-up assume the fallback form. The
    condition for the sub-window form (decision 5) is still checked at the gate.
  - *"Application"*: section 4.6 gives the wording.

## 4. The research question

### 4.1 Question

With density held fixed, does the arrangement of connections in building-level proximity graphs
change how far injected classification errors spread under probabilistic propagation, and does the
size of that effect differ between Kibera and a planned US suburban area?

The wording is unchanged from v7. It is a narrowed form of the problem in `inputs/problem.md`,
narrowed by the human (decisions 1 to 4). It remains a moderation question: arrangement moderates
the relationship between injected error and error reach.

Two clarifications of terms, which do not change the question:

- "Classification errors" are flags injected at random and spread by rule A3. No classifier is
  trained or evaluated (independent review, issue 11).
- "How far errors spread" is measured primarily by the transmission probability at which spread
  stops being local, relative to a rewired graph with the same degrees (decision 7).

### 4.2 Estimands

**Primary (decision 7)**

- **Critical transmission probability** `t_c(G)`: the `t` at which spread on `G` stops being
  local. Estimated on the largest connected component, with cluster sizes counted on core nodes.
  Reported for every graph.
- **Critical-value ratio** `Q(G) = t_c(G) / mean t_c(rewired G)`. `Q > 1` means arrangement delays
  the onset of extensive spread relative to arbitrary wiring at the same degrees.
- **Cross-site critical-value contrast** `D = log Q(Kibera) − log Q(suburb)`. **This is the sole
  primary estimand.** Minimum contrast of interest: `|D| ≥ log 1.25`, that is a cross-site ratio
  of `Q` at or above 1.25 or at or below 0.80 (decision 6). The 1.25 figure is a convention fixed
  in advance; it has no basis in evidence or in a stated consequence.

`t_c` does not depend on the seeding rate `p`. Rule A3 is equivalent to keeping each link with
probability `t` and taking the clusters that contain seeds (independent review, issue 5;
consistent with Barthélemy p. 91). The pilot confirms this on one graph.

**Secondary and descriptive (decision 7)**

- **Amplification factor** `A(G; t, p)`: expected number of non-seeded nodes flagged per seeded
  node, at sub-critical `t`.
- **Arrangement effect on amplification** `R(G; t, p) = A(G) / mean A(rewired G)`.
- **Cross-site amplification contrast** `C(t, p) = log R(Kibera) − log R(suburb)`.

These are reported with Monte Carlo intervals. They are not tested against a margin and **do not
count as corroboration** of the primary result, whatever their size or sign.

**Diagnostic (decision 7)**

- **Degree assortativity** of each observed graph, with the mean over its rewired ensemble beside
  it for reference. Reported next to `Q`. Not tested.

### 4.3 Hypotheses

- **H1-T (primary, critical-value contrast).** `|D| ≥ log 1.25`. Two-sided. Judged on the
  block-bootstrap interval (4.4).
  - *Direction.* `inputs/problem.md` expects irregular layouts to spread error further. After
    the density control, the nearest equivalent is that Kibera's arrangement delays the onset of
    extensive spread **less** than the suburb's: `Q(Kibera) < Q(suburb)`, so `D ≤ −log 1.25`.
    A result at `D ≥ +log 1.25` would meet the margin and be **contrary to** that expectation.
    The mapping is the agent's, and it is partial, because the original expectation included
    density.
- **H1-R (effect sizes, not a test).** `Q` is reported per site with its interval. `Q > 1` is
  expected from prior theory on lattices. If found, it is **not a finding of this study**. The
  expectation is checked on the synthetic patterns first.
- **H1-G (supporting, synthetic gradient).** Across synthetic patterns at fixed node count and
  mean degree, `Q` changes monotonically with the coefficient of variation of nearest-neighbour
  distance, from lattice through uniform random to clustered. This is the only place arrangement
  is manipulated directly. Results are also given for each segment separately, because the
  clustered segment changes degree heterogeneity and assortativity as well as local pattern.
- **H1-L (supporting, learning component).** The fallback form is the expected path (decision 7):
  a model trained on the synthetic gradient predicts each real site's `Q` from its arrangement
  metrics. The sub-window form replaces it only if the condition in 5.7 is met.
- **Secondary description.** `R` and `C` are reported at the pre-specified sub-critical `t` and
  `p`. No hypothesis is attached to them.
- **Competing explanations for a non-zero `D`.**
  - Node definition or footprint source differs in meaning between sites.
  - Mapping completeness is uneven inside a site, removing high-degree nodes.
  - A residual graph-size effect within the ±10% matching band.
  - The spatial concentration of high-degree nodes, which decision 7 counts as arrangement. A
    reader who counts it as density would read the same `D` differently.

### 4.4 Decision rule for the primary contrast

Decision 7 makes `D` the sole primary estimand and names the interval. The rule below is the
one-contrast form of the table approved in decision 6.

| Block-bootstrap interval for `D` | Reading |
|---|---|
| Wholly at or beyond `+log 1.25`, or wholly at or beyond `−log 1.25` | Supports the hypothesis for the onset of extensive spread, within this model. The sign is reported against the expectation in 4.3 |
| Wholly inside (`−log 1.25`, `+log 1.25`) | Refuted within this model, subject to section 7 |
| Straddles either margin | **Inconclusive.** Reported as such, not as support or refutation |

- **The interval is the block-bootstrap interval** (decision 7). It describes how much `D` would
  move for a different piece of the same settlements.
- **Monte Carlo replicates** are run until their error is negligible relative to that interval
  (A11). The Monte Carlo interval is reported beside it as a precision check only. It never
  decides a row.
- **Amplification plays no part in the rule.** A large `C` with an inconclusive `D` is an
  inconclusive study.
- An inconclusive result is an acceptable outcome (decision 6).

### 4.5 Three-step decomposition (decision D6)

Reported for amplification, which is now descriptive:

1. **Unadjusted.** Amplification at a common metric threshold `d`, density left free.
2. **Mean-degree matched.** Amplification with `d` set per site to a common target mean degree.
3. **Degree-sequence controlled.** `R` and `C`.

Raw `t_c` at the matched mean degree and `Q` are the counterparts of steps 2 and 3 for the
primary quantity. A step-1 `t_c` is not added.

Step 2 to 3 removes the degree sequence. It does not remove where the dense patches sit; that
stays in step 3 by decision 7 (A5).

### 4.6 Contribution statement (decision 7)

This study is an **application**. It applies two established methods, the comparison of a spatial
graph with its degree-preserving rewiring (Iotti et al. 2017) and percolation-threshold
estimation on building locations (Behnisch et al. 2019), to building-proximity graphs at two
sites and on a synthetic gradient. No novelty, evidence-gap, causal, policy, or external-validity
claim is made. Any such claim needs human approval (AGENTS.md rule 9) and, for novelty, a
literature search that has not been done.

## 5. Design components

### 5.1 Estimator validation and synthetic pilot (first)

**Step A. Validate the `t_c` estimator on a square lattice (decision 7). No data needed.**

- Graph: four-neighbour square lattice. Known bond percolation threshold: 0.5 (Barthélemy
  pp. 85–86).
- Candidate estimator: the `t` at which the second-largest cluster peaks (documented for a
  distance threshold in Behnisch et al. p. 4; its use for `t` is an agent inference).
- Pass criterion: A11. **No other graph is analysed until the pass is recorded.**
- Then run the estimator on lattices of `N*` nodes and report the gap from 0.5 as its size bias.
- If the estimator fails, it is replaced and the validation repeated. The failed attempt is kept
  in the record.

**Step B. Synthetic pilot at `N*` nodes.**

- `N*` comes from the Kibera count step in 5.2. That step reads footprints and counts nodes. It
  computes no graph outcome. Pilot code can be developed at a placeholder size; the runs that fix
  operating values use `N*`.
- Patterns: the gradient in 5.4, each with a core and a buffer built as in 5.8.
- For each: the observed graph and its rewired ensemble; `t_c`, `Q`, assortativity, and the
  largest-component share.
- Fix, before any propagation or threshold result on a real graph is seen:
  - the estimator settings, including a rule for curves with more than one peak;
  - the block-bootstrap scheme, block size, and number of resamples;
  - the rewiring settings and the mixing check;
  - the target mean degree and the buffer width as a multiple of `d`;
  - the Thomas-process parameter grid;
  - the `t` and `p` values for the secondary amplification runs.
- Checks:
  - **Cascade against bond percolation.** Simulated cascades and link-removal clusters give the
    same `t_c` on one graph.
  - **Rewiring is fully mixed.** An edge-crossing swap can leave spatially correlated edge pairs
    under partial rewiring (Iotti et al. p. 3); `networkx.double_edge_swap` is such a swap.
  - **Block bootstrap against independent realisations.** On synthetic patterns the spread over
    many independent realisations is available for comparison.
  - Record which graph has the lowest `t_c`, and the sign of `Q − 1`. Neither is assumed.
- The pilot sets operating values only. The primary estimand, margin, and rule are fixed.

### 5.2 Pre-analysis gate: sources, completeness, node definition, node count (required)

**Kibera**

1. Download OSM, Microsoft, and Google footprints for the study area plus a buffer. Record each
   source's date. Project to EPSG:32737.
2. Tabulate building counts, footprint areas, and nearest-neighbour distances per source.
3. **Source rule (decisions 6 and 7).** If the OSM count differs from Microsoft or Google by more
   than 30%, switch to the better-matching source (A10). The source used must pass a visual spot
   check of 50 randomly sampled buildings. Stop only if no source passes.
4. **Within-site completeness (decision 7).** Compare counts and total footprint area across the
   three sources by grid cell (A11). Map the result. Record every flagged cell.
5. Fix the node definition: minimum footprint area, and whether touching polygons are merged.
   Centroid distance and a minimum-area filter have precedent (Behnisch et al. p. 2).
6. Record `N*`, the core node count.

**Suburb**

7. Confirm that OSM and Microsoft footprints both exist for the metro under consideration. OSM
   building coverage in United States cities can be patchy (Herfort et al. p. 6).
8. Nominate 5 planned suburban neighbourhoods in one metro, each with a one-line justification.
9. **Human approval of the five, before any indicator is computed** (decision 6).
10. Size each candidate's window to hold 0.9 `N*` to 1.1 `N*` nodes under the same node
    definition (A11). Windows are sized on Microsoft footprints for all five, so that patchy OSM
    coverage does not decide the window.
11. Compute orientation order and dead-end share on each window's street network with OSMnx
    (indicators as defined in Boeing 2019, pp. 5–6).
12. Rank by orientation order descending and by dead-end share ascending; select the lowest
    summed rank; break ties by orientation order. City-wide values are not used.
13. **Source rule at the selected suburb (decision 7).** Compare OSM and Microsoft. Apply the 30%
    rule with the two-source reading in A10. Spot-check 50 randomly sampled buildings. If the
    chosen source changes the count, re-size the window once to stay within ±10% of `N*`.

**Both sites**

14. Record, per site: the source used, all counts, spot-check tallies, source dates, ground area,
    and core node count.
15. Count the spatial blocks each site supports and record whether the sub-window condition in
    5.7 is met.

The suburb rule uses street indicators and building counts only, and no outcome of the study, so
it cannot be tuned toward a result. It does not guarantee a regular *building* pattern; that is
measured afterwards (5.5).

### 5.3 Graphs

- Project to local UTM (EPSG:32737 for Nairobi; the suburb's zone once chosen).
- Distance-band graphs via libpysal, converted to NetworkX. `d` is set so that the mean degree of
  **core** nodes equals the target (agent default, following the edge rule in 5.8).
- Report the `d` each site needs, what it physically spans, and each site's ground area, as
  descriptive results. At matched node count the suburb will cover a much larger area than
  Kibera, and its `d` will be much larger.
- Rewire the **full** graph, core plus buffer (decision 7), using the settings fixed in 5.1.
- For every observed and rewired graph: find the largest connected component, record its share of
  nodes, and estimate `t_c` on it by the validated method. Compute `Q` and `D`.
- Report degree assortativity for each observed graph.
- Only then run the secondary amplification analysis, at `t` below the lowest `t_c` of all
  compared graphs (decision 4).

### 5.4 Synthetic gradient (decisions D2, D4, 7)

Three segments, all at `N*` core nodes and the target mean degree:

1. **Regular lattice**, then lattices with increasing random displacement of points.
2. **Uniform random points.**
3. **Clustered points** from a Thomas process at two or more clustering strengths (A11).

- Each pattern has `R`, `t_c`, `Q`, assortativity, largest-component share, and the 5.5 metrics.
- Position on the gradient is the coefficient of variation of nearest-neighbour distance (A11).
- Both real sites are placed on the gradient by that metric. Their other metrics are reported
  beside it, since a site can sit at different positions on different metrics.
- Clustered patterns under a distance band are expected to fragment more and to have more uneven
  degrees than the other segments (agent inference). The largest-component share and
  assortativity are therefore read alongside `Q` on this segment.

### 5.5 Arrangement metrics and diagnostic

Per site and per synthetic pattern (and per sub-window if 5.7 runs in that form):

- coefficient of variation of nearest-neighbour distance (also the gradient axis);
- a pair-correlation or Ripley's K summary;
- mean clustering coefficient at the target mean degree. This is expected to be nearly constant
  along the random part of the gradient (Barthélemy pp. 43–44), so it may add little;
- an orientation-entropy measure adapted from Boeing (2019); the adaptation from street bearings
  to building or local street orientation is an agent proposal.

Diagnostic, reported and not used as a predictor: degree assortativity (A5).

### 5.6 Sensitivity analyses

For `Q` and `D`:

- target mean degree;
- node definition; centroid versus footprint-edge distance;
- k-nearest-neighbour graphs (robustness only), with the symmetrisation rule stated.

For amplification only:

- 2-hop-limited propagation (decision 4);
- `p`; `t` across the sub-critical grid.

### 5.7 Learning component (decisions D5, 4, 5, 6, 7)

Exactly one form runs. **The fallback is the expected path** (decision 7).

**Fallback form (expected)**

- Training data: synthetic gradient patterns across all three segments, each with its 5.5 metrics
  and estimated `Q`.
- Model: a **linear model is primary**, with a tree-based model alongside for comparison. This
  reverses the v7 default (O9) on the independent review's point that a tree-based model returns
  a constant outside its training range and so cannot give a meaningful value for a site
  reported as extrapolation. It is an agent default, not a ruling.
- Test: the two real sites. A prediction counts as in-sample only if the site's metrics fall
  within the synthetic gradient's range (A10); otherwise it is reported as extrapolation
  (decision 6).
- **This form does not exercise spatial-leakage control** (decision 5). Training and test data
  share no space, so the leakage described by Roberts et al. and Sun et al. cannot occur, and
  cannot be shown to be controlled either. The write-up must say that the spatial-leakage
  requirement in `inputs/problem.md` is described and not demonstrated.

**Sub-window form (only if its condition is met)**

- Condition (decision 5): each site yields at least 20 spatial blocks, each wider than the
  typical cascade extent. At a matched count of `N*` nodes this is not expected.
- Unit: a spatial block with its own graph, rewired ensemble, and `R`. Response: log `R` per
  block, which is now a secondary quantity. Predictors: the 5.5 metrics.
- Model: linear regression, or a spatial lag or error model from `spreg` if residuals are
  spatially autocorrelated, against a site-mean baseline (decision 6).
- Validation: 5-fold spatially blocked cross-validation, with blocks checked against the range of
  residual autocorrelation (Roberts et al. pp. 918–920).

### 5.8 Edge handling and intervals (decision 7)

**One rule for every graph: real, synthetic, observed, rewired.**

- Each graph has a core and a buffer. The buffer is a band of real buildings (or synthetic
  points) around the core, of a width fixed in the pilot as a multiple of `d`.
- The **full graph, core plus buffer, is rewired.**
- Propagation and cluster formation run on the full graph.
- **All outcomes are computed on core nodes only**: cluster sizes for `t_c`, flagged counts for
  amplification, and assortativity and arrangement metrics where they are node-based.
- The core share of nodes is reported for every graph.

In a rewired graph the core is the same set of node labels as in the observed graph. Those nodes
no longer form a spatial interior, because rewired links join any two nodes. This is a
consequence of the rule and is recorded as a concern (M5).

**Intervals**

- The interval for `Q` and `D` is the **block-bootstrap interval** over spatial blocks of the
  observed graph, with the scheme fixed in the pilot.
- Monte Carlo replicates continue until the criterion in A11 is met. Their interval is reported
  as precision only.
- No significance test is reported for the site term.

## 6. Threats to validity

**Primary estimand**

- **The critical value is the noisiest quantity in the study**, and it now carries the whole
  result. Critical values on small areas are noisy (Behnisch et al. p. 4; Fagundes et al. p. 1).
  A wide interval for `D` makes the study inconclusive.
- **A block bootstrap of a whole-graph quantity is not straightforward.** *Agent inference.*
  `t_c` belongs to a connected graph. Resampling blocks cuts the links that cross block
  boundaries, and a value estimated on a small block is shifted relative to the whole site. The
  rewired graph has no spatial blocks at all. The interval may be biased or too wide. The pilot
  checks it on synthetic patterns.
- **Matching node count removes the main size confound, not all of it.** Sites may differ by up
  to 10%. The observed and rewired graphs may also respond to size differently, in which case
  `Q` keeps a size dependence that matching at one `N*` does not reveal. The scaling laws quoted
  in the independent review are unsourced.
- **The validated estimator may still be biased at `N*`.** The pass criterion binds on a large
  lattice. The bias at `N*` is reported, not gated. It cancels in `D` only to the extent that it
  is the same for both sites.
- **A square lattice is the easy case.** Passing there does not show the estimator behaves on
  patchy graphs, where the second-largest-cluster curve can have several peaks.
- **The largest component may be a different share of each site.** A settlement cut by a rail
  line or a suburb cut by arterial roads may split at the chosen `d`. `Q` then describes
  different fractions of the two sites. No minimum share is set.
- **`Q` is a normalisation, not a proven one.** *Agent inference.* Whether arrangement and degree
  heterogeneity combine multiplicatively in `t_c` is not established.
- **The arrangement effect includes the clumping of dense patches** (A5). A non-zero `D` may come
  from that clumping and not from local connection pattern. Assortativity helps a reader judge
  this. Nothing in the design separates the two.

**Edge rule**

- **Core nodes in a rewired graph are not a spatial interior.** Measuring on the same node labels
  keeps the rule uniform, but the buffer protects the observed graph from boundary truncation and
  does nothing comparable for the rewired graph.
- **Runs near and above threshold reach the buffer's outer edge.** Outer buffer nodes have
  truncated neighbourhoods, which lowers their degree in the observed graph.

**Data**

- **Uneven completeness would bias degree where it matters most.** *Agent inference from Yeboah
  et al.* If the densest patches are the least completely mapped, the missing buildings are
  high-degree nodes, and the bias enters `Q` directly.
- **Yeboah et al. motivates the check; it does not predict the result.** Its sites are
  anonymised, it reports one figure per site, and its low figures describe remote tracing before
  fieldwork.
- **The within-site check compares sources with each other.** If all three under-map the same
  cells, no cell is flagged. Machine-derived sources are weakest in dense contiguous settlement
  (Sirko et al. pp. 1, 4).
- **A count difference does not say which source is wrong.** In Nairobi, OSM and machine-derived
  polygons often do not match one to one (Okyere et al. pp. 14–16). The 30% rule could move
  Kibera off OSM for a reason unrelated to OSM's quality.
- **Two sources at the suburb may not be independent.** OSM footprints there may have been
  imported from another dataset (Herfort et al. p. 9, in general terms). Agreement would then
  show common origin, not accuracy. The spot check cannot detect omitted buildings.
- **The sites may end on different footprint sources**, with different dates and different
  meanings of "one polygon".
- **Temporal misalignment** between sources and between sites.

**Design**

- **Count matching makes the two sites very different in ground area and in `d`.** The same
  per-link probability `t` then spans very different physical distances. Decision 2 treats this
  as descriptive; it still limits what "the same error process" means across sites.
- **Site confounding.** Two places are two cases (A6). A non-zero `D` cannot be attributed to
  layout type. The gradient reduces this; it does not remove it.
- **The gradient may not reach either site.** Real building patterns cannot overlap, and they are
  patchy and laid out in rows. Three synthetic segments may contain neither site.
- **The clustered segment changes several things at once**: local pattern, degree heterogeneity,
  assortativity, and connectedness.
- **The rewired null is not spatial.** `Q` measures spatial arrangement against arbitrary wiring.
- **Rewiring bias.** Incomplete mixing would leave spatial correlation in the null.
- **Rule realism.** Results describe an independent cascade. The one documented scoring method in
  the evidence works differently and may respond to degree in the opposite direction (A4).
- **MAUP.** Scale form: target mean degree and `d`. Zoning form: node merging, block layout for
  the bootstrap, and grid cells for the completeness check.
- **Suburb rule.** Nomination is a judgement, made visible by the justifications and the approval
  step. The ranking uses street layout, which need not track building arrangement.

**Learning component**

- **Fallback form: two test cases.** Two predictions give two errors, not an estimate of skill.
  A site inside each metric's range can still lie outside the region the patterns jointly cover.
  Training rows from the same gradient step are not independent.
- **Fallback form: no leakage control is exercised.**

**Secondary quantities**

- **Amplification is forced toward equality at low `t`.** To first order in `t`, the number
  flagged per seed is `t` times the mean degree on any graph (agent derivation; consistent with
  Ghadiri et al. p. 6 for a related quantity). `R` then mostly reflects how far `t` is from each
  rewired ensemble's threshold, which the degree sequence sets. This is why decision 7 makes it
  descriptive.

## 7. What would weaken or refute the hypothesis

- **Refuted within this model:** the block-bootstrap interval for `D` lies wholly inside the
  margin, **and** the two sites sit at similar positions on the synthetic gradient.
- **Inconclusive:** the interval straddles a margin. Not a refutation and not support.
- **Not attributable to arrangement:**
  - `D` changes sign across node definitions or footprint sources;
  - the largest-component shares of the two sites differ widely;
  - flagged completeness cells at Kibera coincide with its densest patches.
- **Read with caution:** `D` meets the margin and the two sites differ sharply in assortativity.
  The result then rests largely on the clumping of dense patches.
- **H1-G fails:** `Q` does not vary with the gradient metric. This undercuts the mechanism even if
  the real sites differ.
- **The estimator fails validation:** no result on any graph is reported until it passes.
- **H1-L, fallback form, fails:** the model's errors on the two real sites are no smaller than
  those of a baseline that predicts the mean `Q` of the synthetic patterns. With two cases this
  is weak evidence in either direction, and weaker still for a site reported as extrapolation.

## 8. Disposition of earlier candidates

| v3 candidate | Status |
|---|---|
| RQ1 | The approved question (section 4) |
| RQ2 threshold sensitivity | Absorbed as 5.6 |
| RQ3 structural predictors | Reduced; learning component 5.7, fallback form expected |
| RQ4 clustered seeding | Dropped |
| RQ5 footprint robustness | Pre-analysis gate 5.2 |

## 9. Response to the reviews

### 9.1 Independent review (`outputs/scientific_critic_independent.md`)

| Review issue | Severity given | Resolution in v8 |
|---|---|---|
| 1. Cross-site contrast confounded with graph size | Blocking for H1-T | Suburb matched to Kibera on node count within ±10%; synthetic patterns at the same count (decision 7). Residual size effect recorded as M3 |
| 2. `Q > 1` and `R < 1` near-guaranteed; low-`t` test adds little | Major | `D` is the sole primary estimand; amplification is descriptive and is not corroboration (decision 7). `Q > 1` stated as expected and not a finding (4.3) |
| 3. Decision rule does not name its interval | Blocking for the rule | Block-bootstrap interval decides; Monte Carlo is a precision check (decision 7). The review preferred window subsampling; the ruling keeps the block bootstrap (M1) |
| 4. Rewiring does not remove all of what D6 calls density | Major | Arrangement explicitly includes spatial concentration of high-degree nodes; assortativity reported; no second null (decision 7; A5) |
| 5. Critical value may be undefined; estimator uncalibrated | Major | Largest component with its share reported; estimator validated on the square lattice before any other use (decision 7). No minimum share is set (M6) |
| 6. Gate assumes three independent sources at both sites | Major | Three sources at Kibera, two at the suburb, spot check at both, within-site completeness at Kibera (decision 7). No omission check at the suburb (M10) |
| 7. Gradient may not reach either site | Major | Gradient extended to clustered patterns (decision 7). No minimum-spacing pattern was added (M11) |
| 8. Edge handling specified for local spread on the observed graph only | Major | Full graph rewired; all outcomes on core nodes; one rule for every graph (decision 7). `d` calibrated on core nodes as an agent default |
| 9. Sub-window model unlikely to run or inform | Major for H1-L | Fallback is the expected path (decision 7). Linear model made primary as an agent default |
| 10. No evidence gap established | Major for novelty | No novelty wording; contribution described as an application (decision 7; 4.6). No literature search has been done (M14) |
| 11. Result is about a cascade model | Minor | A4 retained; terms clarified under 4.1 without rewording the question (M15) |
| 12. Smaller points | Minor | Margin called a convention (4.2). Direction mapped to the sign of `D` (4.3). Suburb stated as an extreme case (A6). Clustering metric's limit noted (5.5). Rewired fragmentation handled by the largest-component rule. Section 1 corrected |

### 9.2 Earlier review (`outputs/history/question_review.md`)

Resolutions are as recorded in v7 section 9, with these changes:

| Earlier issue | Change in v8 |
|---|---|
| 1. Estimand undefined | The primary estimand is now `D` alone |
| 4. One pair of sites | Unchanged in substance; the gradient now has three segments |
| 9. Edge handling | Replaced by the single rule in 5.8 |
| 10. Learning component; leakage | Fallback expected; leakage control described, not demonstrated |
| 11. No evidence gap established | Still open; no novelty wording is used |

## 10. Open points

The question is frozen (decision 7). **Nothing here requests a ruling.** Items are grouped by who
settles them.

### 10.1 Operating values the pilot fixes

- **O1. `t` and `p` for the secondary amplification runs.** All `t` lie below the lowest `t_c` of
  all compared graphs (decision 4). Each is also reported as a fraction of that graph's own
  rewired threshold, as the independent review suggested.
- **O2. Target mean degree.** A square lattice under a distance rule admits only certain degrees
  (4, 8, 12), and a uniform random pattern needs a mean degree above about 4.5 to hold together
  (agent arithmetic from Barthélemy p. 43). One prior study used 8 (Iotti et al. p. 4). The
  validation lattice in step A is four-neighbour whatever value is chosen here.
- **O3. Rewiring details.** Number of swaps, ensemble size, and the mixing check.
- **O4. Estimator settings**, including the rule for curves with more than one peak.
- **O5. Block-bootstrap scheme**, block size, number of resamples, and interval level.
- **O6. Buffer width** as a multiple of `d`.
- **O7. Thomas-process parameter grid** and the number of displaced-lattice steps.

### 10.2 Agent defaults in force (A10, A11)

Listed so the methodology critic can challenge each one.

- **G1.** Node count means core nodes after the node definition; `N*` is Kibera's.
- **G2.** Suburb windows are squares grown to the count, sized on Microsoft footprints.
- **G3.** Monte Carlo error is negligible at one tenth of the bootstrap half-width.
- **G4.** Largest component taken per graph on core plus buffer; sizes counted on core nodes.
- **G5.** Validation tolerance 0.50 ± 0.02 on a lattice of at least 250,000 nodes.
- **G6.** Thomas process for the clustered segment; Matérn cluster as substitute.
- **G7.** Gradient position is the coefficient of variation of nearest-neighbour distance.
- **G8.** A completeness cell is flagged at more than 30% below both other sources.
- **G9.** Two-source rule at the suburb: higher spot-check tally, then more footprints.
- **G10.** Spot-check pass level 45 of 50.
- **G11.** `d` calibrated on core-node mean degree.
- **G12.** Fallback model: linear primary, tree-based alongside.
- **G13.** "Within range" judged metric by metric.

### 10.3 Concerns for the methodology stage

- **M1. What a block bootstrap of `t_c` is.** The scheme is undefined: how blocks are recombined
  into a graph, how links across block boundaries are treated, and what the rewired denominator
  is for each resample. The independent review preferred non-overlapping windows of equal node
  count. If the pilot shows the bootstrap behaving poorly, that is a limitation of the interval
  and most likely an inconclusive study.
- **M2. Where the tolerance binds.** G5 gates on a large lattice and only reports the bias at
  `N*`. The methodology stage should decide whether a bound at `N*` is also needed.
- **M3. Residual size effect.** A cheap pilot check is `Q` for one fixed synthetic pattern at
  0.9 `N*`, `N*`, and 1.1 `N*`. If `Q` moves by more than a small fraction of `log 1.25` across
  that band, ±10% is too loose. This is a suggestion, not a test in the design.
- **M4. Ground area and `d` after count matching.** The suburb window may be many times
  Kibera's area. Check that the street indicators in step 11 are still meaningful on a window of
  that size, and that the buffer rule is workable.
- **M5. Core nodes in rewired graphs.** Compare `Q` with and without a buffer on one synthetic
  pattern to see how much the uniform rule matters.
- **M6. Largest-component share.** No minimum is set. The independent review suggested 90% of
  core nodes. The methodology stage should state what is done when the share is low or differs
  between sites.
- **M7. Reading `D` when assortativity differs between sites.** No test separates local pattern
  from the clumping of dense patches. The write-up needs agreed wording for that case.
- **M8. What follows a flagged completeness cell.** Decision 7 requires the check, not a
  response. Agent suggestion: report the map, proceed on the gate-chosen source, and repeat `Q`
  on the source with the highest count in the flagged cells as a sensitivity run. The Map Kibera
  documentation, which would say how Kibera's buildings were mapped, has not been supplied.
- **M9. The 30% rule at Kibera.** City-wide figures suggest it will trigger (Okyere et al.). A
  switch decided by counts could select a machine-derived source in the setting where such
  sources are weakest. Comparing total built area as well as counts would be less sensitive to
  merged polygons.
- **M10. The suburb gate.** Two sources that may share an origin; no check for omitted
  buildings; OSM coverage that may be near zero. The order of window sizing and source choice
  (steps 10 and 13) should be confirmed.
- **M11. Reach of the gradient.** The clustered parameters are set from a grid fixed in the
  pilot, not tuned to the sites. A site outside the range is reported as extrapolation. The
  methodology stage may prefer to measure the sites' metrics first and span them; that uses no
  outcome data. A minimum-spacing pattern, which the independent review suggested, is not in the
  design.
- **M12. Fallback evidence value.** Two test cases; possibly both extrapolation.
- **M13. Unsourced background.** `outputs/evidence_notes.md` section 23 lists the independent
  review's statements that no supplied paper supports. The pilot check of the rewired threshold
  against a formula, which the review proposed, needs a source before it is used.
- **M14. No literature search.** Needed before any abstract goes beyond "application".
- **M15. Wording downstream.** Packets 03 and 04 should say "injected label errors under an
  independent-cascade rule" and repeat A4. The question in 4.1 keeps its frozen wording.
- **M16. Emphasis.** The independent review recommends leading the write-up with the synthetic
  gradient and presenting the sites as cases on it. No ruling was made; H1-G stays supporting.
- **M17. Conventions.** The 1.25 margin, the one-tenth Monte Carlo criterion, the 30% thresholds,
  and the validation tolerance have no evidence basis. They are fixed in advance and should be
  described as conventions.

### 10.4 Standing limitations

- **L1. Evidence.** Nineteen papers read, several in part and three by keyword search; eleven
  listed items unread. Two of the new papers are preprints. No literature search. No novelty,
  causal, policy, or external-validity claim without human approval.
- **L2. Review independence.** The independent review did not read the evidence and comes from
  the same model family as this document (section 1).
- **L3. Nothing has been measured.** Node counts, extents, arrangement metrics, source agreement,
  and completeness at both sites are all unknown.

## 11. Suggested next scientific action

1. **Validate the `t_c` estimator on the square lattice** (5.1, step A). It needs no data and
   gates everything else.
2. **Kibera count step** (5.2, steps 1 to 6): sources, spot check, within-site completeness, node
   definition, `N*`.
3. **Synthetic pilot at `N*`** (5.1, step B), fixing O1 to O7 and checking M3 and M5 if the
   methodology stage adopts them.
4. **Nominate 5 suburb candidates** with one-line justifications, for human approval, before any
   indicator is computed (5.2, steps 7 to 9).
5. **Suburb selection and gate** (5.2, steps 10 to 15).
6. Regenerate the prompt packets so they embed this v8 file. Two gaps in what the orchestrator
   embeds (`src/orchestrator.py`):
   - Packets 03 and 04 look for `outputs/question_review.md`. That file is now in
     `outputs/history/`, and the independent review is saved under a different name, so neither
     review would be embedded as things stand.
   - No packet embeds `outputs/evidence_notes.md`. Packet 04 should be given it by hand.

## 12. Future work (recorded, out of scope)

- **Averaging versus cascade propagation** (decision 5). The one documented association-scoring
  method in the evidence averages over neighbours, which dilutes a single erroneous neighbour as
  degree grows; the cascade used here does the opposite. Repeating the analysis under an
  averaging rule would show whether the conclusions depend on that choice.
- **A null that also preserves which degrees are linked to which.** It would separate local
  connection pattern from the clumping of dense patches (independent review, issue 4). Decision 7
  rules it out of this study.
- **The sub-window model and spatial-leakage control**, which the fallback does not exercise.
- **More than two sites**, so that the cross-site contrast is not a comparison of two cases.


## FILE: outputs/question_review.md

# Independent scientific review of the research question (v7)

Stage: 02 Scientific Critic Agent (independent rerun)
Packet: `outputs/prompt_packets/02_scientific_critic.md`
Date: 2026-10-04
Target: `outputs/research_questions.md` (v7, frozen pending this review)
Verdict: **REVISE** (the question in section 4.1 stands; its operationalisation needs four fixes
before real data is touched)

Nothing in this review is a finding. No data was examined and no code was run.

## 0. Independence statement

- I worked only from the packet text: `inputs/problem.md` and `outputs/research_questions.md` v7.
- I did not open `outputs/history/`, `outputs/question_review.md`, `outputs/evidence_notes.md`,
  the evidence PDFs, or any earlier critic output.
- **Limit on independence 1.** Section 9 of v7 lists the earlier review's eleven issue titles, so
  I have seen those titles. I have not seen the reasoning behind them.
- **Limit on independence 2.** This review is written by an LLM, very likely of the same family
  as the author. Under AGENTS.md rule 4, agreement between this review and v7 is not independent
  scientific validation. Points where I agree with v7 should carry less weight than points where
  I disagree and give a checkable reason.
- I could not check any page reference in v7, because I did not read the papers. Statements in
  v7 section 2 are taken as the author's reading, not as verified evidence.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` (as embedded in the packet) | Broad problem, boundary conditions, required validity threats |
| `outputs/research_questions.md` v7 (as embedded) | The object under review |
| Reviewer background knowledge of percolation on networks | Sections 4 and 5. Marked **[background, unverified]** wherever used. None of it is in `inputs/evidence/`, and it must be checked against a source before anyone relies on it |

## 2. Assumptions made by this review

- **R1.** The propagation rule in A3 is read literally: each flagged node tries each unflagged
  neighbour once, with independent success probability `t`, and there is no hop limit.
- **R2.** "Comparable extent" in decision 5 means comparable ground area, not comparable node
  count.
- **R3.** Every numeric value I give is rough reviewer arithmetic, intended to show whether an
  issue is large enough to matter. None is a result.
- **R4.** The scope freeze (decision 4) is respected. Where a recommendation would add work, I
  say so and leave the choice to the human.

## 3. Candidate questions and recommendation

v7 carries one live question (section 4.1). The other four candidates were absorbed, reduced,
or dropped by human decision (section 8), and I see no reason to revive any of them.

**Recommendation: keep the question in 4.1 without rewording.** The revisions requested below
change how it is measured and how the result is read. They do not change what is asked.

One observation about the question's two halves, for the human to weigh:

- *"Does arrangement change how far errors spread at fixed degrees?"* The answer is very likely
  yes for any spatial graph, on known theory (issue 2). v7 already treats this half as an effect
  size (H1-R), which is the right handling. It is not where the study can be wrong.
- *"Does the size of that effect differ between Kibera and a suburb?"* This half is falsifiable,
  but it is a comparison of two cases and it is exposed to a size confound (issue 1).
- The synthetic gradient (H1-G) is the only place where arrangement is manipulated with
  everything else held fixed. It is the strongest identifying evidence in the design, yet it is
  labelled "supporting". I recommend the write-up lead with it and present the two sites as
  cases placed on it. That is a change of emphasis for the human to approve, not a new question.

## 4. Scores

| Dimension | Score (1–5) | Basis |
|---|---|---|
| Importance | 3 | A clear methodological question about reliability of proximity-based inference. The link to any real scoring system is by analogy only (A4) |
| Novelty / evidence gap | 2 | Not established. No literature search has been done (v7 L1), and the core comparison is acknowledged as prior work |
| Testability | 4 | Estimands, margins, and a joint decision rule are fixed in advance. One gap: which interval the rule uses (issue 3) |
| Identification strategy | 3 | The gradient is sound. The cross-site contrast is confounded with graph size (issue 1) and partly with density unevenness (issue 4) |
| Data adequacy | 2 | Nothing has been measured. Node counts are unknown. One of the three footprint sources may not cover the suburb (issue 6) |
| Spatial validity | 3 | CRS, edge handling, MAUP, and blocking are all addressed in prose. Edge handling for the rewired graphs and above threshold is not (issue 8) |
| Robustness / validation | 3 | Good sensitivity list. No calibration of the critical-value estimator against a known answer (issue 5) |
| Feasibility | 3 | Simulation cost is manageable. The sub-window model is unlikely to be runnable or informative (issue 9) |

## 5. Issues

Ordered by severity. "Evidence" and "inference" are separated inside each item.

### Issue 1. The cross-site contrast is confounded with graph size

- **LOCATION / TARGET:** 4.2 (`Q`, `D`), 5.2 (suburb "of comparable extent"), decision 5.
- **SEVERITY:** blocking for the interpretation of H1-T; not blocking for the synthetic pilot.
- **WHY IT MATTERS:**
  - *Evidence from v7:* critical values on small areas are noisy (section 2); the suburb is
    matched to Kibera on extent; `d` is set per site to reach a common mean degree.
  - *Inference:* at equal ground area, a planned suburb will hold far fewer buildings than
    Kibera. It will also need a much larger `d`, so its width measured in units of `d` is much
    smaller. The two graphs then differ in node count `N` and in the share of nodes near an edge.
  - *Inference:* an estimated critical value on a finite graph is shifted from its large-graph
    value, and the shift depends on `N`. The shift follows a different law for a planar spatial
    graph than for a rewired graph **[background, unverified:** roughly `N^(-3/8)` for
    two-dimensional percolation and `N^(-1/3)` for random graphs**]**. Dividing one by the other
    in `Q` therefore does not cancel the size effect.
  - *Reviewer arithmetic:* for `N` between 500 and 2,000, `N^(-3/8)` is about 0.06 to 0.10
    before any prefactor. The margin is a ratio of 1.25. A size effect of that order could
    produce or hide a contrast at the margin with no difference in arrangement.
- **EVIDENCE NEEDED:** `Q` for one fixed synthetic pattern, computed at the two sites' actual
  node counts. If `Q` differs between those two sizes by more than a small fraction of
  `log 1.25`, the confound is live.
- **RECOMMENDED ACTION:**
  1. Add that check to the pilot (5.1). It needs no real data, but it needs the two node counts
     from the gate, so the gate's count step should run first.
  2. If the confound is live, compare the sites at matched `N`: estimate Kibera's `Q` on
     sub-windows that each hold about as many buildings as the suburb, and report the spread.
  3. Ask the human whether "comparable extent" in decision 5 should become "comparable node
     count". This reopens one clause of a ruling, which decision 6 permits the review to do.

### Issue 2. `Q > 1` and `R < 1` are close to guaranteed, and the low-`t` test adds little

- **LOCATION / TARGET:** 4.3 (H1-R, H1-C), 4.4, section 6 first two threats, O1.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Check of the agent derivation:* the first-order claim is correct. For a random seed, the
    expected number flagged is `t` times the mean degree on any graph. The second-order term is
    also fixed by the degree sequence except for triangles, so at low `t` the only arrangement
    signal in `R` is clustering.
  - *Background, unverified:* a fully rewired graph behaves like a random graph with the same
    degrees, whose threshold is about `<k> / (<k²> − <k>)`. At mean degree 8 that is near 0.125
    for Poisson-like degrees and 1/7 for a regular graph. Planar graphs at the same degree have
    thresholds well above that (my recollection for an eight-neighbour square lattice is near
    0.25).
  - *Inference:* the "lowest `t_c` of all compared graphs" will be a rewired graph's, most
    likely the one with the most uneven degrees. At any `t` below it, both observed graphs are
    far below their own thresholds, while each rewired graph sits at a different distance from
    its own. `R` then mostly measures that distance, which the degree sequence sets. This is the
    threat v7 already names, and I think it is the expected behaviour, not a risk.
  - *Consequence:* the row "H1-C meets margin, H1-T meets margin, same direction" counts H1-C as
    corroboration. If H1-C is driven by the degree sequence, it corroborates nothing. In
    practice the study has one primary estimand, `D`.
- **EVIDENCE NEEDED:** from the pilot, `R` as a function of `t / t_c(rewired)` for two synthetic
  patterns with the same arrangement and different degree spread.
- **RECOMMENDED ACTION:**
  1. In the pilot, compare the simulated rewired threshold with the formula above. Agreement is
     also a direct test that the rewiring is fully mixed (5.1 already asks for a mixing check).
  2. Place the primary `t` for H1-C as a fixed fraction of **each graph's own rewired**
     threshold, not of the global lowest. This is an O1 choice and is still open.
  3. State in 4.4 that the first row's support rests on H1-T. Say plainly in the write-up that
     `Q > 1` was expected from prior theory and is not a finding.
  4. Name one primary `(t, p)` pair for H1-C. v7 says "pre-specified values" in the plural,
     which leaves several chances to meet the margin.

### Issue 3. The decision rule does not say which interval it uses

- **LOCATION / TARGET:** 4.4, 5.8, decision 6 ("many realizations and bootstrap over spatial
  blocks").
- **SEVERITY:** blocking for the decision rule.
- **WHY IT MATTERS:**
  - *Inference:* the two interval types answer different questions. A Monte Carlo interval
    describes simulation noise for two fixed graphs, and it shrinks toward zero as runs are
    added. A block-bootstrap interval tries to describe how much the answer would move for a
    different piece of the same settlement.
  - With enough runs, the Monte Carlo interval will always land cleanly on one side of the
    margin. The rule would then never return "inconclusive", and every row would be a statement
    about these two graphs only.
  - With the block interval, v7 itself expects bias and excess width (section 6), so the rule
    may always return "inconclusive".
  - The outcome of the study depends on this choice, and it is not stated.
- **EVIDENCE NEEDED:** none; this is a specification gap.
- **RECOMMENDED ACTION:**
  1. State that 4.4 is judged on the spatial-variability interval, with the Monte Carlo interval
     reported beside it as simulation precision.
  2. Prefer window subsampling to a block bootstrap for `t_c`: estimate `Q` on several
     non-overlapping windows of equal node count and report their spread. It avoids stitching
     blocks into a graph that never existed, and it also serves issue 1.
  3. Record the choice before the pilot, since the pilot is where the method is tuned.

### Issue 4. Rewiring does not remove all of what decision D6 calls density

- **LOCATION / TARGET:** A5, 4.5 step 3, section 6 third threat.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Evidence from v7:* uneven point density becomes uneven degree (section 2); local packing
    unevenness is "counted as density by definition" (D6).
  - *Inference:* in a distance-band graph, a tightly packed patch produces high-degree nodes
    that are linked to each other. Rewiring keeps each node's degree and destroys that
    high-to-high linking. So the observed-versus-rewired difference contains two things:
    arrangement in the intended sense, and the spatial concentration of dense patches.
  - **[background, unverified]** thresholds depend on degree-degree correlation as well as on
    the degree sequence; graphs where high-degree nodes link to each other percolate earlier.
  - Decision 2 defines arrangement as "who connects to whom given fixed degrees", which includes
    this. D6 says packing unevenness is density. The two rulings pull in different directions
    here, and v7 resolves it silently in favour of decision 2.
- **EVIDENCE NEEDED:** degree assortativity of each observed graph; `Q` for a synthetic pattern
  with clustered density against a uniform pattern at the same mean degree.
- **RECOMMENDED ACTION:**
  1. Report degree assortativity for every observed graph next to `Q`. This costs one line of
     NetworkX.
  2. State the limitation in A5: step 3 controls the degree sequence, not where the dense
     patches sit.
  3. Optional, human's call under the scope freeze: a second null that also preserves which
     degrees are linked to which. It would separate the two components.

### Issue 5. The critical value may be undefined or ambiguous on the real graphs, and its estimator is uncalibrated

- **LOCATION / TARGET:** 4.2 (`t_c`: "the `t` at which spread stops being local"), 5.1, 5.3, O4.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Inference:* `t_c` exists only if the graph at `t = 1` has one component holding most nodes.
    A settlement cut by a rail line, river, or wide road may split into several large pieces at
    the chosen `d`. The suburb, with streets between blocks, may do the same.
  - *Inference:* in a patchy graph the second-largest-cluster curve can have several peaks: one
    when each dense patch connects internally, another when patches join. "The" critical value
    then depends on which peak the estimator picks.
  - No step in v7 tests the estimator where the answer is known.
- **EVIDENCE NEEDED:** component sizes of each real graph at `t = 1`; the shape of the
  second-largest-cluster curve on a deliberately patchy synthetic pattern.
- **RECOMMENDED ACTION:**
  1. Add a gate criterion in 5.2: the largest component at `t = 1` must hold a stated share of
     core nodes (I suggest 90%), otherwise `t_c` is reported per component or the target mean
     degree is raised.
  2. Calibrate the estimator in the pilot on a four-neighbour square lattice, where the bond
     threshold is exactly 1/2 **[background; this value is standard, but cite a source]**, at
     the real sites' node counts. The gap from 1/2 is the estimator's size bias.
  3. Fix a rule for multiple peaks before real data is used.
  4. Note for feasibility: the rule in A3 is equivalent to keeping each link with probability
     `t` and taking the components that contain seeds. `t_c` can therefore be estimated without
     simulating cascades or choosing `p`, by adding links one at a time and tracking component
     sizes with union-find. This is far cheaper than a `t` sweep.

### Issue 6. The data gate assumes three independent footprint sources at both sites

- **LOCATION / TARGET:** 5.2 source rule, A10 first bullet, O5.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Missing evidence:* v7 does not show that Google Open Buildings covers the United States.
    **[background, unverified]** my recollection is that it does not. If so, the suburb has two
    sources, and the "agreement with a third source" tie-break in O5 cannot work there.
  - *Missing evidence:* in many US areas, OSM buildings were bulk-imported from Microsoft
    footprints or county data **[background, unverified for any specific metro]**. Close
    agreement between OSM and Microsoft would then show common origin, not accuracy.
  - *Evidence from v7:* machine-derived footprints are weakest in dense contiguous settlement.
    A 30% count difference in Kibera may come from merged or split polygons, so counts compare
    differently defined objects.
  - The spot check tests whether sampled polygons are real. It cannot detect omitted buildings,
    as v7 notes.
- **EVIDENCE NEEDED:** coverage of each source at each site; the import history of OSM buildings
  in the chosen metro (OSM changeset tags or the import catalogue).
- **RECOMMENDED ACTION:**
  1. Check coverage and import provenance before nominating suburbs, since it may affect the
     choice of metro.
  2. Add an omission check to the spot check: sample 50 random points on imagery and record
     whether a visible building there has a footprint.
  3. At Kibera, compare total built area as well as counts, which is less sensitive to merging.

### Issue 7. The synthetic gradient may not reach either real site

- **LOCATION / TARGET:** 5.1, 5.4, H1-G, fallback form in 5.7.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Evidence from v7:* the gradient runs from a regular lattice to uniform random points. No
    measurement places either site on it (section 2, "not supported").
  - *Reviewer conjecture, untested:* real building patterns vary on several axes at once.
    Buildings cannot overlap, which makes centroids more evenly spaced than random at short
    range. Settlements are also patchy at longer range, and suburbs are laid out in rows. A
    one-axis gradient ending at uniform random may contain neither pattern.
  - *Consequence:* under the fallback, both sites could be labelled "extrapolation", and the
    learning component would then have no in-range test case at all.
- **EVIDENCE NEEDED:** the 5.5 metrics for both sites and for the gradient endpoints.
- **RECOMMENDED ACTION:**
  1. Compute the 5.5 metrics for both sites at the gate, before building the gradient.
  2. If either site falls outside, extend the gradient past uniform random with a clustered
     pattern, and add a minimum-spacing pattern. Decision D4 already places synthetic
     benchmarks in scope, so this is not a scope expansion in my reading; the human may differ.
  3. Replace "place both sites on it" with a statement of which metric defines position, since a
     site can sit at different positions on different metrics.

### Issue 8. Edge handling is specified for local spread on the observed graph only

- **LOCATION / TARGET:** 5.8; section 6 ("estimating `t_c` requires runs at and above
  threshold").
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Inference:* nodes at the outer rim of the buffer have fewer neighbours than they would in
    the full city. If `d` is calibrated on all nodes, the core's mean degree overshoots the
    target.
  - *Inference:* a rewired graph has no spatial boundary. Its links join core nodes to buffer
    nodes anywhere. "Buffer carries propagation, core is measured" therefore means something
    different for the observed and rewired graphs, and the difference enters `R` and `Q`.
  - v7 names the above-threshold problem and proposes no remedy.
- **EVIDENCE NEEDED:** `Q` for a synthetic pattern with and without a buffer, at the real sites'
  core share.
- **RECOMMENDED ACTION:** calibrate `d` on core nodes only; rewire the whole core-plus-buffer
  graph; compute `t_c` on the whole graph and amplification on core seeds; state these three
  rules in 5.8; set the buffer width as a multiple of `d`.

### Issue 9. The sub-window learning model is unlikely to run or to be informative

- **LOCATION / TARGET:** 5.7 sub-window form, H1-L.
- **SEVERITY:** major for H1-L; minor for the main question.
- **WHY IT MATTERS:**
  - *Inference from issue 2:* the response is per-block `log R` at low `t`, which is forced
    toward zero. What remains is mostly simulation noise plus the block's degree spread.
  - *Inference:* twenty blocks in a suburb of modest size leaves each block with few buildings,
    most of them near a block edge.
  - *Evidence from v7:* the predictors are graph statistics of the same graph that produces the
    response, which v7 calls near-circular.
  - This component is the only one that exercises spatial-leakage control, which
    `inputs/problem.md` lists as a required threat to address.
- **EVIDENCE NEEDED:** node count per block at the gate; variance of per-block `R` on synthetic
  blocks of that size.
- **RECOMMENDED ACTION:**
  1. Add a gate condition on minimum nodes per block, and expect the fallback to run.
  2. If the fallback runs, say in the write-up that the spatial-leakage requirement of the
     problem statement is described but not demonstrated. v7 already commits to this.
  3. Fallback model (O9): make the linear model primary. A tree-based model predicts a constant
     outside its training range, so it cannot give a meaningful value for a site labelled
     extrapolation.

### Issue 10. No evidence gap has been established

- **LOCATION / TARGET:** section 2 ("not supported"), L1, 9 row 11.
- **SEVERITY:** major for any novelty claim; does not block the pilot.
- **WHY IT MATTERS:** percolation and epidemic thresholds on spatial versus rewired networks is
  an established research area. Without a search, the study may repeat a known result at two
  new locations. AGENTS.md rule 9 bars a novelty claim without human approval in any case.
- **EVIDENCE NEEDED:** a targeted search on (a) bond percolation or epidemic thresholds on
  random geometric graphs against degree-preserving nulls, (b) percolation analyses of building
  or street patterns, (c) label-noise propagation in relational classifiers.
- **RECOMMENDED ACTION:** run the search before packet 03 drafts an abstract. Until then the
  abstract should describe the work as an application and replication, with no novelty wording.

### Issue 11. The result is about a cascade model, and the write-up must not drift from that

- **LOCATION / TARGET:** 4.1 wording ("classification errors"), A4, section 12.
- **SEVERITY:** minor, provided A4 is carried into every downstream artifact.
- **WHY IT MATTERS:**
  - No classifier appears anywhere in the design. What spreads is a flag under a fixed rule.
  - *Evidence from v7:* the one documented scoring method averages over neighbours, which v7
    says behaves in the opposite way as degree grows. The conclusions may reverse under that
    rule.
  - The per-link probability `t` is the same at both sites, although a link spans a very
    different physical distance at each. The human accepted this as descriptive (decision 2).
    It still limits what "the same error process" means across sites.
- **EVIDENCE NEEDED:** none for this study; the averaging rule is recorded as future work.
- **RECOMMENDED ACTION:** packets 03 and 04 should use "injected label errors under an
  independent-cascade rule" and repeat A4. No claim about any deployed system, policy
  relevance, or external validity without human approval (AGENTS.md rule 9).

### Issue 12. Smaller points

- **SEVERITY:** minor (all).
- **Margin.** The 1.25 ratio has no stated basis in evidence or in a consequence. That is
  acceptable for a pre-registered threshold, but it should be described as a convention.
- **Direction.** `inputs/problem.md` expects dense irregular layouts to amplify. H1-C and H1-T
  are two-sided. State which sign of `C` and `D` corresponds to the original expectation, so a
  result in the other direction is reported as contrary to it.
- **Suburb rule.** Picking the most grid-like of five maximises the contrast with Kibera. That
  fits A6 (two cases), and it should be stated that the suburb is an extreme case by design.
- **Clustering coefficient as a metric (5.5).** For distance-band graphs on unstructured points
  it is nearly constant **[background, unverified]**, so it may not discriminate along the
  random end of the gradient.
- **Rewired fragmentation (O3).** If rewired graphs are allowed to fragment, their `t_c` is not
  comparable with a connected observed graph. Decide this before the estimator is fixed.
- **Section 1 of v7** cites "human decisions 1 to 4"; it should read 1 to 6.

## 6. Required checks

| Check | Answer |
|---|---|
| What would falsify the claim? | For the cross-site claim: both `C` and `D` inside the margin on the spatial-variability interval, with sites at similar gradient positions (v7 section 7). I add: a contrast that disappears when the sites are compared at matched node count (issue 1). The within-site claim (`Q ≠ 1`) is not realistically falsifiable and is rightly an effect size |
| Is the spatial and temporal scale aligned? | Partly. Building-level nodes fit the question. Matching on ground extent misaligns graph size across sites (issue 1). Time is a single snapshot per source; source dates may differ across sites (v7 section 6) |
| Could spatial autocorrelation or leakage inflate performance? | Only in the learning component. The sub-window form addresses it with blocked folds but is unlikely to run (issue 9). The fallback does not exercise it. The main estimands are simulation outputs, not predictions |
| Are causal claims justified? | Along the synthetic gradient, a controlled-manipulation claim within the model is justified. Between the two sites, no. v7 says so (A6) |
| Does the evidence discriminate among alternatives? | Not yet between arrangement and graph size (issue 1), or arrangement and spatial concentration of density (issue 4). Node definition and source are handled by sensitivity runs |
| Is a second dataset or independent benchmark available? | Three useful ones, none yet in the design: the exact lattice threshold for calibrating the estimator (issue 5); the random-graph threshold formula for checking the rewired ensemble (issue 2); a second footprint source at Kibera. At the suburb, source independence is unverified (issue 6) |
| Is the question answerable with the proposed data? | The gradient half: yes, with no real data. The cross-site half: possibly, and only if the node counts support a stable `t_c`, which is unknown until the gate |

## 7. Unresolved issues

- **U1.** Whether "comparable extent" should become "comparable node count" (issue 1). Human
  ruling needed; it reopens one clause of decision 5.
- **U2.** Which interval drives the decision table (issue 3). Human ruling needed.
- **U3.** Whether a null that preserves degree-degree linking is in scope (issue 4).
- **U4.** Whether extending the gradient beyond uniform random counts as scope expansion
  (issue 7).
- **U5.** Every **[background, unverified]** statement in this review. The most consequential
  are the two size-scaling laws in issue 1, the rewired threshold formula in issue 2, and the
  coverage of Google Open Buildings in issue 6.
- **U6.** Node counts, site extents, and arrangement metrics at both sites. Nothing has been
  measured.
- **U7.** I did not verify any citation or page reference in v7.

## 8. Suggested next scientific action

1. **Human rulings on U1 and U2.** Both change what the pilot must fix.
2. **Verify the background statements in U5** against sources, and add those sources to
   `inputs/evidence/` with notes. A single network-percolation review would cover most of them.
3. **Run the gate's count step early** (building counts and extents only, no propagation), so
   the pilot can be run at the real node counts.
4. **Synthetic pilot (5.1), extended with four checks:** estimator calibration on the
   four-neighbour lattice; rewired threshold against the formula; `Q` against node count for a
   fixed pattern; `Q` with and without a buffer.
5. **Check footprint coverage and OSM import provenance** before nominating suburbs.
6. **Literature search** (issue 10) before packet 03 is run.

## 9. Verdict

**REVISE.**

The question in section 4.1 is well posed, falsifiable in its cross-site half, and more
carefully pre-specified than is usual at this stage; I recommend keeping it as worded. The
revision concerns measurement. Two gaps would let the study return a confident answer for the
wrong reason: the sites are matched on area, so the critical-value contrast is exposed to a
graph-size effect that may be of the same order as the 1.25 margin (issue 1), and the decision
table does not say which interval it is judged on, which determines whether the study can ever
be conclusive or ever be inconclusive (issue 3). Three further points weaken interpretation and
are cheap to address: the low-`t` amplification contrast is largely determined by the degree
sequence and should not count as corroboration (issue 2), rewiring leaves the spatial
concentration of density inside the "arrangement" effect (issue 4), and the critical value needs
an existence check and a calibration against a known answer (issue 5). None of this requires
real data, and most of it fits inside the synthetic pilot already planned. The data gate needs
its source assumptions checked for the suburb (issue 6). No novelty, causal, policy, or
external-validity claim is supported at present, and several of my own technical statements
rest on unverified background knowledge that must be sourced before the design relies on them.


## FILE: outputs/abstract_v1.md



# EXECUTION REQUIREMENT

Work only from the supplied state and clearly label missing evidence.
Return the requested structured artifact. Do not silently change the research question.
