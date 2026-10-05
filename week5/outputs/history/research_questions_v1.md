# Candidate research questions and hypotheses (v1)

Stage: 01 Research Question Agent
Packet: `outputs/prompt_packets/01_research_question_agent.md`
Date: 2026-10-04

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
- **A6. The human hypothesis bundles density, irregularity, and connectivity** into one construct
  ("dense, irregular, highly connected"). This agent does not unbundle it silently; RQ1 keeps the
  original framing and adds a control that reports how much of the effect density alone accounts for.
  Whether density is part of "spatial structure" or a confound is a decision for the human (see U1).

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

### Rank 1. RQ1: Does urban layout moderate error reach, and is the moderation more than density?

This is the human's stated question, kept intact, with one added control.

- **Research question.** At matched injected error rates, does the reach of propagated error on
  building-level proximity graphs differ between Kibera and a planned US suburban grid, and does a
  difference remain when the two graphs are matched on mean degree?
- **Candidate hypothesis.**
  - H1a: the slope of error reach on injected error rate `p` is steeper in Kibera than in the suburb
    (a site × `p` interaction).
  - H1b: the interaction persists after degree matching, meaning layout contributes beyond density.
  - Competing H1c: after degree matching the interaction shrinks to near zero or reverses, because
    spatial clustering makes paths redundant.
- **Expected observable evidence.** Reach-versus-`p` curves per site, over many seeded replicates,
  that separate by more than replicate-to-replicate variation. Under H1b the curves still separate
  when `d` is rescaled per site to equalise mean degree, and observed reach differs from reach on
  degree-preserving rewired versions of each graph.
- **Plausible data.** OSM building footprints for both sites via OSMnx, with a recorded snapshot
  date. Google Open Buildings and/or Microsoft footprints for a completeness cross-check.
- **Plausible method.** Project each site to its local UTM zone (EPSG:32737 for Nairobi; EPSG:32612
  for Phoenix or EPSG:32611 for Las Vegas). Build distance-band graphs with libpysal, convert to
  NetworkX. Inject errors at a grid of `p` values. Propagate with a multi-hop rule fixed in advance.
  Measure reach as the fraction of non-seeded nodes flagged, plus an amplification ratio (flagged per
  seeded node). Compare against (i) degree-matched thresholds and (ii) degree-preserving rewiring
  (`networkx.double_edge_swap`). Fit reach on `p`, site, and their interaction.
- **Main threat to validity.** A5 above: with two sites the "structure" effect cannot be separated
  from site identity. The rewiring null partly addresses this, because it changes layout while
  holding site and degree fixed. A second threat is a threshold artefact: one metric `d` may link
  nearly everything in Kibera and nearly nothing in the suburb (expected, not yet measured).
- **What would weaken or refute it.** H1a: site curves overlap within replicate variation across the
  tested `p` range. H1b: the site difference disappears under degree matching and observed graphs
  are indistinguishable from their rewired nulls. Either outcome is a reportable result.

### Rank 2. RQ2: Is the layout effect stable across proximity thresholds and graph definitions?

Best run as the built-in sensitivity analysis for RQ1 rather than a separate study.

- **Research question.** Do the sign and size of the site × `p` interaction hold across proximity
  thresholds and graph-construction rules, or do they depend on the scale chosen?
- **Candidate hypothesis.** H2: each site has a threshold range in which reach rises sharply as the
  graph becomes connected, and this range sits at smaller `d` in Kibera than in the suburb. Outside
  those ranges the between-site ordering is stable.
- **Expected observable evidence.** Reach and largest-connected-component share plotted against `d`
  show a sharp rise per site at different `d`. The ordering of sites is the same for fixed-distance,
  k-nearest-neighbour, and scale-normalised thresholds (for example multiples of each site's median
  nearest-neighbour distance).
- **Plausible data.** Same as RQ1.
- **Plausible method.** Sweep `d` and repeat the RQ1 experiment; repeat with kNN graphs and with
  centroid versus footprint-edge distance. Report the full curve, not one chosen threshold.
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

Adopt **RQ1 as the single primary question, with RQ2 as its mandatory sensitivity analysis**. That
fits the stated scope (two sites, one method, one error-injection experiment), and the degree-matched
and rewired nulls give the hypothesis a real chance of failing. Add RQ3 only if time allows; run the
RQ5 completeness check as a data-quality step regardless of which question is chosen.

## 7. Unresolved issues

- **U1. Is density part of "spatial structure" or a confound?** Determines whether H1a or H1b is the
  central claim. Needs a human decision.
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

1. Send this file to the Scientific Critic (packet 02) with U1 and the GeoAI-relevance concern
   flagged for explicit judgement.
2. In parallel, a cheap descriptive pull that commits to no hypothesis: download OSM building
   footprints for Kibera and one candidate suburb, record the snapshot date, project to UTM, and
   tabulate building counts and nearest-neighbour distance distributions. This resolves U6 and U7
   and shows whether a common threshold range exists at all.
