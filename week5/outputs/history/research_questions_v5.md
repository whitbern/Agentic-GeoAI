# Research question and hypotheses (v5)

Stage: 01 Research Question Agent (revision)
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous versions: `outputs/history/research_questions_v1.md` to `_v4.md`
Responds to: `outputs/question_review.md` (verdict REVISE)

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

Standing effects of these decisions:

- Decision 3 supersedes decision 2 on one point: the mean-degree-matched comparison is the middle
  step of the decomposition, not a co-primary test.
- Decision 4 confirms the agent's three readings in v4: no hop limit in the primary rule; the
  arrangement effect is the ratio of amplification factors, observed over rewired; and the
  mean-degree-matched comparison is not co-primary.
- **Scope is frozen.** Nothing below adds a component. New concerns are recorded as threats or
  unresolved issues, to be handled inside the existing runs or stated as limitations.

Changes in v5: decision 4 applied; evidence from five further papers; assumptions A3, A4 and A8;
section 4.2 (margin, critical `t`); section 5 (pilot, `t` grid, rewiring check, conditional
sub-window model); threats; unresolved issues.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` | Broad problem, scope limits, required validity threats |
| `inputs/evidence/README.md` (revised by the human) | Claim-to-citation map, now with a Claim 5 |
| `inputs/evidence/*.pdf` (eleven papers; several read only in part) | See `outputs/evidence_notes.md` for pages read, what each says, and checks on the README summaries |
| `outputs/question_review.md` | Issues 1 to 11 and the REVISE verdict |
| Human decisions 1 to 4 | Quoted above |

The reviewer was not independent of the author (review section 0). An independent rerun of
packet 02 is still outstanding.

## 2. What the evidence supports

Page references are in `outputs/evidence_notes.md`. None of the papers studies label-error
propagation on building-proximity graphs, so every point applies here by analogy.

**Supported**

- **Building-centroid proximity graphs have a precedent**, including a minimum-area filter and a
  check of sensitivity to it (Behnisch et al. 2019).
- **Comparing a proximity graph with a degree-preserving rewired version has a precedent** (Iotti
  et al. 2017). That comparison is established and is not a finding of this project.
- **A per-link transmission rule has a precedent** (Watts & Strogatz 1998) and behaves as a
  percolation process with a threshold (Barthélemy 2011).
- **Proximity graphs are highly clustered**, and uneven point density becomes uneven degree
  (Barthélemy 2011; Iotti et al. 2017).
- **Association-based scoring propagates erroneous seed labels.** One research system, tested on
  synthetic data, degrades sharply when many initial labels are wrong (Macskassy & Provost 2005).
- **Blocked cross-validation is required for spatially structured data** (Roberts et al. 2017).
- **The comparison footprint dataset is weakest in dense contiguous settlement** (Sirko et al.
  2021).
- **Critical values estimated on small areas are noisy** (Behnisch et al. 2019; Fagundes et al.
  2025).

**Mixed or unsettled**

- **Which graph has the lower critical value.** Lattice percolation results say rewiring lowers
  the threshold (Barthélemy). An SIS study on random geometric graphs reports the estimated
  critical value rising under heavy rewiring, while outbreaks become easier (Iotti et al.). The
  v4 expectation that rewired graphs cross first is therefore not assumed here.
- **Whether `R` is below 1.** Expected from the lattice results; not established for a one-shot
  cascade on degree-preserving rewirings of irregular proximity graphs.

**Not supported by anything read**

- That the two candidate sites differ in building arrangement.
- That either site's data is adequate.
- That the simulated rule matches any deployed or published scoring method (see A4).
- That an evidence gap exists. No literature search has been done.

## 3. Assumptions

- **A1. Nodes are buildings**, under a node definition fixed at the pre-analysis gate (5.2).
- **A2. Edges are proximity** within distance `d` in a projected CRS, with `d` set per graph to
  reach a common target mean degree.
- **A3. Propagation rule (decisions D1 and 4).** A fraction `p` of nodes is seeded as erroneous.
  Each flagged node gets one attempt to flag each unflagged neighbour, succeeding with probability
  `t` independently per link. Spread continues until no new node is flagged. No hop limit in the
  primary analysis; a 2-hop limit in the sensitivity analysis.
- **A4. The rule is a stylised abstraction, not a published method.** The one association-scoring
  system in the evidence (Macskassy & Provost 2005) averages scores over a node's neighbours. Under
  that rule one erroneous neighbour matters less as a node's degree grows; under the cascade,
  exposure grows with degree. The two respond to degree in opposite directions, so results under
  A3 cannot be read as results for that system. The revised README states that the paper describes
  "the inference mechanism the propagation rule represents"; the paper does not support that
  wording.
- **A5. Density is a confound and includes the whole degree distribution** (decisions 1, 2, D6).
  Arrangement is who connects to whom at fixed degrees. Local packing unevenness is density by
  definition.
- **A6. Two sites are two cases**, not a sample of layout types.
- **A7. Arrangement effect** is the ratio of amplification factors, observed over rewired
  (confirmed in decision 4).
- **A8. Agent readings of decision 4**, each open to correction:
  - *"All compared graphs"* means every graph entering a primary comparison: both real sites,
    their rewired ensembles, and the synthetic reference patterns with theirs.
  - *"Locate critical `t` on synthetic patterns first"* means the estimation method and the rule
    for placing `t` are fixed on synthetic patterns. The real graphs' critical values are then
    estimated by that fixed method before any real-site amplification is examined.
  - *"Ratio of 1.25 in amplification factor"* is applied to the cross-site ratio of arrangement
    effects: the contrast is of interest if `R(Kibera)/R(suburb)` is at least 1.25 or at most
    0.80. The same 1.25 is used as the yardstick when describing how far any `R` is from 1.

## 4. The research question

### 4.1 Question

With density held fixed, does the arrangement of connections in building-level proximity graphs
change how far injected classification errors spread under probabilistic propagation, and does the
size of that effect differ between Kibera and a planned US suburban area?

This is a narrowed form of the problem in `inputs/problem.md`, narrowed by the human (decisions 1
to 4). It remains a moderation question: arrangement moderates the relationship between injected
error and error reach.

### 4.2 Estimands

Defined for a graph `G`, transmission probability `t`, and seeding rate `p`.

- **Amplification factor** `A(G; t, p)`: expected number of non-seeded nodes flagged per seeded
  node. Primary estimand, at sub-critical `t` only.
- **Arrangement effect** `R(G; t, p) = A(G) / mean A(rewired G)`. `R < 1` means arrangement
  contains error relative to arbitrary wiring at the same degrees; `R > 1` means it amplifies.
- **Cross-site contrast** `C(t, p) = log R(Kibera) − log R(suburb)`. Minimum contrast of interest:
  `|C| ≥ log 1.25` (decision 4).
- **Critical transmission probability** `t_c(G)`: the `t` at which spread on `G` stops being local.
  Reported for every graph as a secondary arrangement measure that does not depend on graph size
  in principle (decision 4). The observed-to-rewired ratio of `t_c` is the corresponding
  arrangement measure.

### 4.3 Hypotheses

- **H1-C (co-primary, cross-site contrast).** `|C(t, p)| ≥ log 1.25` at the pre-specified
  sub-critical `t` and `p`. Two-sided: the problem statement expects the irregular site to amplify
  more, but direction is the empirical question.
- **H1-R (co-primary, effect size).** `R` is reported per site with intervals, as an effect size
  and not a pass/fail test. Expected below 1 by analogy with lattice results; this expectation is
  itself checked on the synthetic patterns before real data is used.
- **H1-T (secondary, critical values).** The observed-to-rewired ratio of `t_c` differs between
  the two sites.
- **H1-G (supporting, synthetic gradient).** Along a regular-to-irregular gradient of synthetic
  patterns at fixed intensity and mean degree, `R` changes monotonically with irregularity. This is
  the only place arrangement is manipulated directly.
- **H1-L (supporting, conditional).** Within sites, arrangement metrics predict per-sub-window
  arrangement effects better than a baseline under spatially blocked cross-validation. Tested only
  if section 5.7's condition is met.
- **Competing explanation.** A cross-site contrast is produced by node definition, mapping
  completeness, site size, or the two sites' differing degree sequences, not by arrangement.

### 4.4 Three-step decomposition (decision D6)

1. **Unadjusted.** Amplification at a common metric threshold `d`, density left free.
2. **Mean-degree matched.** Amplification with `d` set per site to a common target mean degree.
3. **Degree-sequence controlled.** The arrangement effects `R` and their contrast `C`.

Step 1 to 2 shows the contribution of uniform density. Step 2 to 3 shows the contribution of
degree heterogeneity, which includes local packing unevenness and is density by definition. Step 3
bears on the hypothesis.

## 5. Design components

### 5.1 Synthetic pilot (first; no real data)

- Patterns: regular lattice, uniform random points, and intermediate steps, at one target mean
  degree.
- For each: the observed graph and its rewired ensemble.
- Sweep `t`; estimate `t_c` for every graph. The peak in second-largest cluster size is one
  documented estimator (Behnisch et al. p. 4, for a distance threshold); using it for `t` is an
  agent inference.
- Fix, before any real data is touched: the `t_c` estimator, the rule for placing `t` below the
  lowest `t_c`, the `p` values, and the rewiring settings.
- Check that the rewired ensemble is fully mixed. An edge-crossing swap can leave spatially
  correlated edge pairs under partial rewiring (Iotti et al. p. 3); `networkx.double_edge_swap`
  is an edge-crossing swap.
- Record which graph has the lowest `t_c`. This is not assumed.

### 5.2 Pre-analysis gate: node definition and data adequacy (required)

- Download OSM building footprints for each study area plus a buffer; record the snapshot date.
- Tabulate building counts, footprint areas, nearest-neighbour distances, and site extents.
- Compare with Google Open Buildings and/or Microsoft footprints. Disagreement in dense contiguous
  settlement is recorded as uncertainty, not as an OSM error.
- Fix the node definition: minimum footprint area and whether touching polygons are merged.
  Centroid distance and a minimum-area filter both have precedent (Behnisch et al. p. 2).
- Select the suburb from **measured neighbourhood indicators: low dead-end share and high
  orientation order** (decision 4), computed with OSMnx on the candidate neighbourhood's street
  network. City-wide values are not sufficient (Boeing 2019, Table 1).
- Decide whether the sub-window model runs (5.7).

### 5.3 Graphs

- Project to local UTM (EPSG:32737 for Nairobi; the suburb's zone once chosen).
- Distance-band graphs via libpysal, converted to NetworkX, at the common target mean degree.
- Report the `d` each site needs and what it physically spans, as a descriptive result.
- Rewired ensembles per graph, using the settings fixed in 5.1.
- Estimate each real graph's `t_c` by the fixed method; confirm the pre-specified `t` values lie
  below the lowest. Only then examine amplification.

### 5.4 Reference patterns and gradient (decisions D2, D4)

- The synthetic patterns of 5.1, at the real sites' intensity and mean degree, each with `R` and
  `t_c`.
- Both real sites are placed on the gradient using the arrangement metrics in 5.5.

### 5.5 Arrangement metrics

Per site, per synthetic pattern, and per sub-window if 5.7 runs:

- coefficient of variation of nearest-neighbour distance;
- a pair-correlation or Ripley's K summary;
- mean clustering coefficient at the target mean degree;
- an orientation-entropy measure adapted from Boeing (2019); the adaptation from street bearings
  to building or local street orientation is an agent proposal.

### 5.6 Sensitivity analyses

- **2-hop-limited propagation** (decision 4).
- Target mean degree; `p`; `t` across the sub-critical grid.
- Node definition; centroid versus footprint-edge distance.
- k-nearest-neighbour graphs (robustness only), with the symmetrisation rule stated.

### 5.7 Sub-window learning component (conditional; decisions D5 and 4)

- **Runs only if** the measured site extents give enough spatial blocks. The minimum number is not
  yet set (U3) and should be fixed before extents are measured.
- If run: sub-windows each with their own graph, rewired ensemble, and `R`; response is log `R`;
  predictors are the 5.5 metrics; one tree-based regressor against a site-mean baseline; spatially
  blocked cross-validation with blocks larger than the residual autocorrelation range (Roberts et
  al. pp. 918–920); leave-one-site-out reported separately as extrapolation.
- **If not run:** H1-L is stated as future work. The study then has no learning component and does
  not exercise spatial leakage control, which reopens review issue 10. This must be said plainly
  in the write-up.

### 5.8 Edge handling and variability

- Buffer nodes carry propagation; estimands are computed on core nodes; the core share is reported.
- Monte Carlo intervals give precision. Spread across spatial blocks within each site gives
  variability, if extents allow blocks at all. No significance test is reported for the site term.

## 6. Threats to validity

- **The arrangement effect is forced toward 1 at low `t`.** *Agent derivation; to be checked in
  the pilot.* To first order in `t`, the expected number flagged per seed is `t` times the mean
  degree on any graph, whatever its arrangement. Arrangement enters only through paths of two or
  more links. So `R` tends to 1 as `t` falls, and `C` tends to 0. Restricting the primary analysis
  to `t` below the lowest critical value keeps estimates size-robust, but it also moves the
  analysis toward the regime where the 1.25 margin is hardest to reach. A null result at very low
  `t` would say little.
- **The contrast may reflect how close `t` is to each null's threshold.** *Agent inference.* The
  denominator of `R` depends on the rewired graph's degree sequence. If one site's rewired
  ensemble is nearer its own threshold at the chosen `t`, its `R` is pulled down for a reason that
  is density by definition, not arrangement. `R` is therefore not a clean normalisation, and `C`
  at a common `t` can differ from zero without any difference in arrangement. H1-T and the
  reporting of `R` across the whole sub-critical grid are the checks available within scope.
- **Critical values are noisy on small graphs.** Both sites are small. `t_c` is size-robust in
  principle but will carry finite-size uncertainty in practice (Behnisch et al. p. 4; Fagundes et
  al. p. 1).
- **Rule realism.** Results describe an independent cascade. The one documented scoring method in
  the evidence works differently (A4).
- **Site confounding.** A non-zero contrast between two places cannot be attributed to layout
  type. The gradient reduces this; it does not remove it.
- **The rewired null is not spatial.** `R` measures spatial arrangement against arbitrary wiring.
  The reference patterns separate irregular from regular.
- **Rewiring bias.** Incomplete mixing would leave spatial correlation in the null (5.1).
- **Node comparability.** A polygon may mean different things at the two sites.
- **MAUP.** Scale form: target mean degree and `d`. Zoning form: node merging, and sub-window size
  and origin if 5.7 runs.
- **Temporal misalignment** between the OSM snapshot and comparison footprints.
- **If 5.7 runs:** ecological fallacy; leakage between neighbouring sub-windows; near-circular
  prediction of one graph statistic from others of the same graph.

## 7. What would weaken or refute the hypothesis

- **Refuted within this model:** `|C| < log 1.25` across the pre-specified `t` and `p`, **and**
  the observed-to-rewired `t_c` ratio does not differ between sites beyond its uncertainty,
  **and** the two sites sit at similar positions on the synthetic gradient. The first condition
  alone is not sufficient, because of the low-`t` threat above.
- **Not attributable to arrangement:** `C` is non-zero but changes sign across node definitions,
  or tracks the distance of `t` from each rewired ensemble's threshold.
- **H1-G fails:** neither `R` nor `t_c` varies with irregularity along the synthetic gradient.
  This undercuts the mechanism even if the real sites differ.
- **Step 2 differs but step 3 does not:** the site difference is carried by degree distribution,
  which is density by definition.
- **H1-L fails (if run):** no skill over the site-mean baseline under blocked cross-validation,
  or skill that collapses relative to random folds.

## 8. Disposition of earlier candidates

| v3 candidate | Status |
|---|---|
| RQ1 | The approved question (section 4) |
| RQ2 threshold sensitivity | Absorbed as 5.6 |
| RQ3 structural predictors | Reduced; conditional component 5.7 |
| RQ4 clustered seeding | Dropped |
| RQ5 footprint robustness | Pre-analysis gate 5.2 |

## 9. Response to the review

| Review issue | Resolution |
|---|---|
| 1. Deterministic rule; slope estimand undefined | Probabilistic rule; amplification factor at pre-specified sub-critical `t` and `p` |
| 2. Rewiring test passes for any spatial layout | Effect size, plus spatial reference patterns. Direction now treated as open and checked in the pilot |
| 3. Decision 2 may control away irregularity | Accepted with the three-step decomposition |
| 4. One pair of sites | Synthetic gradient; within-site sub-windows only if extents allow. Partly addressed |
| 5. Irregularity unmeasured | Metrics named (5.5); suburb chosen on measured indicators. Not yet measured |
| 6. Contrast depends on scale | Ratio scale; margin fixed at 1.25 |
| 7. Replicates are not places | Monte Carlo intervals for precision; block spread for variability where possible; no significance test for the site term |
| 8. Node comparability | Pre-analysis gate, with precedent for the filter |
| 9. Edge handling | Section 5.8. Simplified by the sub-critical restriction, since spread stays local |
| 10. No learning component; leakage unaddressed | Addressed only if 5.7 runs. Otherwise reopened and stated |
| 11. No evidence gap established | Still open. The spatial-versus-rewired comparison is established and must not be presented as a finding |

## 10. Unresolved issues

- **U1. Placement of `t` within the sub-critical range.** Decision 4 sets the ceiling. How far
  below it, and how many values, is open. Too low and `R` is near 1 by construction. One option
  within scope is to state `t` values as fractions of the lowest `t_c`, fixed in the pilot.
- **U2. The 1.25 margin may be unreachable at admissible `t`.** The pilot will show the largest
  `R` departure the synthetic extremes produce below the ceiling. If even lattice versus random
  cannot reach 1.25, the human should decide whether H1-T becomes the main evidence. This is a
  question about interpretation, not a request to add scope.
- **U3. "Enough spatial blocks."** No number is set for the condition in 5.7.
- **U4. Suburb thresholds.** "Low" dead-end share and "high" orientation order need cut-offs or a
  ranking rule among candidate neighbourhoods.
- **U5. Target mean degree.** A square lattice under a distance rule only admits certain degrees
  (4, 8, 12), and a uniform random pattern needs a mean degree above about 4.5 to hold together
  (agent arithmetic from Barthélemy p. 43). One prior study used a mean degree of 8 (Iotti et al.
  p. 4).
- **U6. Gate pass criteria.** What level of footprint disagreement or node-definition sensitivity
  would stop the study or change the site.
- **U7. Rewiring details.** Whether rewired graphs may fragment, number of swaps, ensemble size,
  and the mixing check.
- **U8. README wording on Claim 5.** The statement that Macskassy & Provost describe the mechanism
  the propagation rule represents should be revised by the human; `inputs/` is not edited by
  agents.
- **U9. Evidence.** Eleven papers read, several in part; eleven listed items unread. No literature
  search. No novelty, causal, policy, or external-validity claim without human approval.
- **U10. Independent review.** Outstanding.

## 11. Suggested next scientific action

1. **Synthetic pilot (5.1).** It settles U1, U5 and U7, tests whether `R < 1` and which graph has
   the lowest `t_c`, and shows whether the 1.25 margin is reachable below the ceiling (U2).
2. Human rulings on U3 and U4, before site extents are measured.
3. **Pre-analysis gate (5.2).**
4. Regenerate packets 03 and 04 so they embed this v5 file and the review. The orchestrator does
   not embed `outputs/evidence_notes.md`.
