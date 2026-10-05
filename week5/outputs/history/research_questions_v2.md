# Candidate research questions and hypotheses (v2)

Stage: 01 Research Question Agent
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04
Previous version: `outputs/history/research_questions_v1.md`

## 0. Human decision recorded in v2

After v1, the human resolved the open question of whether density belongs to "spatial structure":

> Density is a confound, not part of spatial structure. The central hypothesis is that spatial
> arrangement (irregularity, clustering, connectivity pattern) affects error spread independently of
> density. Use the degree-matched control as the primary test. Report the unadjusted
> density-inclusive comparison as a secondary result.

Changes from v1: assumption A6, RQ1 (hypotheses, primary test, refutation condition), RQ2 (sweep
variable), the recommendation, and the unresolved-issues list. RQ3 to RQ5 and the ranking are
unchanged. This narrows the hypothesis in `inputs/problem.md`, which described the expected
amplifying layout as "dense, irregular, highly connected"; the narrowing is the human's, not the
agent's.

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
- **A6. Density is a confound (human decision, section 0).** "Spatial structure" means arrangement:
  irregularity, clustering, and connectivity pattern. Density is to be held constant in the primary
  test.
- **A7. Density is controlled through graph degree.** Building density affects propagation only by
  giving each building more neighbours within `d`. Matching the two graphs on mean degree is
  therefore taken as controlling density. This is an agent assumption: equal mean degree does not
  imply equal degree distributions (see U1).

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

Central question as fixed by the human decision in section 0.

- **Research question.** When building-level proximity graphs for Kibera and a planned US suburban
  grid are matched on mean degree, does the relationship between injected error rate and error
  reach still differ between them?
- **Candidate hypothesis.**
  - **H1 (primary, degree-matched).** With mean degree equalised, the slope of error reach on
    injected error rate `p` differs between the two sites (a site × `p` interaction). The problem
    statement expects the irregular layout to amplify; the test is two-sided because the direction
    is the empirical question.
  - **H1-alt (competing).** With mean degree equalised, the interaction is near zero, or the
    irregular site spreads errors *less*, because spatial clustering makes paths redundant
    (section 3).
  - **H1-sec (secondary, unadjusted).** At a common metric threshold `d`, with density left free,
    reach rises faster with `p` in Kibera. Reported for context; it does not bear on the central
    claim because density alone could produce it.
- **Expected observable evidence.** Under H1, degree-matched reach-versus-`p` curves for the two
  sites separate by more than replicate-to-replicate variation, and each site's observed reach
  differs from reach on degree-preserving rewired versions of its own graph. The gap between the
  unadjusted and degree-matched site differences shows how much of the raw effect density carries.
- **Plausible data.** OSM building footprints for both sites via OSMnx, with a recorded snapshot
  date. Google Open Buildings and/or Microsoft footprints for a completeness cross-check.
- **Plausible method.** Project each site to its local UTM zone (EPSG:32737 for Nairobi; EPSG:32612
  for Phoenix or EPSG:32611 for Las Vegas). Build distance-band graphs with libpysal, convert to
  NetworkX. For the primary test, choose `d` separately per site so both graphs reach the same
  target mean degree. Inject errors at a grid of `p` values. Propagate with a multi-hop rule fixed
  in advance. Measure reach as the fraction of non-seeded nodes flagged, plus an amplification
  ratio (flagged per seeded node). Fit reach on `p`, site, and their interaction. Two supporting
  controls: (i) degree-preserving rewiring of each graph (`networkx.double_edge_swap`), which holds
  the full degree sequence and the site fixed while destroying arrangement; (ii) the unadjusted
  common-`d` comparison for H1-sec.
- **Main threat to validity.**
  - *Site confounding (A5).* With two sites, an arrangement effect cannot be separated from site
    identity. The rewiring control partly addresses this because it removes arrangement within a
    single site.
  - *Incomplete density control (A7).* Equal mean degree can hide different degree distributions.
    A highly uneven distribution could itself change reach, and whether that unevenness counts as
    arrangement or as residual density is not settled (U1).
  - *Different physical scales.* Degree matching will require a much larger `d` in the suburb than
    in Kibera (expected, not yet measured). The matched graphs then represent different notions of
    "proximity", and the suburban threshold may span streets.
- **What would weaken or refute it.** H1 is refuted if, across the tested `p` range and target
  degrees, degree-matched site curves overlap within replicate variation **and** observed graphs are
  indistinguishable from their rewired nulls. If only the between-site difference vanishes while
  each site still differs from its null, arrangement matters but does so similarly at both sites;
  that outcome should be reported as such, not as support for H1. A minimum effect size of interest
  should be fixed before any run (U5).

### Rank 2. RQ2: Is the layout effect stable across proximity thresholds and graph definitions?

Best run as the built-in sensitivity analysis for RQ1 rather than a separate study.

- **Research question.** Do the sign and size of the degree-matched site × `p` interaction hold
  across target mean degrees and graph-construction rules, or do they depend on the scale chosen?
  The same sweep over raw `d` supports the secondary, unadjusted comparison.
- **Candidate hypothesis.** H2: each site has a threshold range in which reach rises sharply as the
  graph becomes connected, and this range sits at smaller `d` in Kibera than in the suburb. Outside
  those ranges the between-site ordering is stable.
- **Expected observable evidence.** Reach and largest-connected-component share plotted against `d`
  show a sharp rise per site at different `d`. The ordering of sites is the same for fixed-distance,
  k-nearest-neighbour, and scale-normalised thresholds (for example multiples of each site's median
  nearest-neighbour distance).
- **Plausible data.** Same as RQ1.
- **Plausible method.** Sweep the target mean degree (primary) and raw `d` (secondary) and repeat
  the RQ1 experiment; repeat with kNN graphs and with centroid versus footprint-edge distance.
  Report the full curve, not one chosen value. Note that a kNN graph fixes degree by construction,
  so it is a second, independent way of removing density.
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
degree-matched site × `p` interaction (H1) is the primary test; the unadjusted common-threshold
comparison (H1-sec) is reported as a secondary result. That fits the stated scope (two sites, one
method, one error-injection experiment), and the degree-matched and rewired controls give the
hypothesis a real chance of failing. Add RQ3 only if time allows; run the RQ5 completeness check as
a data-quality step regardless.

## 7. Unresolved issues

- **U1. How strictly is density controlled?** (Replaces v1's U1, resolved in section 0.) Matching
  mean degree leaves the degree distribution free. Options are to match mean degree only, to add
  the degree-preserving rewiring control as a co-primary test, or to use kNN graphs. Whether
  unevenness in degree counts as "arrangement" needs a human decision.
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

1. Regenerate packet 02 so it embeds this v2 file, then send it to the Scientific Critic with U1
   (strictness of the density control) and the GeoAI-relevance concern flagged for explicit
   judgement.
2. In parallel, a cheap descriptive pull that commits to no hypothesis: download OSM building
   footprints for Kibera and one candidate suburb, record the snapshot date, project to UTM, and
   tabulate building counts and nearest-neighbour distance distributions. This resolves U6 and U7
   and shows whether a common threshold range exists at all.
