# Research question and hypotheses (v4, revised after review)

Stage: 01 Research Question Agent (revision)
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous versions: `outputs/history/research_questions_v1.md`, `_v2.md`, `_v3.md`
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

Decision 3 supersedes decision 2 on one point: the cross-site mean-degree-matched comparison
(H1-X) is no longer co-primary. It is the middle step of the decomposition.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` | Broad problem, scope limits, required validity threats |
| `inputs/evidence/README.md` | Human claim-to-citation map |
| `inputs/evidence/*.pdf` (six papers, partly read) | See `outputs/evidence_notes.md` for pages read and what each says |
| `outputs/question_review.md` | Issues 1 to 11 and the REVISE verdict |
| Human decisions 1 to 3 | Quoted above |

The reviewer was not independent of the author (review section 0). An independent rerun of
packet 02 is still outstanding.

## 2. What the evidence now supports

Details and page references are in `outputs/evidence_notes.md`. All of it concerns lattices, model
networks, or city-scale street networks, so each point applies here by analogy.

- **The chosen propagation rule has a precedent.** Watts & Strogatz (1998, p. 441) simulate spread
  with a per-link infection probability and one period of infectiousness.
- **That rule is a percolation process with a threshold.** Barthélemy (2011, p. 91) reports that
  such a model maps to bond percolation: above a critical transmission probability, an extensive
  share of nodes is reached; below it, spread stays local.
- **Spatial graphs contain spread relative to rewired ones.** Shortcuts lower the threshold and
  speed spread (Watts & Strogatz pp. 441–442; Barthélemy pp. 86, 91).
- **Proximity graphs are highly clustered.** A two-dimensional random geometric graph has mean
  clustering near 0.59 at any size (Barthélemy pp. 43–44).
- **Uneven point density becomes uneven degree** (Barthélemy p. 43), which decision 2 counts as
  density.
- **Blocked cross-validation is required for spatially structured data**, with blocks larger than
  the residual autocorrelation range (Roberts et al. 2017, pp. 913, 918–920).
- **The comparison footprint dataset is weakest in dense contiguous settlements** (Sirko et al.
  2021, pp. 1, 4).

Not supported by anything read: that the two candidate sites differ in building arrangement, that
either site's data is adequate, or that an evidence gap exists.

## 3. Assumptions

- **A1. Nodes are buildings**, under a node definition fixed at the pre-analysis gate (section 5.1).
- **A2. Edges are proximity** within distance `d` in a projected CRS, with `d` set per graph to
  reach a common target mean degree.
- **A3. Propagation rule (decision D1, agent's operational reading).** A fraction `p` of nodes is
  seeded as erroneous. Each flagged node gets one attempt to flag each unflagged neighbour, which
  succeeds with probability `t`, independently per link. Spread continues until no new node is
  flagged. This is an independent-cascade process, equivalent to keeping each edge with probability
  `t` and flagging every node connected to a seed. **No hop limit is assumed**, because decision
  D1 does not mention one (U2).
- **A4. The rule is a modelling choice.** No supplied evidence describes how any real
  association-based tool propagates a label.
- **A5. Density is a confound and includes the whole degree distribution** (decisions 1, 2, D6).
  Arrangement is who connects to whom at fixed degrees. Local packing unevenness is density by
  definition.
- **A6. Two sites are two cases.** They are not a sample of layout types.
- **A7. Reading of decision D3.** "Effective neighbourhood size" was defined in the review for a
  deterministic rule. Under the probabilistic rule its counterpart is the amplification factor at
  low `p`. The arrangement effect is therefore taken as the ratio of amplification factors,
  observed over rewired. If the human meant something else, section 4.2 changes.

## 4. The research question

### 4.1 Question

With density held fixed, does the arrangement of connections in building-level proximity graphs
change how far injected classification errors spread under probabilistic propagation, and does the
size of that effect differ between Kibera and a planned US suburban area?

This is a narrowed form of the problem in `inputs/problem.md`, narrowed by the human (decisions
1 to 3). It remains a moderation question: arrangement moderates the relationship between injected
error and error reach.

### 4.2 Estimands

All are defined for a graph `G`, transmission probability `t`, and seeding rate `p`.

- **Amplification factor** `A(G; t, p)`: expected number of non-seeded nodes flagged per seeded
  node. The primary estimand (decision D1).
- **Arrangement effect** `R(G; t, p) = A(G) / mean A(rewired G)`: amplification on the observed
  graph divided by mean amplification over an ensemble of degree-preserving rewirings of that
  graph. `R < 1` means arrangement contains error relative to arbitrary wiring; `R > 1` means it
  amplifies.
- **Cross-site contrast** `C(t, p) = log R(Kibera) − log R(suburb)`: the ratio-scale difference in
  arrangement effects (decision D3). Zero means arrangement matters equally at both sites.

### 4.3 Hypotheses

- **H1-C (co-primary, cross-site contrast).** `C(t, p)` differs from zero by more than a margin
  fixed in advance, at the pre-specified `t` and `p`. Two-sided: the problem statement expects the
  irregular site to amplify more, but direction is the empirical question.
- **H1-R (co-primary, effect size).** `R` is reported for each site with intervals, as an effect
  size and not a pass/fail test (decision D2). *Expectation from the evidence, by analogy:* `R < 1`
  at both sites and for both reference patterns, because spatial graphs contain spread relative to
  rewired ones. An `R` near or above 1 at any real site would be a surprise worth investigating.
- **H1-G (supporting, synthetic gradient).** Along a regular-to-irregular gradient of synthetic
  point patterns at fixed intensity and mean degree, `R` changes monotonically with irregularity.
  This is the only place where arrangement is manipulated directly.
- **H1-L (supporting, learning component).** Within sites, arrangement metrics predict the
  per-sub-window arrangement effect better than a baseline under spatially blocked
  cross-validation (decision D5).
- **Competing explanation.** Any cross-site contrast is produced by node definition, mapping
  completeness, or site size, not by arrangement. Sections 5.1 and 5.5 exist to test this.

### 4.4 Three-step decomposition (decision D6)

Reported in full so that nothing is hidden by the density control:

1. **Unadjusted.** Amplification at a common metric threshold `d`, density left free.
2. **Mean-degree matched.** Amplification with `d` set per site to a common target mean degree
   (the former H1-X).
3. **Degree-sequence controlled.** The arrangement effects `R` and their contrast `C`.

The change from step 1 to 2 is the contribution of uniform density. The change from step 2 to 3
is the contribution of degree heterogeneity, which includes local packing unevenness. Step 3 is
what bears on the hypothesis.

## 5. Design components

### 5.1 Pre-analysis gate: node definition and data adequacy (required)

Nothing downstream runs until this is recorded.

- Download OSM building footprints for each study area plus a buffer; record the snapshot date.
- Tabulate building counts, footprint-area distributions, and nearest-neighbour distance
  distributions per site.
- Compare against Google Open Buildings and/or Microsoft footprints per sub-window. Because the
  comparison data is least certain in dense contiguous settlement (Sirko et al. pp. 1, 4),
  disagreement is recorded as uncertainty, not as an OSM error.
- Fix the node definition: minimum footprint area, and whether touching polygons are merged.
- Fix centroid versus footprint-edge distance.
- Choose the suburban site from measured neighbourhood-level indicators. City-wide figures are not
  enough: Las Vegas and Phoenix are strongly oriented but have high dead-end shares (Boeing 2019,
  Table 1).

### 5.2 Graphs

- Project to local UTM (EPSG:32737 for Nairobi; EPSG:32612 Phoenix or EPSG:32611 Las Vegas).
- Distance-band graphs via libpysal, converted to NetworkX, at a common target mean degree.
- Report the `d` each site needs, and what it physically spans, as a descriptive result
  (decision 2).
- Rewired nulls: an ensemble per graph via `networkx.double_edge_swap`.
- Robustness only: k-nearest-neighbour graphs, with the symmetrisation rule stated.

### 5.3 Reference patterns and gradient (decisions D2, D4)

- Synthetic point patterns at the same intensity and mean degree: a regular lattice, uniform
  random points, and intermediate steps (for example a lattice with increasing random jitter),
  extended past uniform random into clustered patterns if the real sites require it.
- Each pattern gets the same treatment as a real site: observed graph, rewired ensemble, `R`.
- Both real sites are placed on the gradient using the arrangement metrics in 5.4.
- Synthetic patterns are benchmarks, not study areas.

### 5.4 Arrangement metrics

Computed on building footprints per site, per sub-window, and per synthetic pattern:

- coefficient of variation of nearest-neighbour distance;
- a pair-correlation or Ripley's K summary;
- mean clustering coefficient at the target mean degree;
- an orientation-entropy measure adapted from Boeing (2019). Boeing's measure is for street
  bearings; applying it to building or local street orientation is an agent proposal.

### 5.5 Sub-window learning component (decision D5)

- Unit: a spatial sub-window within a site, with its own graph, rewired ensemble, and `R`.
- Response: log arrangement effect per sub-window.
- Predictors: the arrangement metrics in 5.4.
- Model: one tree-based regressor, compared against a baseline of the site mean.
- Validation: spatially blocked cross-validation, blocks larger than the residual autocorrelation
  range (Roberts et al. pp. 918–920). Leave-one-site-out reported separately and labelled as
  extrapolation.
- The spread of `R` across spatial blocks is also the variability measure for the site-level
  results (review issue 7).

### 5.6 Sensitivity analysis (former RQ2)

Target mean degree; `t` and `p` across the pre-specified grid; node definition; centroid versus
edge distance; sub-window size and origin; kNN graphs.

### 5.7 Edge handling

Buffer nodes carry propagation; estimands are computed on core nodes only; the core share of
nodes is reported.

## 6. Threats to validity

- **Threshold behaviour of the estimand.** *Inference from Barthélemy p. 91.* Each graph has a
  critical `t`. Below it amplification is small and does not depend on graph size. Above it,
  amplification at low `p` grows with the number of nodes, so two sites of different size are not
  comparable. Rewired graphs are expected to cross their threshold at lower `t` than observed
  graphs. In the band between the two thresholds `R` will be very small and size-dependent. A `t`
  grid chosen without regard to this could produce a contrast that reflects site size (U1).
- **Site confounding.** A non-zero contrast between two places cannot be attributed to layout
  type. The gradient and the within-site model reduce this; they do not remove it.
- **The rewired null is not spatial.** `R` measures spatial arrangement against arbitrary wiring.
  The reference patterns are what separate irregular from regular.
- **Node comparability.** A polygon may mean different things at the two sites. Addressed by the
  gate, not eliminated.
- **MAUP.** Scale form: target mean degree and `d`. Zoning form: sub-window size and origin, and
  node merging.
- **Ecological fallacy.** Sub-window relationships do not describe individual buildings.
- **Spatial dependence and leakage.** Neighbouring sub-windows share buildings and propagated
  errors. Random folds would inflate skill.
- **Near-circular prediction.** The sub-window model predicts one graph statistic from other
  statistics of the same graph. Skill would show which simple metrics summarise arrangement; it
  would not be independent confirmation of a mechanism.
- **Sample size for the learning component.** The number of usable sub-windows depends on site
  extents, which are unmeasured. It may be too small to support a tree-based model.
- **Temporal misalignment** between the OSM snapshot and comparison footprints.
- **Model realism.** Results describe this propagation model only.

## 7. What would weaken or refute the hypothesis

- **Refuted within this model:** `C` lies inside the pre-set margin across the pre-specified `t`
  and `p`, and inside the spread across spatial blocks; **and** the two sites sit at similar
  positions on the synthetic gradient.
- **Not attributable to arrangement:** `C` is non-zero but changes sign across node definitions or
  disappears when sites are matched on node count.
- **H1-G fails:** `R` does not vary with irregularity along the synthetic gradient. This would
  undercut the mechanism even if the two real sites differ.
- **H1-L fails:** the model does no better than the site-mean baseline under blocked
  cross-validation, or its skill collapses relative to random folds.
- **Step 2 differs but step 3 does not:** the site difference is carried by degree distribution,
  which is density by definition. The hypothesis is not supported.

## 8. Disposition of earlier candidates

| v3 candidate | Status in v4 |
|---|---|
| RQ1 | The approved question (section 4) |
| RQ2 threshold sensitivity | Absorbed as section 5.6 |
| RQ3 structural predictors | Reduced and absorbed as section 5.5 (decision D5) |
| RQ4 clustered seeding | Dropped (decision 3) |
| RQ5 footprint robustness | Promoted to the pre-analysis gate, section 5.1 (decision 3) |

## 9. Response to the review

| Review issue | Resolution |
|---|---|
| 1. Deterministic rule leaves no experiment; slope estimand undefined | Probabilistic rule; amplification factor at pre-specified `t`, `p` (D1) |
| 2. Rewiring test passes for any spatial layout | Reported as effect size; spatial reference patterns added (D2). Direction now supported by evidence |
| 3. Decision 2 may control away irregularity | Accepted with three-step decomposition (D6) |
| 4. One pair of sites | Synthetic gradient (D4) and within-site sub-windows (D5). Partly addressed |
| 5. Irregularity unmeasured | Arrangement metrics named (5.4). Not yet measured |
| 6. Contrast depends on scale | Ratio scale fixed (D3) |
| 7. Replicates are not places | Monte Carlo intervals for precision; spread across spatial blocks for variability; no significance test for the site term |
| 8. Node comparability | Pre-analysis gate (5.1) |
| 9. Edge handling | Section 5.7. Interacts with U1 and U2 |
| 10. No learning component; leakage unaddressed | Section 5.5 (D5) |
| 11. No evidence gap established | Still open. The lattice-versus-rewired result is established and must not be presented as a finding |

## 10. Unresolved issues

- **U1. Where the `t` grid sits relative to each graph's threshold.** Decision D1 calls for
  pre-specified `t` and `p` but the values are not set. Options: restrict the primary analysis to
  `t` below the lowest threshold among all graphs compared, or match node counts across sites and
  windows so amplification is comparable above threshold. The thresholds should be located on
  synthetic patterns first, so the grid is not tuned on the real sites.
- **U2. Hop limit.** None is assumed. A limit would make spread local at any `t` and simplify edge
  handling, but adds a parameter with no evidence behind its value.
- **U3. Target mean degree.** A regular lattice only admits certain degrees under a distance rule
  (4, 8, 12 on a square lattice), and a uniform random pattern needs a mean degree above about 4.5
  to hold together (agent arithmetic from Barthélemy p. 43). The target should be chosen with both
  in mind.
- **U4. Margins.** The minimum contrast of interest for H1-C and the skill margin for H1-L must be
  fixed before any real-site run.
- **U5. Gate pass criteria.** What level of OSM-versus-comparison disagreement, or sensitivity to
  node definition, would stop the study or change the site.
- **U6. Rewiring details.** Whether rewired graphs may fragment, number of swaps, ensemble size.
- **U7. Suburban site.** Not chosen; criteria in 5.1 need thresholds.
- **U8. Scope.** Two sites, a synthetic gradient, rewired ensembles, a parameter grid, a
  sub-window model, and a data gate is a large programme for one semester. A minimal pre-specified
  grid (one target degree, few `t` and `p` values) is advisable.
- **U9. Sub-window count.** Unknown until site extents are measured; may not support 5.5.
- **U10. Evidence.** Six papers partly read; eleven items still unread. No literature search. No
  novelty, causal, policy, or external-validity claim may be made without human approval.
- **U11. Independent review.** Outstanding.

## 11. Suggested next scientific action

1. **Synthetic pilot, no real data.** Lattice, jittered lattice, and uniform random points at a
   candidate mean degree; observed and rewired graphs; sweep `t`. This locates the thresholds,
   settles U1 and U3, and checks the expectation that `R < 1` for spatial patterns.
2. **Pre-analysis gate** (section 5.1), which also resolves U7 and U9.
3. Human rulings on U1, U2, and U4 before any real-site propagation run.
4. Regenerate packets 03 and 04 so they embed this v4 file and the review. Note that
   `outputs/evidence_notes.md` is not embedded by the orchestrator.
