# AGENT ROLE

# 🧪 Scientific Critic Agent

## Role
You are a skeptical scientific reviewer, not a copy editor.

## Goal
Challenge questions, hypotheses, methods, and claims using scientific criteria.

## Review dimensions
Score 1–5:
- importance;
- novelty/evidence gap;
- testability;
- identification strategy;
- data adequacy;
- spatial validity;
- robustness/validation;
- feasibility.

## Required checks
Ask:
- What evidence would falsify the claim?
- Is the spatial/temporal scale aligned with the question?
- Could spatial autocorrelation or leakage inflate performance?
- Are causal claims justified?
- Does the proposed evidence discriminate among alternative explanations?
- Is a second dataset or independent benchmark available?
- Is the question answerable with the proposed data?

## Output schema
For each issue:
- ISSUE
- LOCATION / TARGET
- SEVERITY: minor | major | blocking
- WHY IT MATTERS
- EVIDENCE NEEDED
- RECOMMENDED ACTION

End with: ACCEPT / REVISE / REJECT and a one-paragraph rationale.


# CURRENT TASK

Critique the methodology as a scientific reviewer, emphasizing spatial validity and falsifiability.

# CURRENT RESEARCH STATE / EVIDENCE

## FILE: outputs/methodology_v1.md



## FILE: outputs/research_questions.md

# Research question and hypotheses (v7, frozen pending independent review)

Stage: 01 Research Question Agent (revision)
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous versions: `outputs/history/research_questions_v1.md` to `_v6.md`
Status: **frozen** by human decision 6. No further rulings are requested in this document.
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

Standing effects of these decisions:

- **The research question, estimands, hypotheses, margins, and decision rule are frozen** as of
  v7. Points that decision 6 leaves operationally open are recorded in section 10 as stated agent
  defaults or as values the pilot fixes. They are not requests for rulings. The independent
  review may reopen any of them.
- Decision 3 supersedes decision 2 on one point: the mean-degree-matched comparison is the middle
  step of the decomposition, not a co-primary test.
- Decisions 4, 5 and 6 confirm the agent's readings recorded in v4, v5 and v6.
- Decision 5 supersedes decision 4 on one point: estimated critical `t` is co-primary, not
  secondary.
- Decision 5 fixes the co-primary estimands **before the pilot**. The pilot can set operating
  values (which `t`, how many swaps). It cannot change which estimands are primary.
- **Scope is frozen** (decision 4). The fallback in decision 5 replaces the sub-window model when
  that model cannot run; it is not an addition. New concerns are recorded as threats, unresolved
  issues, or future work.

Changes in v7: decision 6 applied; assumptions A9 and A10; margin for the critical-value contrast
(4.2, 4.3); decision table marked approved (4.4); uncertainty method (5.1, 5.8); data-gate rule
and suburb nomination (5.2); model types and fallback target (5.7); threats; section 10 rewritten
as defaults and pilot values.

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
  system in the evidence (Macskassy & Provost 2005) averages scores over a node's neighbours,
  where the cascade gives each link an independent chance to transmit. Results under A3 cannot be
  read as results for that system. The human has revised the README to say this, and has ruled
  that the difference is recorded as future work only (section 12).
- **A5. Density is a confound and includes the whole degree distribution** (decisions 1, 2, D6).
  Arrangement is who connects to whom at fixed degrees. Local packing unevenness is density by
  definition.
- **A6. Two sites are two cases**, not a sample of layout types.
- **A7. Arrangement effect** is the ratio of amplification factors, observed over rewired
  (confirmed in decision 4).
- **A8. Readings of decision 4, confirmed by decision 5.**
  - "All compared graphs" means every graph entering a primary comparison: both real sites, their
    rewired ensembles, and the synthetic reference patterns with theirs.
  - The critical-`t` estimation method and the rule for placing `t` are fixed on synthetic
    patterns. The real graphs' critical values are then estimated by that method before any
    real-site amplification is examined.
  - The 1.25 margin applies to the cross-site ratio of arrangement effects: of interest at 1.25
    or above, or 0.80 or below.
- **A9. Readings of decision 5, confirmed by decision 6.**
  - Raw critical `t` is reported for every graph. The cross-site comparison uses each site's
    observed-to-rewired ratio of critical `t`.
  - A spatial block is the unit of the sub-window model: one block, one graph, one arrangement
    effect. Twenty per site gives 40 observations.
  - Typical cascade extent is measured by a method fixed in the pilot.
- **A10. Agent operational readings of decision 6.** Stated so the work can proceed; not requests
  for rulings.
  - *"Better-matching source"* is the source whose building count agrees most closely with a
    third source at that site. With three sources, the two that agree identify the outlier.
  - *"Passes a visual spot check"* needs a pass level. Agent default: at least 45 of the 50
    sampled footprints correspond to one real structure in current imagery. The level is written
    into the gate protocol before the sample is drawn.
  - *"Within the synthetic gradient's range"* means every arrangement metric of the real site
    lies between the minimum and maximum of that metric across the synthetic patterns.
  - *"Linear or spatial regression"*: ordinary least squares first; a spatial lag or error model
    from `spreg` if the residuals show spatial autocorrelation.
  - *"Stop"* means the study does not proceed at that site with any footprint source.

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
  Reported for every graph. Co-primary estimand (decision 5).
- **Critical-value ratio** `Q(G) = t_c(G) / mean t_c(rewired G)`. `Q > 1` means arrangement delays
  the onset of extensive spread relative to arbitrary wiring at the same degrees.
- **Cross-site critical-value contrast** `D = log Q(Kibera) − log Q(suburb)`. Minimum contrast of
  interest: `|D| ≥ log 1.25`, that is a cross-site ratio of `Q` at or above 1.25 or at or below
  0.80 (decision 6).

### 4.3 Hypotheses

Fixed before the pilot (decisions 5 and 6).

- **H1-C (co-primary, amplification contrast).** `|C(t, p)| ≥ log 1.25` at the pre-specified
  sub-critical `t` and `p`. Two-sided.
- **H1-T (co-primary, critical-value contrast).** `|D| ≥ log 1.25`. Two-sided. Judged with its
  interval (5.8).
- **H1-R (co-primary, effect size).** `R` and `Q` are reported per site with intervals, as effect
  sizes and not pass/fail tests. Expected `R < 1` and `Q > 1` by analogy with lattice results;
  both expectations are checked on the synthetic patterns before real data is used.
- **H1-G (supporting, synthetic gradient).** Along a regular-to-irregular gradient of synthetic
  patterns at fixed intensity and mean degree, `R` and `Q` change monotonically with
  irregularity. This is the only place arrangement is manipulated directly.
- **H1-L (supporting, learning component).** One of two forms, decided at the gate (5.7):
  - *Sub-window form.* In a linear or spatial regression, arrangement metrics predict per-block
    arrangement effects better than a site-mean baseline under 5-fold spatially blocked
    cross-validation.
  - *Fallback form.* A model trained on the synthetic gradient predicts the two real sites'
    critical-value ratio `Q` from their arrangement metrics.
- **Competing explanation.** A cross-site contrast is produced by node definition, mapping
  completeness, site size, or the two sites' differing degree sequences, not by arrangement.

### 4.4 Decision rule for the two contrasts

**Approved by decision 6.** Having two co-primary contrasts gives two chances to find an effect,
so their joint reading is fixed in advance.

| H1-C (amplification, sub-critical) | H1-T (critical value) | Reading |
|---|---|---|
| Meets margin | Meets margin, same direction | Supports the hypothesis within this model |
| Below margin | Meets margin | Supports the hypothesis for the onset of extensive spread only. Expected to be the common case, because `R` is forced toward 1 at low `t` |
| Meets margin | Below margin | Not treated as support. Check whether `C` tracks the distance of `t` from each rewired ensemble's threshold (section 6) |
| Meets margin | Meets margin, opposite direction | Reported as conflicting; no support claimed |
| Below margin | Below margin | Refuted within this model, subject to section 7 |

"Same direction" means the site whose arrangement contains spread more (`R` lower) is also the one
whose arrangement delays onset more (`Q` higher).

A contrast "meets margin" when its interval lies wholly at or beyond the margin, and is "below
margin" when its interval lies wholly inside it. If an interval straddles the margin, that
contrast is **inconclusive**, and so is any row that depends on it. An inconclusive result is an
acceptable outcome of this study (decision 6) and is reported as such, not as support or
refutation. The interval-based wording is the agent's operational reading of the approved table.

### 4.5 Three-step decomposition (decision D6)

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
  lowest `t_c`, the `p` values, the rewiring settings, the interval level, and the method for
  measuring typical cascade extent.
- Uncertainty in `t_c` and `Q` (decision 6): many realisations, plus a bootstrap over spatial
  blocks. The pilot tests the block bootstrap on synthetic patterns, where the answer from many
  large independent realisations is available for comparison, and fixes block size and the number
  of resamples.
- The pilot sets operating values only. The co-primary estimands and the decision rule in 4.4 are
  already fixed and are not revisited in light of pilot results (decision 5).
- Check that the rewired ensemble is fully mixed. An edge-crossing swap can leave spatially
  correlated edge pairs under partial rewiring (Iotti et al. p. 3); `networkx.double_edge_swap`
  is an edge-crossing swap.
- Record which graph has the lowest `t_c`. This is not assumed.

### 5.2 Pre-analysis gate: node definition and data adequacy (required)

- Download OSM building footprints for each study area plus a buffer; record the snapshot date.
- Tabulate building counts, footprint areas, nearest-neighbour distances, and site extents.
- **Source rule (decision 6).** Compare OSM building counts with Microsoft and Google footprints
  at each site. If OSM differs from either by more than 30%, switch that site to the
  better-matching source. Each source used must pass a visual spot check of 50 randomly sampled
  buildings. Stop only if no source passes.
- Record, per site, which source was used, the three counts, the spot-check tallies, and the
  source dates.
- Fix the node definition: minimum footprint area and whether touching polygons are merged.
  Centroid distance and a minimum-area filter both have precedent (Behnisch et al. p. 2).
- **Suburb nomination and selection (decisions 5 and 6).**
  1. Nominate 5 planned suburban neighbourhoods in one metro, of comparable extent to the Kibera
     study area, each with a one-line justification.
  2. **Human approval of the five, before any indicator is computed.**
  3. For each, compute orientation order and dead-end share on its street network with OSMnx
     (indicators as defined in Boeing 2019, pp. 5–6).
  4. Rank by orientation order descending and by dead-end share ascending; select the lowest
     summed rank; break ties by orientation order. City-wide values are not used.
- The rule uses street indicators only and no outcome of the study, so it cannot be tuned toward
  a result. It does not guarantee a regular *building* pattern; that is measured afterwards (5.5).
- Count the spatial blocks each site supports and decide which form of the learning component
  runs (5.7).

### 5.3 Graphs

- Project to local UTM (EPSG:32737 for Nairobi; the suburb's zone once chosen).
- Distance-band graphs via libpysal, converted to NetworkX, at the common target mean degree.
- Report the `d` each site needs and what it physically spans, as a descriptive result.
- Rewired ensembles per graph, using the settings fixed in 5.1.
- Estimate each real graph's `t_c` and `Q` by the fixed method; confirm the pre-specified `t`
  values lie below the lowest `t_c`. Only then examine amplification.

### 5.4 Reference patterns and gradient (decisions D2, D4)

- The synthetic patterns of 5.1, at the real sites' intensity and mean degree, each with `R`,
  `t_c`, and `Q`.
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

### 5.7 Learning component (decisions D5, 4, 5)

Exactly one of the two forms runs. The choice is made at the gate from measured extents, before
any propagation result on real data is seen.

**Condition.** The sub-window form runs only if **each site** yields at least 20 spatial blocks,
each wider than the typical cascade extent. If either site falls short, the fallback runs.

**Sub-window form**

- Unit: a spatial block with its own graph, rewired ensemble, and arrangement effect `R`.
- Response: log `R` per block. Predictors: the 5.5 metrics.
- Model: linear regression, or a spatial lag or error model if residuals are spatially
  autocorrelated, against a site-mean baseline. No tree-based model, given 40 observations
  (decision 6).
- Validation: 5-fold spatially blocked cross-validation. Roberts et al. (pp. 918–920) require
  blocks larger than the residual autocorrelation range; the cascade-extent condition is the
  project's stand-in for that and should be checked against residual autocorrelation after
  fitting.
- Leave-one-site-out reported separately, labelled as extrapolation.

**Fallback form**

- Training data: synthetic gradient patterns, each with its arrangement metrics and estimated
  critical-value ratio `Q`.
- Model: one regressor predicting `Q` from the 5.5 metrics. Decision 6 rules out a tree-based
  model for the sub-window form only; here the training set is synthetic and can be made as large
  as needed, so the tree-based model stated in v6 is kept, with a linear model alongside for
  comparison.
- Test: the two real sites. A site's prediction counts as in-sample only if its arrangement
  metrics fall within the synthetic gradient's range; otherwise it is reported as extrapolation
  (decision 6).
- **This form does not exercise spatial-leakage control** (decision 5). That must be stated in
  the write-up, together with the fact that review issue 10 is then only partly addressed.

### 5.8 Edge handling and variability

- Buffer nodes carry propagation; estimands are computed on core nodes; the core share is reported.
- Monte Carlo intervals give precision. Spread across spatial blocks within each site gives
  variability, if extents allow blocks at all. No significance test is reported for the site term.
- Intervals for `t_c`, `Q`, and `D` come from many realisations and a bootstrap over spatial
  blocks (decision 6), with settings fixed in the pilot.

## 6. Threats to validity

- **The arrangement effect is forced toward 1 at low `t`.** *Agent derivation; to be checked in
  the pilot.* To first order in `t`, the expected number flagged per seed is `t` times the mean
  degree on any graph, whatever its arrangement. So `R` tends to 1 and `C` to 0 as `t` falls.
  Decision 5 answers this by making critical `t` co-primary; H1-C alone being null is not read as
  refutation (4.4).
- **The amplification contrast may reflect how close `t` is to each null's threshold.** *Agent
  inference.* The denominator of `R` depends on the rewired graph's degree sequence. If one
  site's rewired ensemble is nearer its own threshold at the chosen `t`, its `R` is pulled down
  for a reason that is density by definition. This is why H1-C without H1-T is not treated as
  support (4.4).
- **`Q` is a better normalisation but not a proven one.** *Agent inference.* Dividing by the
  rewired critical value removes what the degree sequence alone determines. Whether arrangement
  and degree heterogeneity combine multiplicatively in `t_c` is not established, so `Q` may still
  carry some density signal. The synthetic gradient is the available check.
- **The co-primary critical value is the noisiest quantity in the study.** Both sites are small,
  and critical values estimated on small areas are noisy (Behnisch et al. p. 4; Fagundes et al.
  p. 1). Rewired graphs may also have less sharply defined transitions at this size. If the
  uncertainty in `D` is wide, H1-T will be inconclusive. Since 4.4 does not count H1-C alone as
  support, the study as a whole would then be inconclusive.
- **Estimating `t_c` requires runs at and above threshold**, where spread reaches the buffer and
  the edge of the study area. Edge handling in 5.8 was designed for local spread.
- **A block bootstrap of a whole-graph quantity is not straightforward.** *Agent inference.*
  Critical `t` belongs to a connected graph. Resampling blocks cuts the links that cross block
  boundaries, and a critical value estimated on a small block is shifted relative to the whole
  site. The intervals may therefore be biased or too wide. The pilot checks this on synthetic
  patterns; if the bootstrap behaves poorly there, that is reported as a limitation of the
  intervals, and a wide interval leads to an inconclusive result under 4.4.
- **The source rule can leave the two sites on different footprint sources.** Node comparability
  across sites is then weaker, and source dates may differ.
- **A count difference does not say which source is wrong.** Machine-derived footprints are least
  certain in dense contiguous settlement (Sirko et al. pp. 1, 4), where they may merge structures.
  In Kibera a large difference could therefore reflect the comparison data. The 50-building spot
  check is the safeguard; it tests whether sampled footprints are real, and is less able to detect
  buildings that a source omits.
- **Sub-window form: 40 observations.** Cross-validated skill will have high variance even with a
  linear model, and a null result will be weak evidence. Per-block `R` is noisier than site-level
  `R`. The arrangement metrics may be strongly correlated with each other.
- **Fallback form: two test cases.** Two predictions give two errors, not an estimate of skill.
  A site inside each metric's range can still lie outside the region the synthetic patterns
  jointly cover, so "in-sample" by the range rule is a weaker statement than it sounds. Training
  rows from the same gradient step are not independent of each other.
- **Suburb rule.** Nomination of the five is a judgement, made visible by the one-line
  justifications and the approval step. The ranking uses street layout, which need not track
  building arrangement.
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
- **Sub-window form, additionally:** ecological fallacy; leakage between neighbouring blocks;
  near-circular prediction of one graph statistic from others of the same graph.

## 7. What would weaken or refute the hypothesis

- **Refuted within this model:** both contrasts fall below their margins (last row of 4.4),
  **and** the two sites sit at similar positions on the synthetic gradient. If an interval
  straddles its margin, the result is inconclusive, not a refutation.
- **Not attributable to arrangement:** a contrast changes sign across node definitions, or `C`
  tracks the distance of `t` from each rewired ensemble's threshold.
- **H1-G fails:** neither `R` nor `Q` varies with irregularity along the synthetic gradient. This
  undercuts the mechanism even if the real sites differ.
- **Step 2 differs but step 3 does not:** the site difference is carried by degree distribution,
  which is density by definition.
- **H1-L, sub-window form, fails:** no skill over the site-mean baseline under blocked
  cross-validation, or skill that collapses relative to random folds.
- **H1-L, fallback form, fails:** the model's errors on the two real sites are no smaller than
  those of a baseline that predicts the mean `Q` of the synthetic patterns. With two cases this
  is weak evidence in either direction, and weaker still for a site reported as extrapolation.

## 8. Disposition of earlier candidates

| v3 candidate | Status |
|---|---|
| RQ1 | The approved question (section 4) |
| RQ2 threshold sensitivity | Absorbed as 5.6 |
| RQ3 structural predictors | Reduced; learning component 5.7, sub-window or fallback form |
| RQ4 clustered seeding | Dropped |
| RQ5 footprint robustness | Pre-analysis gate 5.2 |

## 9. Response to the review

| Review issue | Resolution |
|---|---|
| 1. Deterministic rule; slope estimand undefined | Probabilistic rule; amplification factor at pre-specified sub-critical `t` and `p`, with critical `t` co-primary |
| 2. Rewiring test passes for any spatial layout | Effect size, plus spatial reference patterns. Direction now treated as open and checked in the pilot |
| 3. Decision 2 may control away irregularity | Accepted with the three-step decomposition |
| 4. One pair of sites | Synthetic gradient; within-site blocks only if each site yields 20. Partly addressed |
| 5. Irregularity unmeasured | Metrics named (5.5); suburb chosen by a fixed ranking rule on measured indicators. Not yet measured |
| 6. Contrast depends on scale | Ratio scale; margin fixed at 1.25 |
| 7. Replicates are not places | Monte Carlo intervals for precision; block spread for variability where possible; no significance test for the site term |
| 8. Node comparability | Pre-analysis gate, with precedent for the filter |
| 9. Edge handling | Section 5.8. Simplified by the sub-critical restriction, since spread stays local |
| 10. No learning component; leakage unaddressed | A learning component always runs. Leakage control is exercised only in the sub-window form; under the fallback this is stated as not addressed |
| 11. No evidence gap established | Still open. The spatial-versus-rewired comparison is established and must not be presented as a finding |

## 10. Unresolved issues

The question is frozen (decision 6). Nothing here requests a ruling. The independent reviewer
should treat every item as open to challenge.

Resolved by decision 6: the margin for the critical-value contrast; the decision table; the
uncertainty method; the fallback target; the sub-window model type; suburb nomination and
tie-breaking; the data-gate rule.

**Operating values the pilot fixes, before any real data is touched**

- **O1. Placement of `t` within the sub-critical range**, for example as fixed fractions of the
  lowest `t_c`.
- **O2. Target mean degree.** A square lattice under a distance rule only admits certain degrees
  (4, 8, 12), and a uniform random pattern needs a mean degree above about 4.5 to hold together
  (agent arithmetic from Barthélemy p. 43). One prior study used 8 (Iotti et al. p. 4).
- **O3. Rewiring details.** Whether rewired graphs may fragment, number of swaps, ensemble size,
  and the mixing check.
- **O4. `t_c` estimator, interval level, block size and resample count for the bootstrap, and the
  cascade-extent measure.**

**Agent defaults in force unless the review changes them** (assumption A10)

- **O5.** "Better-matching source" identified by agreement with a third source.
- **O6.** Spot-check pass level of 45 of 50, written into the gate protocol before sampling.
- **O7.** "Within range" judged metric by metric.
- **O8.** Interval-based reading of "meets margin", "below margin", and "inconclusive" (4.4).
- **O9.** Fallback model type: tree-based, with a linear model for comparison (5.7).

**Standing limitations**

- **L1. Evidence.** Eleven papers read, several in part; eleven listed items unread. No
  literature search. No novelty, causal, policy, or external-validity claim without human
  approval.
- **L2. Independent review.** Outstanding. The existing review was written by the same model in
  the same conversation as the question, and predates decisions 3 to 6.

## 11. Suggested next scientific action

1. **Independent review** of this v7 file in a fresh conversation, as decision 6 anticipates.
2. **Synthetic pilot (5.1)**, which fixes O1 to O4 and checks the expectations `R < 1` and
   `Q > 1`. It uses no real data and does not depend on the review.
3. **Nominate 5 suburb candidates** with one-line justifications, for human approval, before any
   indicator is computed (5.2).
4. **Pre-analysis gate (5.2).**
5. Regenerate packets 03 and 04 so they embed this v7 file and the review. The orchestrator does
   not embed `outputs/evidence_notes.md`.

## 12. Future work (recorded, out of scope)

- **Averaging versus cascade propagation** (decision 5). The one documented association-scoring
  method in the evidence averages over neighbours, which dilutes a single erroneous neighbour as
  degree grows; the cascade used here does the opposite. Repeating the analysis under an
  averaging rule would show whether the conclusions depend on that choice.
- **The sub-window model**, if the fallback runs in its place.
- **Spatial-leakage control**, if the fallback runs: not exercised in this study.


# EXECUTION REQUIREMENT

Work only from the supplied state and clearly label missing evidence.
Return the requested structured artifact. Do not silently change the research question.
