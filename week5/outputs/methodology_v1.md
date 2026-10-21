# Methodology (v1)

Stage: 04 Methodology Agent
Packet: `outputs/prompt_packets/04_methodology_agent.md`
Date: 2026-10-05
Source question: `outputs/research_questions.md` (v8, frozen by human decision 7)
Revision: second draft, after human decision 8 (section 0). First draft kept at
`outputs/history/methodology_v1_draft1.md`. The filename stays `methodology_v1.md` because
`src/orchestrator.py` embeds that path in packets 05 to 07.

Nothing in this document is a finding. No data has been collected and no code has been run.

## 0. Human decision recorded

**Decision 8 (after the first draft of this methodology).**

> Accept all three additions: the result caps, the non-gating omission and provenance checks, and
> the nearest-neighbour bearing entropy (flagged as without precedent).
> Kibera boundary: do not use an administrative boundary. Define the study area as a fixed square
> window within Kibera, placed and recorded before any outcome is computed, sized to contain the
> full core-plus-buffer. Its core building count sets the target for suburb matching.
> No other changes. Proceed to the methodology critic.

Effects:

- The cap in MD26 and the flags in 14.4 are now part of how v8 4.4 is read.
- MD11 (omission check) and MD12 (provenance check) are approved. They still gate nothing.
- MD22 (nearest-neighbour bearing entropy) is approved and stays flagged as having no precedent
  in the evidence.
- MD1 and MD2 are rewritten: the Kibera study region is a fixed square window, not a settlement
  polygon. `N*` is the core count of that window.
- Changes in this draft: section 0 added; 5.1 rewritten; 5.2, 7, 14.1, 15, 19, 20, and 21 edited
  to match. Nothing else is changed.

The research question, the primary estimand `D`, the hypotheses, the 1.25 margin, and the decision
rule are taken from v8 unchanged. This document makes them operational. Every choice below that
is not a human ruling is labelled an **agent default** (codes `MD1` to `MD27`) so the methodology
critic can challenge it. None is a request for a ruling. Decision 8 has since approved MD11,
MD12, MD22, MD26, and the flags in 14.4.

## 1. Inputs used

| Input | Use |
|---|---|
| `inputs/problem.md` | Scope, constraints, required validity threats |
| `outputs/research_questions.md` v8 | Question, estimands, hypotheses, decision rule, design components 5.1 to 5.8, concerns M1 to M17 |
| `outputs/question_review.md` | Independent review of v7. Used for the reasoning behind M1 to M17 |
| `outputs/abstract_v1.md` | Wording (M15) and open items X1 to X10 |
| `outputs/evidence_notes.md` | Read by hand, as v8 section 11 item 6 asks. It is not embedded in the packet. Every citation below is limited to what the notes record, on the pages they list |
| Human decisions 1 to 7 | As quoted in v8 section 0 |
| Human decision 8 | Quoted in section 0 |

The evidence PDFs were not opened at this stage. Citations are as reliable as the notes, which
record that several papers were read only in part.

## 2. Assumptions of this stage

- **S1.** v8 is binding. Where v8 leaves a value to the pilot, this document gives a default, a
  candidate set, and the rule by which the pilot picks. The pilot uses synthetic patterns only.
- **S2.** Following v8 M15, the process is called "injected label errors under an
  independent-cascade rule". No classifier is trained or evaluated. The rule is a stylised
  abstraction and not a published scoring method (v8 A4). Results cannot be read as results for
  the averaging rule of Macskassy & Provost (2005).
- **S3.** All nodes are buildings. No person, household, or population weight is used. The
  optional WorldPop or GHSL weighting in `inputs/problem.md` is not used.
- **S4.** Numbers called "convention" have no evidence basis. They are fixed in advance so they
  cannot be tuned to a result (v8 M17).
- **S5.** Some technical statements below come from the agent's background knowledge and are in
  no supplied paper. They are marked **[background, unverified]** and collected in section 17.
- **S6.** This document, v8, the review, and the abstract come from the same model family. Their
  agreement is not independent validation (AGENTS.md rule 4).

## 3. Summary against the required components

| # | Component | Short answer | Section |
|---|---|---|---|
| 1 | Unit of analysis | Building (node); graph (site or pattern, observed or rewired); the cross-site contrast (n = 1) | 4 |
| 2 | Study area and temporal scope | A fixed square window within Kibera; one count-matched planned suburb window in one US metro; one dated snapshot per footprint source | 5 |
| 3 | Dependent variables | `t_c`, `Q`, `D` (primary); `A`, `R`, `C` (descriptive) | 6 |
| 4 | Explanatory variables | Wiring (observed or rewired), site, gradient position; controlled: degree sequence, node count, mean degree | 6 |
| 5 | Data provenance and resolution | OSM, Microsoft, Google footprints at Kibera; OSM and Microsoft at the suburb; OSM streets for suburb selection | 7 |
| 6 | Preprocessing | Projection, node definition, core and buffer, distance calibration | 8 |
| 7 | Model / algorithm | Distance-band graph, degree-preserving rewiring, bond-percolation threshold by edge addition with union-find, cascade simulation | 9 |
| 8 | Baseline / comparator | Rewired ensemble; synthetic gradient; mean-`Q` baseline for the learning component | 10 |
| 9 | Spatial train/test strategy | Train on synthetic, test on two real sites, grouped validation within the gradient. Spatial-leakage control is described, not demonstrated | 11 |
| 10 | Uncertainty / sensitivity | Block-bootstrap interval, Monte Carlo precision, pre-specified sensitivity runs | 12 |
| 11 | External validation | Three known-answer benchmarks; a second footprint source; no validation beyond the two sites | 13 |
| 12 | Stopping / decision criteria | Ordered gates, the v8 4.4 rule, and "not attributable" flags | 14 |

## 4. Unit of analysis

| Level | Unit | Count | Role |
|---|---|---|---|
| Node | One building footprint after the node definition (8.2) | `N*` core nodes per graph, plus buffer | Carries a flag in the cascade |
| Edge | A pair of nodes within distance `d` | About 4 per node at mean degree 8 | Transmits with probability `t` |
| Graph | A site or a synthetic pattern, in its observed or one rewired form | 2 sites, the gradient patterns, and an ensemble for each | `t_c` is a property of a graph |
| Contrast | `D` | **1** | Primary estimand |
| Bootstrap block | A square tile of a site's study region | 25 per site by default | Resampling unit for the interval |
| Learning row | One synthetic pattern realisation | Set in the pilot | Training data for the fallback model |

**Ecological inference.** Every outcome is a graph-level quantity. Nothing is inferred about any
single building, and nothing at all about people. The two sites are two cases (v8 A6), so `D` is
one number and no test of the site term is reported.

## 5. Study area and temporal scope

### 5.1 Kibera

- **Study region `P_K` (decision 8)**: a fixed square window within Kibera. No administrative
  boundary is used. The window holds the whole graph, core plus buffer.
- **MD1. Placement rule** (agent default; decision 8 requires a window and leaves its placement
  to be recorded).
  - The square is axis-aligned in EPSG:32737.
  - It is placed by eye on current imagery so that it lies wholly inside contiguous built
    settlement, and it is the largest square that does so.
  - Its corner coordinates, the imagery date, and an image of the window on the imagery are
    committed to the repository **before any footprint is counted**. This is earlier than
    decision 8 requires ("before any outcome is computed").
  - Once committed it is not moved or resized.
- **MD2. Core and buffer.** The core is the window shrunk by `w` on every side. The buffer is
  the rest of the window. Buildings outside the window are not used, although they exist.
- **`N*`** is the number of nodes in that core (decision 8). It is **unmeasured**.
- `w` is a multiple of `d` fixed in the pilot (MD14), and the pilot runs at `N*`. To break the
  circle, `N*` is first computed at the default `w` = 4`d`. If the pilot fixes a different
  multiple, `N*` is recomputed once and the pilot's operating values are confirmed at the new
  count.
- **Consequences.**
  - Results describe one window of Kibera, not Kibera. A different window could give a different
    `Q`. The block bootstrap describes variation inside the window only.
  - Placement is a judgement. The record makes it visible; it does not make it neutral.
  - The outer edge of the buffer cuts through continuous settlement, so outer buffer nodes have
    truncated neighbourhoods. The same now holds at the suburb, which makes the edge rule
    symmetric across sites.

### 5.2 Suburb

- One metro, five nominated planned neighbourhoods, human approval before any indicator is
  computed, then selection by summed rank (v8 5.2, steps 7 to 12). No suburb is named here.
- **Study region `P_S`**: a square centred on the nominated neighbourhood (v8 A11). **MD3.** The
  square is the whole region, and its core is the square shrunk by `w` on each side, as in MD2.
  The square grows until the core holds 0.9 `N*` to 1.1 `N*` nodes. Both sites are now squares
  under one core-and-buffer rule. They differ in one respect: the Kibera square is fixed first
  and sets the count, and the suburb square is grown to match it.
- Consequence to expect: a planned suburb holds far fewer buildings per unit area than an
  informal settlement, so `P_S` may be many times the area of `P_K` and may extend past the
  nominated neighbourhood into arterial roads, commercial land, and open space
  **[background, unverified; nothing has been measured]**. Section 14 states what is done then.

### 5.3 Temporal scope

- Cross-sectional. One snapshot per footprint source. No change over time is studied.
- OSM is taken at one stated date for both sites. Microsoft and Google footprints carry the dates
  of their source imagery, which the project cannot choose.
- Recorded per site: download date, source release, imagery date where published, and the date
  of the imagery used in the spot check.
- **Temporal leakage** does not arise, because nothing is predicted forward in time. **Temporal
  misalignment** between sources and between sites remains a stated threat (v8 section 6).

## 6. Variables

### 6.1 Dependent

| Symbol | Definition | Status |
|---|---|---|
| `t_c(G)` | Transmission probability at which the second-largest cluster of `G` peaks, on the largest component, counting core nodes (9.3) | Reported for every graph |
| `Q(G)` | `t_c(G)` divided by the mean `t_c` of its rewired ensemble | Reported per site and per pattern |
| `D` | `log Q(Kibera) − log Q(suburb)` | **Sole primary estimand** |
| `A`, `R`, `C` | Amplification factor, its observed-to-rewired ratio, and the cross-site contrast (v8 4.2) | Descriptive. Never corroboration |
| Degree assortativity | Per observed graph, with the rewired mean beside it | Diagnostic. Not tested |

### 6.2 Explanatory and controlled

| Variable | Type | How it enters |
|---|---|---|
| Wiring: observed or rewired | Manipulated | Defines `Q` |
| Site: Kibera or suburb | Two observed cases | Defines `D` |
| Gradient position: coefficient of variation of nearest-neighbour distance | Manipulated on synthetic patterns | H1-G |
| Arrangement metrics (9.6) | Measured | Predictors in the learning component |
| Degree sequence | Controlled exactly | Rewiring |
| Core node count | Controlled within ±10% | Window sizing |
| Core mean degree | Controlled | Per-graph calibration of `d` |
| `t`, `p` | Set by design | Secondary runs only. `t_c` does not depend on `p` |
| Footprint source, node definition, `d` | Nuisance | Gate, then sensitivity runs |

## 7. Data provenance and resolution

| Dataset | Site | Use | Resolution | To record or verify |
|---|---|---|---|---|
| OSM building polygons, via OSMnx | Both | Candidate node source | One polygon per mapped building | Snapshot date; how the snapshot is pinned (Overpass date setting or ohsome): **TO VERIFY** |
| Microsoft Global ML Building Footprints | Both | Candidate node source; suburb window sizing | Machine-derived polygons | Release identifier; imagery dates if published: **TO VERIFY** |
| Google Open Buildings | Kibera only | Third source for the gate | Machine-derived polygons with a confidence score **[background, unverified]** | Version. **MD4:** one confidence cut-off, fixed before download and taken from the dataset's documentation: **TO VERIFY** |
| OSM street network, via OSMnx | Suburb candidates | Orientation order and dead-end share (Boeing 2019, pp. 5–6) | Street segments | Network type "drive"; same OSM date as the buildings |
| Current satellite imagery (ArcGIS Pro basemap) | Both | Visual spot check | Sub-metre **[background, unverified]** | Imagery date at each sampled location |
| Study window | Kibera | `P_K` | One square, placed on imagery | Corner coordinates and imagery date (MD1) |

**What the evidence says about these sources**

- In Nairobi as a whole the Microsoft count is about 35% below the OSM count, and most OSM
  polygons have no matching machine-derived polygon (Okyere et al. 2025, preprint, pp. 14–16).
  This is a city-wide figure, not a Kibera figure.
- Machine-derived footprints are weakest in dense contiguous settlement (Sirko et al. 2021,
  pp. 1, 4).
- OSM coverage in Las Vegas is described as a few well-mapped neighbourhoods among unmapped areas
  (Herfort et al. 2023, p. 6).
- **Not established by anything read:** that either site's data is adequate; how Kibera's OSM
  buildings were mapped (the Map Kibera documentation has not been supplied); whether OSM
  buildings in the chosen metro were imported from another dataset.

## 8. Preprocessing

### 8.1 CRS and units

- Kibera: EPSG:32737 (WGS 84 / UTM zone 37S), as in v8.
- Suburb: the UTM zone of the chosen metro. EPSG:32612 if in the Phoenix area, EPSG:32611 if in
  the Las Vegas area **[background; confirm at selection]**.
- All distances and areas are in metres and square metres, computed after projection. No
  distance is computed in degrees.
- UTM scale error over a few kilometres is far below a percent **[background, unverified]**.
  It is close to uniform within a site, and `d` is calibrated to a mean degree, so a uniform
  scale error does not change any graph. It affects only the reported metric value of `d`.

### 8.2 Node definition (fixed at the gate, before any graph is built)

- **MD5.** One node per footprint polygon. Position is the polygon centroid (Behnisch et al.
  2019, p. 2).
- **MD6.** Polygons under 10 m² are removed. The figure is the one used by Behnisch et al.
  (p. 2) for German cadastral data. Its suitability for Kibera is **unknown**. The share of
  polygons removed is reported per site and per source, because footprint sizes differ between
  Nairobi and a US city (Okyere et al., p. 16).
- **MD7.** Touching or overlapping polygons are **not** merged in the primary definition.
  Merging them is a sensitivity run.
- Invalid geometries are repaired with a zero-width buffer. Exact duplicates are dropped.
  Counts of both are recorded.
- The same definition applies to both sites and all sources.

### 8.3 Pre-analysis gate

The gate follows v8 5.2 steps 1 to 15. This section adds only what v8 left to this stage.

**Kibera**

| v8 step | Addition |
|---|---|
| 3. Source rule on counts | **MD8 (M9).** Total built area per source is tabulated beside the counts. The 30% rule on counts is a human ruling and is applied as written. If it moves the study off OSM while built area differs by less than 30%, the cause is probably merged or split polygons. `D` is then also computed on OSM as a required sensitivity run |
| 3. Spot check | 50 footprints drawn with a fixed seed from the core. Pass is 45 of 50 matching one real structure (v8 A10). Two tallies are kept: "no structure" and "more than one structure" |
| 4. Within-site completeness | **MD9.** 100 m square cells aligned to the projected grid. A cell is flagged when the chosen source is more than 30% below both others on count or on built area (v8 A11). Cells with fewer than 20 buildings in every source are not assessed. A 200 m grid is run as the zoning check |
| 4. Response to flagged cells | **MD10 (M8).** Report the map. Proceed on the gate-chosen source. Recompute `Q` and `D` on the source with the highest count in the flagged cells. If more than half of the flagged cells fall in the top fifth of cells by building density, `D` carries the flag "not attributable to arrangement" (v8 section 7) |
| New, not gating (approved, decision 8) | **MD11 (M10).** Omission check: 50 random points in the core; at each, the nearest building visible in imagery is checked for a footprint in the chosen source. The omission tally is reported. It gates nothing, because decision 7 defines the gate |

**Suburb**

- Step order is confirmed (M10): size all five windows on Microsoft footprints, select, apply the
  source rule at the selected window, then re-size once if the source changes.
- **MD12 (approved, decision 8; not gating).** Before nomination, read the OSM `source` and
  changeset tags for buildings in the metro, to see whether they were imported. **TO VERIFY**
  that such tags exist there. If the OSM
  buildings derive from the Microsoft footprints, the two sources count as one, the 30% rule has
  nothing to compare, and the spot check and omission check are the only quality evidence. This
  is reported as a limitation, not a stop.
- **MD13 (M4).** Orientation order and dead-end share are computed on the full square `P_S`.
  Boeing (2019) computed them at city scale, so a large window is within their documented use.
  The ground area of each window is reported beside its indicators.

**Both sites, structural measurements with no propagation**

These read geometry and graph structure only. No percolation or cascade is run on a real graph
until the protocol freeze in section 14.

- Arrangement metrics (9.6), so that the synthetic gradient can be made to span the sites
  (MD20).
- Largest-component share of core nodes at mean degree 8, 10, and 12 (MD16).
- Moran's I of node degree under the distance-band weights (`esda`), and the distance at which
  the degree correlogram falls to near zero. This sets a lower bound on block size (12.1).

### 8.4 Core, buffer, and distance calibration

- **MD14. Buffer width `w`** is a multiple of `d`, chosen in the pilot from 2`d`, 4`d`, 6`d`:
  the smallest value at which `log Q` on the uniform random pattern differs from its value at
  the next larger width by less than 0.1 × log 1.25. Default if all pass: 4`d`.
- **Calibration.** For each graph, `d` is found by bisection so that the mean degree of core
  nodes equals the target to within 0.05. Because `w` depends on `d`, the two are solved
  together by iteration.
- Reported per graph: `d` in metres, `w`, ground area, core node count, core share of nodes.

## 9. Model and algorithms

### 9.1 Graph construction

- Distance-band graph with `libpysal.weights.DistanceBand` (binary, threshold `d`), converted
  to an undirected simple NetworkX graph. Node attributes: coordinates, core flag, block index.
- **MD15. Target mean degree 8** (one precedent: Iotti et al. 2017, p. 4). It is above the
  critical mean degree of about 4.5 for a uniform random pattern (agent arithmetic from
  Barthélemy 2011, p. 43), and a square lattice can reach it under a distance rule.
- **MD16 (M6). Largest-component rule.** The largest component of the full graph must hold at
  least 90% of core nodes at **both** real sites. The figure is the independent review's
  suggestion and is a convention.
  - If either site fails at mean degree 8, the target is raised to 10, then 12, for **every**
    graph in the study.
  - If a site still fails at 12, the analysis proceeds and `D` carries the flag "describes
    different fractions of the two sites".
  - The same flag applies if the two sites' shares differ by more than 10 percentage points.
  - Synthetic patterns below 90% are kept, marked, and left out of one sensitivity fit of the
    learning model. They do not move the target.
  - The share is a structural quantity at `t = 1`, not a study outcome. It is still related to
    the outcome, so choosing the target from it is a mild form of looking ahead. It is recorded
    as such.

### 9.2 Degree-preserving rewiring

- `networkx.double_edge_swap` on the full graph, core plus buffer (decision 7). No self-loops or
  multi-edges. Connectivity is not enforced, because enforcing it changes which graphs are
  sampled **[background, unverified]**. Each rewired graph gets its own largest component
  (v8 A11).
- **MD17. Swaps and ensemble.** Default 10 successful swaps per edge and 20 rewired graphs per
  observed graph. The ensemble grows until the Monte Carlo criterion in 12.2 is met.
- **Mixing check (pilot, then repeated on every real graph).** An edge-crossing swap can leave
  spatially correlated edge pairs under partial rewiring (Iotti et al., p. 3). Four quantities
  are tracked at 1, 2, 5, 10, and 20 swaps per edge:
  1. share of original edges still present;
  2. mean Euclidean length of edges, using the nodes' retained coordinates;
  3. mean clustering coefficient;
  4. `t_c`.
  The rewiring counts as mixed when 2 to 4 change by less than 2% between 10 and 20 swaps per
  edge. If not, the number of swaps is doubled until they do. Check 2 addresses the spatial
  correlation that Iotti et al. warn about directly: in a mixed null, edge length should match
  the distance between random node pairs.
- **Not used:** the comparison of the rewired threshold with a formula, which the independent
  review proposed. No supplied paper gives the formula (M13).

### 9.3 Estimator of the critical transmission probability

Rule A3 is equivalent to keeping each link independently with probability `t` (Barthélemy 2011,
p. 91, for an SIR process with sharply peaked infection times; its fit to the one-step rule is an
agent inference, checked in 13.1). `t_c` is therefore estimated without simulating cascades.

1. Take the largest component of the full graph. Let `E` be its number of edges.
2. Shuffle the edges with a seeded generator. Start with no edges.
3. Add edges one at a time, merging clusters with a union-find structure. A cluster's size is
   its number of **core** nodes.
4. After each addition record the size of the second-largest cluster.
5. Repeat for `M` permutations and average the curves at each edge count `n`.
6. Smooth the mean curve with a running mean. `t_c` is `n/E` at the maximum.

- This is the edge-addition method the independent review described (issue 5). A published
  source for it exists **[background, unverified: Newman & Ziff; not supplied, not to be cited
  until read]**.
- Using `n/E` for `t` fixes the number of kept edges where the cascade fixes their probability.
  The two agree as `E` grows **[background, unverified]**. Check 13.1 tests the agreement.
- The second-largest-cluster peak is documented for a distance threshold on building data
  (Behnisch et al. 2019, p. 4). Its use for `t` is an agent inference (v8 5.1).
- **MD18. Settings.** `M` = 100 permutations per estimate by default. Smoothing window 1% of
  `E`. Both are fixed in the pilot.
- **MD19. Rule for more than one peak.** `t_c` is the global maximum of the smoothed mean curve.
  Any other local maximum at least half as high is recorded, and the graph is marked
  "multi-peak". For a multi-peak real graph, `D` is recomputed taking the lowest such peak and
  again taking the highest. If the two give different rows of the decision table, the result is
  reported as inconclusive.

### 9.4 Synthetic gradient

All patterns have `N*` core nodes and the target core mean degree, with a buffer built as in 8.4.

| Segment | Construction | Default steps |
|---|---|---|
| 1. Lattice | Square lattice; then each point displaced by a Gaussian with standard deviation σ × spacing | σ = 0, 0.1, 0.2, 0.3, 0.5 |
| 2. Uniform random | Binomial point pattern in the window | 1 |
| 3. Clustered | Thomas process: Poisson parents, Gaussian offspring. Matérn cluster if the count cannot be held (v8 A11) | At least 2 clustering strengths |

- 20 independent realisations per step by default.
- **MD20 (M11).** The sites' arrangement metrics are measured at the gate, before the Thomas
  grid is fixed. The grid is then chosen so that the gradient's range of nearest-neighbour
  coefficient of variation contains both sites where it can. This uses no outcome.
- Real buildings cannot overlap, which makes centroids more evenly spaced than random at short
  range (independent review, issue 7; a conjecture). Such patterns would have a coefficient of
  variation below the uniform random value of about 0.52, which is the range the displaced
  lattices cover. A separate minimum-spacing pattern is not added, because scope is frozen.
- A site outside the gradient's range on any metric is reported as extrapolation (decision 6).

### 9.5 Secondary: cascade simulation for amplification

- Rule A3 on the full graph. Seeds drawn independently with probability `p` over all nodes.
- `A` = (non-seeded core nodes flagged) / (seeded core nodes), averaged over runs.
- **MD21. Operating values (O1).** `t` at 0.25, 0.50, and 0.75 of the lowest `t_c` among all
  compared graphs (decision 4). `p` at 0.01 and 0.05. Each `t` is also given as a fraction of
  that graph's own rewired `t_c`.
- Three-step decomposition (v8 4.5). Step 1 uses a common metric `d`; it is run at Kibera's
  calibrated `d` and at the suburb's, and both are reported.
- 2-hop-limited runs as the sensitivity analysis (decision 4).
- Monte Carlo intervals only. No margin, no test, no part in the decision rule.

### 9.6 Arrangement metrics

Computed on node positions, so they exist for real sites and synthetic patterns alike.

| Metric | Definition | Note |
|---|---|---|
| Nearest-neighbour coefficient of variation | Standard deviation over mean of nearest-neighbour distances, core nodes | Gradient axis. 0 for a lattice; about 0.52 for uniform random points (agent arithmetic) |
| Second-order summary | Ripley's `L(r) − r` at `r = d` and `r = 3d`, with edge correction (`pointpats`) | Function names **TO VERIFY** |
| Mean clustering coefficient | NetworkX, core nodes | Expected to be nearly flat on the random part of the gradient (Barthélemy pp. 43–44) |
| **MD22.** Nearest-neighbour bearing entropy | Entropy of the bearing from each node to its nearest neighbour, taken modulo 180° in 18 bins | Agent adaptation of Boeing's orientation entropy (2019, pp. 5–6). v8 5.5 proposed building or street orientation. Synthetic points have neither, so that version could not serve as a predictor. Approved by decision 8. **It has no precedent in the evidence** |

Diagnostics, reported and not used as predictors: degree assortativity (v8 A5) and Moran's I of
degree.

## 10. Baselines and comparators

| Comparator | Compared with | What it isolates |
|---|---|---|
| Degree-preserving rewired ensemble | Each observed graph | Arrangement at a fixed degree sequence. This is the null inside `Q` |
| Synthetic gradient | Both real sites | Arrangement manipulated directly, with count and mean degree fixed (H1-G) |
| Mean-degree-matched raw `t_c` | `Q` | What the degree sequence contributes (v8 4.5) |
| Mean `Q` of the synthetic patterns | Learning model | Whether the model beats a constant |
| k-nearest-neighbour graph | Distance-band graph | Robustness of `D` to the edge rule |

The rewired null is not spatial. `Q` compares a spatial graph with arbitrary wiring at the same
degrees. It also contains the clumping of dense patches (v8 A5), and nothing here separates
that from local connection pattern.

## 11. Learning component and its validation

**Fallback form (the expected path, decision 7)**

- **Rows:** every synthetic realisation. **Target:** `log Q` (decision 6 names the ratio; the log
  keeps it on the scale of `D`). **Predictors:** the four metrics in 9.6.
- **Models (v8 G12).** Primary: ordinary least squares of `log Q` on the nearest-neighbour
  coefficient of variation alone. Comparison: least squares on all four metrics, and a random
  forest. **MD23.** The single-predictor model is primary because the gradient has about ten
  distinct steps, and realisations of one step are not independent.
- **Validation inside the gradient.** Leave-one-step-out, with all realisations of a step held
  out together, and leave-one-segment-out. Random row splits are not used, since they would put
  near-identical patterns on both sides of the split.
- **Test.** Two predictions, one per real site, each against the site's estimated `Q` and its
  block-bootstrap interval, and against the mean-`Q` baseline. Each is labelled in-range or
  extrapolation, metric by metric (v8 A10).
- **What it can show (M12).** Two errors are not an estimate of skill. If both sites are
  extrapolation, the component shows only that the gradient did not reach them.

**Spatial leakage**

- Training and test data share no space, so the leakage described by Roberts et al. (2017) and
  Sun et al. (2023) cannot occur here, and its control cannot be shown either.
- The write-up must say that the spatial-leakage requirement of `inputs/problem.md` is
  **described and not demonstrated** (decision 5).
- Grouped validation by gradient step guards against a related problem, non-independent rows.
  It is not spatial cross-validation and is not presented as such.

**Sub-window form (runs only if its condition is met)**

- Condition (decision 5): at least 20 blocks per site, each wider than the typical cascade
  extent. **MD24.** That extent is the 95th percentile of the distance from a seed to its
  farthest flagged node at the largest secondary `t`, measured on the uniform random pattern in
  the pilot.
- If met: `log R` per block on the 9.6 metrics; linear regression, or a spatial lag or error
  model from `spreg` if Moran's I of the residuals is significant; a site-mean baseline; 5-fold
  spatially blocked cross-validation with blocks larger than the range of residual
  autocorrelation (Roberts et al., pp. 918–920).
- At a matched count of `N*` this is not expected to run (v8 5.7).

## 12. Uncertainty and sensitivity

### 12.1 Block-bootstrap interval for `Q` and `D` (M1)

Decision 7 names the block bootstrap. v8 left the scheme undefined. Two candidates are defined
here. The pilot picks one on synthetic patterns, before any real outcome exists.

**Blocks.** **MD25.** A 5 × 5 grid of equal rectangles over the bounding box of the study
region, giving up to 25 blocks. Blocks holding under 1% of core nodes are merged into a
neighbour. The block side must exceed the range of degree autocorrelation measured in 8.3, by
analogy with the block-size advice of Roberts et al. (p. 918); if it does not, a 4 × 4 grid is
used.

**Scheme W: reweighted counting.**

- The graph and its rewired ensemble are left intact.
- Each resample draws block multiplicities from a multinomial. A core node's weight is its
  block's multiplicity.
- Cluster sizes in 9.3 become sums of weights. `t_c` is recomputed for the observed graph and,
  with the same weights on the same node labels, for each rewired graph. This defines the
  rewired denominator for every resample.
- No link is cut and no blocks are joined.
- **Weakness.** Only the set of nodes being counted varies. The wiring does not. The interval
  may be too narrow.

**Scheme T: tile bootstrap with a rebuilt graph.**

- Whole blocks of points are drawn with replacement and placed in the block positions. Each
  point keeps its position within its block **[background, unverified as a named method]**.
- The distance-band graph is rebuilt on the new pattern, `d` is recalibrated, and a new rewired
  ensemble is drawn.
- **Weakness.** Joins between blocks create neighbour pairs that never existed, which is the
  independent review's objection. It also costs far more computation.

**Choosing between them (pilot, synthetic only).**

- For one pattern per segment, `log Q` is computed on 50 independent realisations. Their
  standard deviation is the reference.
- For each scheme, the bootstrap standard deviation of `log Q` is computed on each realisation
  and averaged.
- The scheme whose ratio to the reference is nearest 1 in its worst segment is adopted. Ties go
  to scheme W.
- **MD26. Calibration band.** The adopted scheme is "calibrated" if that ratio lies between
  0.75 and 1.33 in every segment. The band is a convention.
- **If neither scheme is calibrated**, the interval is still computed and reported, as decision
  7 requires, and the study's reading is capped at **inconclusive**. This follows v8 M1 and is
  approved by decision 8.

**Interval.** The two sites are resampled independently. `D*` = `log Q*(Kibera)` −
`log Q*(suburb)`. The interval is the 2.5th to 97.5th percentile of `D*` over 1,000 resamples
for scheme W, or 200 for scheme T. The level and counts are conventions.

**What the interval means.** It describes variation within each site as mapped. Synthetic
patterns are uniform in their statistics across space and real settlements are not, so a scheme
calibrated on the former may still misstate the latter. The interval says nothing about other
settlements or other suburbs.

**Companion, reported and never deciding.** `Q` on four non-overlapping windows of equal core
node count per site, with their range. This is the method the independent review preferred.
Each window holds a quarter of the nodes, so its `t_c` carries a larger size bias.

### 12.2 Monte Carlo precision

- Sources: edge permutations in 9.3 and the finite rewired ensemble.
- The Monte Carlo standard error of `D` is estimated by resampling permutations and ensemble
  members. Both are increased until it is at most one tenth of the half-width of the bootstrap
  interval (v8 A11).
- Reported beside the bootstrap interval as precision only. It never decides a row.

### 12.3 Pilot checks adopted from v8 section 10.3

| Check | Procedure | Consequence |
|---|---|---|
| Size bias at `N*` (M2) | Estimator on lattices of 0.9 `N*`, `N*`, 1.1 `N*` nodes | The gap from 0.5 is reported. It does not gate, because it enters `D` only where the two sites differ |
| Residual size effect (M3) | `Q` on one pattern per segment at 0.9 `N*`, `N*`, 1.1 `N*` | If `log Q` moves by more than 0.2 × log 1.25 across the band, the suburb's count match is tightened to ±5%, which is inside decision 7's ±10% |
| Buffer effect (M5) | `Q` with and without a buffer on one pattern per segment | Reported. Informs how far the uniform edge rule matters |
| Lowest `t_c`; sign of `Q − 1` | Recorded per pattern | Neither is assumed (v8 section 2) |

### 12.4 Sensitivity runs on real graphs

All are fixed before the freeze. Each reports `Q` per site and `D`.

| Factor | Variants | Validity threat addressed |
|---|---|---|
| Target mean degree | The next two values on the ladder 8, 10, 12 | MAUP, scale form |
| Node definition | No minimum area; 20 m² minimum; touching polygons merged | Unit comparability |
| Distance measure | Footprint edge to footprint edge | Scale mismatch between small and large buildings |
| Edge rule | k-nearest neighbours, `k` = 8, edge kept if either node lists the other; resulting mean degree reported | Robustness only (decision 2) |
| Footprint source | Each alternative source that passed the spot check | Source dependence |
| Block layout | Grid origin shifted by half a block; 4 × 4 and 6 × 6 grids | MAUP, zoning form |
| Completeness cells | 200 m grid | MAUP, zoning form |
| Multi-peak rule | Lowest and highest qualifying peak | Estimator ambiguity |

**Reading rule.** If `D` changes sign across node definitions or footprint sources, the result
is "not attributable to arrangement" (v8 section 7). The sensitivity runs do not produce a
second verdict. They qualify the first.

## 13. External validation

No dataset records how label errors actually spread on building graphs, and by the boundary
conditions none may be used. Validation therefore rests on known answers and on independent
inputs.

### 13.1 Known-answer benchmarks (all before real data)

| Benchmark | Known value | Source | Pass criterion |
|---|---|---|---|
| Bond threshold of the four-neighbour square lattice | 0.5 | Barthélemy 2011, pp. 85–86 | 0.50 ± 0.02 on at least 250,000 nodes (v8 A11). **MD27:** the mean of 10 independent estimates, each from 100 permutations, must lie in the band. **Gates everything** |
| Mean clustering of a uniform random distance-band graph | About 0.587 | Barthélemy 2011, pp. 43–44; Iotti et al. 2017, p. 2 | Within 0.02. Tests the graph builder |
| Mean degree at which a giant component appears in a uniform random pattern | About 4.5 | Agent arithmetic from Barthélemy 2011, p. 43 | Agreement to within 0.5. A sanity check only, since the value comes from a fitted formula |
| Cascade against edge removal | Same `t_c` from simulated cascades and from 9.3 on one graph | Mapping in Barthélemy 2011, p. 91 | Difference below the Monte Carlo error |
| Independence from `p` | Same `t_c` at two seeding rates | v8 4.2 | Difference below the Monte Carlo error |

A failed attempt is kept in the record, the estimator is replaced, and validation is repeated
(v8 5.1). A square lattice is the easy case. Passing does not show the estimator behaves on
patchy graphs.

### 13.2 Independent inputs

- **Second footprint source.** `D` is recomputed on each alternative source that passes the spot
  check. At the suburb the two sources may share an origin (MD12), in which case this is not an
  independent check and is reported so.
- **Synthetic gradient.** It is the only place where arrangement is manipulated with everything
  else fixed, and it does not depend on either site's data.

### 13.3 What is not validated

- Nothing tests whether `D` would be similar for another informal settlement or another suburb.
- Nothing tests the cascade rule against any real system.
- No external-validity, causal, policy, or novelty claim is made. Any such claim needs human
  approval (AGENTS.md rule 9).

## 14. Workflow, stopping criteria, and decision rule

### 14.1 Order of work

| Step | Action | Proceeds if |
|---|---|---|
| 0 | Fix package versions, seeds, and the configuration file. Commit | — |
| 1 | Estimator validation on the square lattice (13.1) | Pass recorded. **No other graph is analysed before this** |
| 2 | Kibera gate: window placed and committed (MD1), sources, spot check, completeness, node definition, `N*` | A source passes the spot check. Otherwise **stop** at this site (v8 A10) |
| 3 | Suburb: provenance check, five nominations, **human approval**, window sizing, indicators, selection, source rule | Approval given; a source passes the spot check |
| 4 | Structural measurements at both sites (8.3) | — |
| 5 | Synthetic pilot at `N*`: remaining 13.1 checks, mixing, target degree, `w`, estimator settings, bootstrap scheme, Thomas grid, `t` and `p` | Mixing criterion met |
| 6 | **Protocol freeze.** All operating values written to the configuration file and committed. The commit hash is recorded in the results | — |
| 7 | Real graphs: `t_c`, `Q`, `D`, assortativity, bootstrap and Monte Carlo intervals | Monte Carlo criterion met |
| 8 | Decision rule (14.3) and flags (14.4) | — |
| 9 | Secondary amplification runs | — |
| 10 | Sensitivity runs (12.4) | — |
| 11 | Learning component (11) | — |

Steps 2 to 4 can run alongside pilot code development at a placeholder size. Step 6 must come
after all of 1 to 5. Any change to an operating value after step 6 is a deviation, recorded with
its reason, and both versions of the affected result are reported.

### 14.2 Stops and escalations

- **Stop** at a site if no footprint source passes the spot check (decision 6).
- **Stop** all analysis while the estimator fails validation.
- **Report to the human, do not decide:** no suburb candidate reaches a 90% largest-component
  share at mean degree 12; or the suburb window has to grow so far beyond the nominated
  neighbourhood that it no longer describes a planned suburb. The first has a default in MD16.
  The second has no numerical rule and is a judgement.

### 14.3 Decision rule (v8 4.4, unchanged)

| Block-bootstrap interval for `D` | Reading |
|---|---|
| Wholly at or beyond `+log 1.25`, or wholly at or beyond `−log 1.25` | Supports H1-T within this model. Sign reported against the expectation in v8 4.3: `D ≤ −log 1.25` matches it, `D ≥ +log 1.25` is contrary to it |
| Wholly inside (`−log 1.25`, `+log 1.25`) | Refuted within this model, if the sites also sit at similar gradient positions (v8 section 7) |
| Straddles either margin | Inconclusive |

`log 1.25` is about 0.223. The 1.25 margin is a convention with no basis in evidence or in a
stated consequence (v8 4.2, M17). The abstract no longer says so (abstract X8); the methodology
and any later write-up should.

### 14.4 Flags that qualify the reading

Approved by decision 8 as part of how the rule in 14.3 is read.

| Condition | Effect |
|---|---|
| Bootstrap scheme not calibrated in the pilot (MD26) | Reading capped at inconclusive |
| Multi-peak graph, and the two peak choices give different rows (MD19) | Inconclusive |
| `D` changes sign across node definitions or sources | Not attributable to arrangement |
| Largest-component share below 90% at a site, or differing by more than 10 points (MD16) | Not attributable to arrangement |
| Flagged completeness cells concentrated in Kibera's densest cells (MD10) | Not attributable to arrangement |
| H1-G fails: `Q` does not vary with gradient position | Mechanism undercut; stated beside any supported `D` |

**Wording when the reading is "supports" (M7).** Always included, whatever the assortativity
values:

> The contrast compares each site with a rewiring that keeps every building's number of
> neighbours. That rewiring also breaks up the grouping of densely packed buildings. The
> contrast therefore reflects both the local pattern of connections and the clumping of dense
> patches, and this study cannot separate them. Degree assortativity was [value] at Kibera and
> [value] at the suburb, against [value] and [value] in their rewired graphs.

### 14.5 Reproducibility

- Python only. Pinned versions in `requirements.txt` for OSMnx, GeoPandas, Shapely, NetworkX,
  libpysal, esda, spreg, pointpats, NumPy, SciPy, and scikit-learn. The last four are not in the
  library list in `inputs/problem.md`; pointpats is part of PySAL, and scikit-learn is needed
  only for the comparison random forest.
- One root seed. Every stochastic step draws from a child generator spawned from it, so any
  step can be rerun alone.
- Downloaded footprints are stored unmodified with a checksum and download date. Derived files
  are written elsewhere.
- One module per stage, each runnable alone and commented for a first-time user of OSMnx and
  NetworkX: `gate`, `nodes`, `graphs`, `rewire`, `percolation`, `cascade`, `synthetic`,
  `metrics`, `bootstrap`, `learning`.
- Every intermediate table and figure is kept (AGENTS.md rule 10), including failed validation
  attempts.

## 15. GeoAI-specific checks

| Check | Where it bites | Handling | Residual risk |
|---|---|---|---|
| CRS and units | Distances define every edge | Local UTM per site; metres; `d` calibrated to mean degree (8.1, 8.4) | Low |
| Spatial autocorrelation | Degree is spatially clustered, so nodes are not independent draws. The interval and any regression are affected | No test that assumes independent nodes. Block resampling with blocks larger than the degree-correlation range. Moran's I of degree reported. `spreg` models if the sub-window form runs | The bootstrap may still be miscalibrated (12.1) |
| Spatial leakage | Learning component only | Training and test share no space; grouped validation by gradient step | Control is described, not demonstrated |
| Temporal leakage and alignment | Sources of different dates | Dates recorded; one OSM date for both sites | Misalignment not removable |
| Scale and resolution mismatch | Count matching gives very different ground areas and `d`. The same `t` spans different physical distances. A centroid stands for buildings of very different size | `d`, `w`, and area reported as results (decision 2). Edge-to-edge distance as a sensitivity run | Limits what "the same error process" means across sites |
| MAUP, scale form | `d` and target mean degree | Mean-degree ladder | — |
| MAUP, zoning form | Node merging; bootstrap blocks; completeness cells; position of the suburb square | Variants in 12.4; square centred by a rule fixed in advance | Window placement is tested only through the five candidates |
| Ecological inference | Graph-level results read as statements about buildings or people | Not done. Stated in section 4 | — |
| Edge effects | Truncated neighbourhoods at the boundary | Core and buffer; full graph rewired; outcomes on core nodes (decision 7) | Core nodes of a rewired graph are not a spatial interior (M5). Clusters above threshold reach the outer edge |
| Spatial sampling bias | OSM completeness varies with mapping effort; machine-derived sources are weakest where buildings are densest; the suburb is an extreme case by design; candidates are nominated by judgement | Three-source gate and cell-level completeness at Kibera; spot and omission checks; approval step for nominations. The Kibera window is placed by eye, on a recorded rule, before any footprint is counted | If all sources under-map the same cells, nothing is flagged. One window is one sample of Kibera |
| Transferability | Two cases | None claimed | Needs more sites (v8 section 12) |

## 16. How the central claim could be wrong

- **By measurement.** `t_c` on a few thousand nodes is noisy and shifted (Behnisch et al., p. 4;
  Fagundes et al., p. 1). An estimator that passes on a large lattice may behave differently on
  a patchy graph with several peaks.
- **By the interval.** A bootstrap that is too narrow turns noise into a supported result. One
  that is too wide makes every result inconclusive. Section 12.1 tests this on synthetic
  patterns only.
- **By the data.** If the densest parts of Kibera are the least completely mapped, the missing
  buildings are high-degree nodes and the bias enters `Q` directly (agent inference from Yeboah
  et al. 2021, which reports one figure per site and does not name Kibera).
- **By the comparison.** The suburb square may contain roads and open land that split the graph,
  so `Q` describes a fragment. The two sites may end on different footprint sources with
  different meanings of "one polygon".
- **By definition.** A reader who counts the clumping of dense patches as density would read the
  same `D` differently (v8 A5).
- **By the model.** Under an averaging rule the conclusions may reverse (v8 A4, section 12).
- **By design.** Two places are two cases. A non-zero `D` cannot be attributed to layout type.

## 17. Unsourced background used in this document

Each must be checked against a source before it is relied on or cited.

| Statement | Used in | If wrong |
|---|---|---|
| Edge addition with union-find gives the cluster-size curve for every `t` in one pass; a published description exists (Newman & Ziff) | 9.3 | Check 13.1 against simulated cascades would fail and expose it |
| Fixing the number of kept edges agrees with fixing their probability on large graphs | 9.3 | Same check |
| Enforcing connectivity during rewiring changes which graphs are sampled | 9.2 | The alternative swap could be used; the largest-component rule would matter less |
| A tile bootstrap is a recognised way to resample a point pattern | 12.1 | Scheme T stands as an agent construction and is judged by the pilot alone |
| Google Open Buildings polygons carry a confidence score | 7 | MD4 is unnecessary |
| UTM zones for Phoenix and Las Vegas; UTM scale error far below a percent | 8.1 | Zone corrected at selection; no effect on graphs |
| Suburban building density is far below that of an informal settlement | 5.2 | The suburb window is smaller than feared |
| Basemap imagery is sub-metre at both sites | 7 | The spot check is weaker than assumed |

## 18. Disposition of v8 concerns

| v8 item | Disposition here |
|---|---|
| M1 block bootstrap undefined | Two schemes defined; pilot selects on synthetic data; calibration band; reading capped at inconclusive if uncalibrated (12.1) |
| M2 where the tolerance binds | Gate on the large lattice only; bias at `N*` reported (12.3) |
| M3 residual size effect | Adopted as a pilot check with a consequence (12.3) |
| M4 ground area and `d` | Indicators on the full square; area reported; escalation if the window stops being a suburb (MD13, 14.2) |
| M5 core nodes in rewired graphs | With-and-without-buffer check adopted (12.3). The concern itself stands |
| M6 largest-component share | 90% minimum, mean-degree ladder, flags (MD16) |
| M7 reading `D` when assortativity differs | Fixed wording, always included (14.4) |
| M8 flagged completeness cells | Report, proceed, sensitivity run, attribution flag (MD10) |
| M9 the 30% rule at Kibera | Built area tabulated beside counts; OSM run required if the switch looks like polygon merging (MD8) |
| M10 the suburb gate | Step order confirmed; provenance check; non-gating omission check (MD11, MD12) |
| M11 reach of the gradient | Site metrics measured first; Thomas grid set to span them (MD20). No minimum-spacing pattern |
| M12 fallback evidence value | Stated plainly; grouped validation inside the gradient added (11) |
| M13 unsourced background | Rewired-threshold formula not used (9.2). New unsourced items in section 17 |
| M14 no literature search | Still not done. No wording beyond "application" |
| M15 wording downstream | Adopted (S2) |
| M16 emphasis | No change. H1-G stays supporting. Left to the human |
| M17 conventions | Every convention is labelled where it appears |
| O1 to O7 | Defaults and selection rules: MD21, MD15, MD17, MD18 and MD19, 12.1, MD14, 9.4 |
| G1 to G13 | Kept. G5 made precise in MD27; G2 in MD3 |

## 19. Assumptions most likely to change the conclusion

Ordered by the agent's judgement of how easily each could move the result from one row of the
decision table to another.

1. **The block-bootstrap interval measures within-site variability of `D` well enough.** The
   decision rule is a statement about this interval's width. Scheme W may understate it and
   scheme T may distort it. This assumption alone can decide between "supports" and
   "inconclusive".
2. **Each real graph has one identifiable critical value.** If Kibera or the suburb square gives
   a second-largest-cluster curve with several peaks, `t_c`, `Q`, and `D` depend on the peak
   rule.
3. **Footprint completeness is even enough within each site.** Missing buildings in the densest
   patches remove exactly the nodes that drive `Q`. The gate compares sources with each other
   and cannot see an omission they share.
4. **A count-matched suburb square is still one connected planned suburb.** If it fragments at
   the target mean degree, `Q` describes different fractions of the two sites.
5. **"One polygon" means the same thing at both sites.** The sites may end on different
   sources, and a single minimum-area filter removes different shares of buildings at each.
6. **The rewired ensemble is fully mixed, and estimator bias at `N*` is similar at both sites.**
   Either failure shifts `Q` at one site more than the other.
7. **The clumping of dense patches counts as arrangement** (decision 7). The number does not
   change if this is rejected. Its meaning does.
8. **An independent cascade is the process of interest.** The one documented scoring method in
   the evidence averages over neighbours and may respond to degree in the opposite direction.
9. **The Kibera window stands for Kibera** (decision 8). It is one square placed by judgement.
   Nothing in the design tests a second placement.

## 20. Unresolved issues

- **U1.** The Kibera window is placed by eye (MD1). "Contiguous built settlement" and "largest
  square" leave room for judgement, and the window sets `N*`, which sets the size of everything
  else. No second placement is tested, since decision 8 allows no other changes.
- **U2.** Nothing has been measured: `N*`, areas, `d`, component shares, arrangement metrics,
  source agreement, and completeness are all unknown.
- **U3.** The calibration of either bootstrap scheme is unknown until the pilot. If neither is
  calibrated, the study is inconclusive by construction.
- **U4.** Resolved. The cap in MD26 and the flags in 14.4 are approved (decision 8).
- **U5.** Whether the largest-component rule should also apply to synthetic patterns when the
  target mean degree is chosen (MD16).
- **U6.** Approved (decision 8). The nearest-neighbour bearing entropy (MD22) still has no
  precedent in the evidence, and its behaviour on real building patterns is unknown.
- **U7.** Resolved. The omission check (MD11) and provenance check (MD12) are approved and gate
  nothing (decision 8).
- **U8.** Access details for all three footprint sources, the OSM snapshot mechanism, and the
  `pointpats` functions are marked TO VERIFY.
- **U9.** No literature search has been done (v8 M14). Section 17 lists background that needs
  sources.
- **U10.** The Map Kibera documentation is still not supplied. It would say how far Yeboah et
  al. applies.
- **U11.** Computation time for scheme T has not been estimated.

## 21. Suggested next scientific action

1. **Run the methodology critic** (packet 05) on this file and v8, with attention to 12.1,
   MD1, MD16, and MD19.
2. **Validate the estimator on the square lattice** (14.1, step 1). It needs no data and gates
   everything.
3. **Place and commit the Kibera window** (MD1), then run the Kibera gate to obtain `N*`.
4. **Check OSM building provenance in the candidate metro**, then nominate five suburbs for
   human approval.
5. **Synthetic pilot at `N*`**, ending in the protocol freeze.
6. Regenerate packets 05 to 07. Packet 05 should also be given `outputs/evidence_notes.md` by
   hand, since `src/orchestrator.py` does not embed it.
