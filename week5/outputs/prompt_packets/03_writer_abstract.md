# AGENT ROLE

# ✍️ Scientific Writer Agent

## Role
You turn an approved research question, hypothesis, and evidence plan into concise scientific prose.

## Goal
Write an abstract or research summary that faithfully represents the current research state.

## Rules
- Do not invent results.
- If no results exist, write proposed-study language rather than implying findings.
- Distinguish motivation, question, method, expected contribution, and validation.
- Every empirical or literature claim must be traceable to supplied evidence or marked as needing a citation.
- Do not claim novelty unless the literature review supports it.

## Output
For an abstract:
- Background/problem
- Research gap/question
- Proposed data/methods
- Validation strategy
- Expected scientific contribution
- 200–300 words unless instructed otherwise


# CURRENT TASK

Write a 200–300 word proposed-study abstract using the approved question and critique.

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

# Candidate research questions and hypotheses (v3)

Stage: 01 Research Question Agent
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous versions: `outputs/history/research_questions_v1.md`, `outputs/history/research_questions_v2.md`

## 0. Human decisions recorded

**Decision 1 (after v1): density is a confound.**

> Density is a confound, not part of spatial structure. The central hypothesis is that spatial
> arrangement (irregularity, clustering, connectivity pattern) affects error spread independently of
> density. Use the degree-matched control as the primary test. Report the unadjusted
> density-inclusive comparison as a secondary result.

This narrows the hypothesis in `inputs/problem.md`, which described the expected amplifying layout
as "dense, irregular, highly connected"; the narrowing is the human's, not the agent's.

**Decision 2 (after v2): how strictly density is controlled, and how thresholds are treated.**

> U1 decision: make degree-preserving rewiring a co-primary test alongside cross-site mean-degree
> matching. Degree distribution (including heterogeneity) is treated as part of density and is
> controlled; "arrangement" means the pattern of who connects to whom given fixed degrees. Use
> k-nearest-neighbour graphs as a robustness check, not a primary test.
>
> Threshold issue: accepted as expected. Report the distance threshold needed at each site to reach
> the target mean degree as a descriptive result; differences in what "proximity" means across sites
> are substantively relevant, not a limitation to minimize.

Changes in v3: assumptions A6 and A7, RQ1 (two co-primary tests, outcome table, threats), RQ2 (kNN
as robustness check), the recommendation, and the unresolved-issues list. RQ3 to RQ5 and the ranking
are unchanged.

## 1. Inputs used

| Input | How it was used |
|---|---|
| `inputs/problem.md` (as embedded in the packet) | Broad problem, boundary conditions, human hypothesis, study areas, constraints, datasets, paper list |
| `inputs/evidence/README.md` | Human-supplied claim-to-citation map. **Not embedded in the packet**; read because the role lists "any supplied evidence" as an input |
| Prior reviewer feedback | None present (`outputs/question_review.md` does not exist yet) |

**Status of the evidence.** Only citations and one-line human summaries were supplied. No full texts,
abstracts, datasets, or measurements were available to this agent. Every statement below about what a
paper shows is the human's summary relayed, not something this agent verified. No results exist yet;
nothing in this document is a finding.

## 2. Assumptions

Labelled so the critic can challenge them individually.

- **A1. Nodes are buildings.** Each node is one building footprint; no persons are modelled.
- **A2. Edges are proximity.** Two buildings are linked if they are within a distance threshold `d`
  in a projected CRS. Whether distance is centroid-to-centroid or footprint-edge-to-edge is undecided
  and will matter in Kibera, where structures may abut.
- **A3. The propagation rule is a modelling choice, not an observed mechanism.** No supplied evidence
  describes how any real association-based tool propagates a label. Weber (2016) and Suchman (2020)
  are marked by the human as context only. All results will be conditional on the chosen rule.
- **A4. "Error" is a synthetic label flip** on a randomly chosen fraction `p` of nodes, with fixed seeds.
- **A5. Two sites are two observations of "city type".** Any Kibera-versus-suburb difference is
  confounded with everything else that differs between the two places and their mapping histories.
- **A6. Density is a confound, and it includes the whole degree distribution (human decisions 1
  and 2).** "Arrangement" means the pattern of who connects to whom given fixed degrees.
- **A7. Density acts on propagation only through degree.** Building density matters by giving each
  building more neighbours within `d`. Fixing the degree sequence is therefore taken as fixing
  density. This link between a spatial quantity (buildings per unit area) and a graph quantity
  (degree) is an agent assumption that the human's definition relies on.

## 3. A point that shapes every candidate

*Inference from basic probability, not from the supplied papers.*

If propagation is one hop and deterministic (every neighbour of an erroneous node is flagged), the
probability that a node with `k` neighbours is flagged at error rate `p` is `1 − (1 − p)^k`. Expected
reach is then fixed entirely by the degree distribution. Under that rule the denser site will show
larger reach by construction, and the experiment would confirm an arithmetic identity rather than
test a hypothesis.

Two consequences follow:

1. The propagation rule must allow more than one hop (or be probabilistic over several steps) for
   layout to matter beyond the count of neighbours.
2. The direction of the effect is open. Proximity graphs are expected to be highly clustered, and
   clustering means many paths lead back to already-flagged nodes. A dense, irregular layout could
   therefore spread errors *less far per link* than a degree-matched random graph. The human's
   summary of Watts & Strogatz (1998) is consistent with clustering and path length both mattering,
   but that is the human's reading of an unread paper.

## 4. Ranked candidates

Ranking reflects this agent's judgement against the seven criteria; it is not validation.

### Rank 1. RQ1: Does spatial arrangement moderate error reach independently of density?

Central question as fixed by human decisions 1 and 2 in section 0.

- **Research question.** With density held fixed, does the arrangement of connections in
  building-level proximity graphs change how far injected errors spread, and does it do so
  differently in Kibera than in a planned US suburban grid?
- **Candidate hypothesis.** Two co-primary tests, each two-sided because direction is the empirical
  question.
  - **H1-R (co-primary, within site, degree-preserving rewiring).** At each site, error reach on
    the observed graph differs from reach on rewired graphs with the identical degree sequence.
    This isolates arrangement exactly as the human defined it.
  - **H1-X (co-primary, cross-site, mean-degree matching).** With `d` chosen per site so both
    graphs have the same mean degree, the slope of error reach on injected error rate `p` differs
    between sites (a site × `p` interaction).
  - **H1-alt (competing).** Arrangement reduces reach relative to the rewired null, because spatial
    clustering makes paths redundant (section 3), and it does so more in the more clustered site.
  - **H1-sec (secondary, unadjusted).** At a common metric threshold `d`, with density left free,
    reach rises faster with `p` in Kibera. Reported for context; density alone could produce it.
- **How the two tests relate.** *Agent reading; the critic should check it.* H1-X matches mean
  degree only, so under decision 2 a cross-site difference could still come from differing degree
  distributions, which now count as density. H1-R controls the full degree sequence but compares a
  site with itself. The outcomes therefore read as:

  | H1-R (observed vs rewired) | H1-X (cross-site) | Interpretation |
  |---|---|---|
  | Differs at both sites | Differs | Supports the central hypothesis, provided the cross-site difference also appears in the arrangement effect (below) |
  | Differs | No difference | Arrangement matters, similarly at both sites; no support for a site-type contrast |
  | No difference | Differs | Cross-site difference is attributable to degree distribution, i.e. density; central hypothesis not supported |
  | No difference | No difference | Central hypothesis refuted within this model |

  To make row 1 airtight, compute an **arrangement effect** per site (observed reach minus mean
  rewired reach, at each `p`) and compare it across sites. That contrast has the full degree
  sequence controlled on both sides. This is an agent proposal, not part of the human decision (U1).
- **Expected observable evidence.** Reach-versus-`p` curves over many seeded replicates for four
  graph conditions: observed and rewired, at each site, all at the same target mean degree.
  Alongside them, as a descriptive result in its own right (decision 2): the distance threshold
  each site needs to reach the target mean degree, with what that distance physically spans at each
  site (abutting structures, neighbouring lots, across a street).
- **Plausible data.** OSM building footprints for both sites via OSMnx, with a recorded snapshot
  date. Google Open Buildings and/or Microsoft footprints for a completeness cross-check.
- **Plausible method.** Project each site to its local UTM zone (EPSG:32737 for Nairobi; EPSG:32612
  for Phoenix or EPSG:32611 for Las Vegas). Build distance-band graphs with libpysal, convert to
  NetworkX, choosing `d` per site to hit a common target mean degree. Generate an ensemble of
  rewired graphs per site with `networkx.double_edge_swap`, using enough swaps that reach
  stabilises. Inject errors at a grid of `p` values. Propagate with a multi-hop rule fixed in
  advance. Measure reach as the fraction of non-seeded nodes flagged, plus an amplification ratio
  (flagged per seeded node). Fit reach on `p`, site, graph condition, and their interactions. Run
  the common-`d` comparison separately for H1-sec.
- **Main threat to validity.**
  - *Site confounding (A5).* With two sites, a cross-site difference in arrangement effect cannot
    be attributed to "irregular versus planned" layouts in general. H1-R is not affected, since it
    is a within-site comparison.
  - *What the rewired null removes.* Rewiring replaces short spatial links with links between
    arbitrary buildings, so the null is no longer a spatial graph. H1-R therefore tests "spatially
    arranged versus arbitrarily wired at the same degrees". It cannot by itself distinguish
    irregular arrangement from regular arrangement; only the cross-site contrast speaks to that.
  - *Rewiring mechanics.* Rewiring can change the number and size of connected components. Whether
    that is allowed (component structure as part of arrangement) or prevented
    (`connected_double_edge_swap`) changes the null and is undecided (U2a).
  - *Two co-primary tests.* Two chances to find an effect; the decision rule and minimum effect
    size should be fixed before any run (U5).
- **What would weaken or refute it.** The central hypothesis is refuted within this model if
  observed graphs are indistinguishable from their rewired nulls at both sites across the tested
  `p` range and target degrees. It is not supported if H1-X shows a difference that disappears in
  the cross-site contrast of arrangement effects. Both are reportable results.

### Rank 2. RQ2: Is the layout effect stable across proximity thresholds and graph definitions?

Best run as the built-in sensitivity analysis for RQ1 rather than a separate study.

- **Research question.** Do the sign and size of the two co-primary RQ1 results hold across target
  mean degrees and graph-construction rules, or do they depend on the scale chosen? The same sweep
  over raw `d` supports the secondary, unadjusted comparison.
- **Candidate hypothesis.** H2: each site has a threshold range in which reach rises sharply as the
  graph becomes connected, and this range sits at smaller `d` in Kibera than in the suburb. Outside
  those ranges the between-site ordering is stable.
- **Expected observable evidence.** Reach and largest-connected-component share plotted against `d`
  show a sharp rise per site at different `d`. The ordering of sites is the same for fixed-distance,
  k-nearest-neighbour, and scale-normalised thresholds (for example multiples of each site's median
  nearest-neighbour distance).
- **Plausible data.** Same as RQ1.
- **Plausible method.** Sweep the target mean degree (primary) and raw `d` (secondary) and repeat
  the RQ1 experiment; repeat with centroid versus footprint-edge distance. Report the full curve,
  not one chosen value, including the threshold-versus-target-degree curve per site (the
  descriptive result from decision 2). As a robustness check only (decision 2), repeat with kNN
  graphs. A kNN graph gives every building `k` outgoing links, but once links are made undirected
  degrees are no longer exactly equal, so the symmetrisation rule must be stated.
- **Main threat to validity.** MAUP in its scale form (Openshaw 1984, per the human's list): the
  conclusion may be an artefact of `d`. Edge effects also grow with `d`, since boundary nodes lose
  more neighbours; analyse a core area inside a buffer at least as wide as the largest `d` times the
  hop limit.
- **What would weaken or refute it.** The sign of the site difference flips across plausible
  thresholds or between graph definitions with no interpretable pattern. That would mean RQ1 has no
  threshold-independent answer, which should then be the headline.
- **Note.** The "sharp rise" expectation is drawn from general network-science reasoning about
  connectivity transitions. The human's summary of Barthélemy (2011) is in that area, but no
  supplied text confirms it for building proximity graphs. **Missing evidence.**

### Rank 3. RQ3: Which local structural properties predict where errors reach, and does that transfer across sites?

The only candidate with a train/test split, so the only one where spatial leakage applies.

- **Research question.** Within each site, which local properties of a neighbourhood (mean degree,
  clustering, building density, footprint-size variation, street-orientation entropy) predict
  simulated error reach, and does a model fitted on one site predict the other?
- **Candidate hypothesis.** H3: local graph and morphology metrics explain reach better than
  building density alone under spatially blocked cross-validation, and the fitted relationship
  transfers across sites with a measurable loss of accuracy.
- **Expected observable evidence.** Held-out skill for the full feature set exceeds a density-only
  baseline under spatial block CV. Residuals show little remaining spatial autocorrelation (Moran's
  I, LISA), or a spatial lag/error model in `spreg` absorbs it. Leave-one-site-out skill is reported
  alongside within-site skill.
- **Plausible data.** RQ1 simulation outputs aggregated to tiles or ego-neighbourhoods; OSM street
  network for orientation entropy (the human cites Boeing 2019 for this measure).
- **Plausible method.** Tree-based regressor plus a spatial autoregressive comparison; spatially
  blocked folds with block size set above the residual autocorrelation range (Roberts et al. 2017 is
  the human's cited source for this practice).
- **Main threat to validity.** Spatial leakage: neighbouring tiles share buildings, edges, and
  propagated errors, so random folds would inflate skill. Also MAUP in its zoning form (tile size
  and origin), and the ecological fallacy if tile-level relationships are read as node-level.
  Leave-one-site-out has a test set of one site, which is a transfer demonstration and not an
  estimate of generalisation.
- **What would weaken or refute it.** Under blocked CV the full model does no better than density
  alone, or skill collapses relative to random CV (a sign that the apparent skill was leakage).

### Rank 4. RQ4: Does the spatial pattern of the initial errors matter as much as the layout?

- **Research question.** At the same overall error rate, do spatially clustered seed errors produce
  different reach than randomly placed ones, and does that difference depend on site?
- **Candidate hypothesis.** H4: clustered seeds produce smaller total reach than random seeds at the
  same `p`, because their neighbourhoods overlap, and the reduction is larger in the more clustered
  graph.
- **Expected observable evidence.** A consistent gap between reach curves for random versus
  clustered seeding, with a seeding × site interaction.
- **Plausible data.** Same as RQ1. Optionally WorldPop or GHSL to weight seeding by population.
- **Plausible method.** Generate seed sets with controlled spatial autocorrelation (for example by
  seeding around random foci), verify the achieved clustering with Moran's I, rerun RQ1.
- **Main threat to validity.** Realism of the seeding process is unknown. The human cites Zhang et
  al. (2023) for errors being spatially structured, but no supplied evidence says what pattern
  errors take in a proximity-association setting. **Missing evidence.** This also adds a second
  experimental factor, which strains the "one controlled experiment" scope limit.
- **What would weaken or refute it.** Reach curves for random and clustered seeding overlap within
  replicate variation at every tested `p`.

### Rank 5. RQ5: Are the conclusions robust to building-footprint incompleteness?

Better treated as a required data check than a headline question.

- **Research question.** Does the difference between OSM and an independent footprint source change
  graph structure enough to alter the RQ1 result?
- **Candidate hypothesis.** H5: the direction of the RQ1 site difference is unchanged when the graph
  is rebuilt from Google Open Buildings or Microsoft footprints.
- **Expected observable evidence.** Building counts, degree distributions, and reach curves from the
  alternative source fall close to the OSM-based ones, with the same site ordering.
- **Plausible data.** OSM (dated snapshot, or ohsome for a historical date), Google Open Buildings
  (Sirko et al. 2021, per the human), Microsoft Global ML Building Footprints.
- **Plausible method.** Rebuild graphs per source; compare; additionally delete a random share of
  buildings to trace how reach responds to omission.
- **Main threat to validity.** Neither source is ground truth. ML-derived footprints may merge or
  split abutting structures in dense settlements, so disagreement does not show which source is
  wrong. Source dates differ from the OSM snapshot (temporal misalignment). The human's Claim 4
  ("the data is adequate") is not yet supported by any measurement.
- **What would weaken or refute it.** The site ordering in RQ1 reverses depending on footprint source.

## 5. Scoring against the required criteria

Qualitative judgement by this agent. H = strong, M = moderate, L = weak.

| Criterion | RQ1 | RQ2 | RQ3 | RQ4 | RQ5 |
|---|---|---|---|---|---|
| Scientific importance | H | H | M | M | M |
| Novelty relative to supplied evidence | Not assessable | Not assessable | Not assessable | Not assessable | Not assessable |
| Testability / falsifiability | H | H | M | H | M |
| Data feasibility | H | H | H | H | M |
| GeoAI / spatial relevance | M | H | H | M | M |
| Tractability in one semester | H | H | M | M | H |
| Validation strategy | H (null models) | H (sweep) | H (blocked CV) | M | M |

**Novelty.** No novelty is claimed for any candidate. The supplied list contains no paper described
as studying error propagation on building-proximity graphs, but an absence from an 11-item reading
list says nothing about the literature. A targeted search is needed before any novelty statement,
and AGENTS.md rule 9 requires human approval for one.

**GeoAI relevance of RQ1 is rated M.** RQ1 is a network simulation with a regression on top; the
"AI" component is thin. RQ3 is where a learned model and spatial cross-validation enter. The critic
should judge whether RQ1 + RQ2 alone meets the course's GeoAI expectation.

## 6. Recommendation

Adopt **RQ1 as the single primary question, with RQ2 as its mandatory sensitivity analysis**. The
co-primary tests are the within-site rewiring comparison (H1-R) and the cross-site mean-degree-matched
interaction (H1-X). The unadjusted common-threshold comparison (H1-sec) and the per-site distance
thresholds are reported as secondary and descriptive results; kNN graphs are a robustness check.
That fits the stated scope (two sites, one method, one error-injection experiment), and the rewired
null gives the hypothesis a real chance of failing. Add RQ3 only if time allows; run the RQ5
completeness check as a data-quality step regardless.

## 7. Unresolved issues

- **U1. How the two co-primary tests combine.** (Earlier U1 items resolved by decisions 1 and 2.)
  H1-X controls mean degree only, while decision 2 counts the whole degree distribution as density.
  The agent proposes the cross-site contrast of arrangement effects (observed minus rewired) as the
  statistic that decides the site-type claim, and the outcome table in RQ1 as the decision rule.
  Needs the critic's assessment and human approval.
- **U2a. Rewiring details.** Whether rewired graphs may fragment or must stay connected, how many
  swaps count as fully mixed, and how many rewired graphs form the ensemble.
- **U2. Propagation rule.** Hop limit, deterministic versus probabilistic, and any decay with
  distance are unspecified. Must be fixed before any run and justified as a stated assumption.
- **U3. Two-site design.** No analysis can attribute a between-site difference to layout type in
  general. Claims about "dense irregular settlements" or "planned grids" as classes would be
  external-validity claims and need human approval.
- **U4. Causal language.** Error injection is randomised inside the simulation, so the effect of `p`
  is causal within the model. Site layout is not randomised. Rewiring manipulates structure, but on
  synthetic graphs. Any causal wording needs human approval.
- **U5. Statistical unit.** Simulation replicates can be made arbitrarily numerous, so p-values will
  become small regardless of practical importance. A minimum effect size of interest should be set
  in advance and results reported as effect sizes with replicate intervals.
- **U6. Suburban site not chosen.** Selection criteria (planned grid, good OSM building coverage,
  comparable extent or node count to Kibera) are undefined. Whether to match sites on area or on
  node count is also open.
- **U7. Distance definition and threshold range.** Centroid versus footprint-edge distance, and the
  range of `d`, cannot be set sensibly until nearest-neighbour distance distributions are measured
  at both sites.
- **U8. No link to real systems.** Nothing supplied shows that any deployed tool behaves like this
  model. Findings would describe the model only; policy relevance needs human approval.
- **U9. Evidence unread.** All eleven listed papers and the evidence README citations should be
  checked against full text before they are cited as support for specific claims.

## 8. Suggested next scientific action

1. Regenerate packet 02 so it embeds this v3 file, then send it to the Scientific Critic with U1
   (how the co-primary tests combine) and the GeoAI-relevance concern flagged for explicit
   judgement.
2. In parallel, a cheap descriptive pull that commits to no hypothesis: download OSM building
   footprints for Kibera and one candidate suburb, record the snapshot date, project to UTM, and
   tabulate building counts and nearest-neighbour distance distributions. This resolves U6 and U7
   and shows whether a common threshold range exists at all.


## FILE: outputs/question_review.md



# EXECUTION REQUIREMENT

Work only from the supplied state and clearly label missing evidence.
Return the requested structured artifact. Do not silently change the research question.
