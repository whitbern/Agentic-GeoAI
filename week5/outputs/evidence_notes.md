# Evidence notes: what the supplied papers say

Date: 2026-10-04
Source files: `inputs/evidence/*.pdf` (read only; not modified)
Purpose: record what each paper states, with page references, so later stages can cite evidence
instead of the one-line summaries in `inputs/evidence/README.md`.

## How to read this file

- **Says** = stated in the paper on the pages listed. Page numbers are the journal's own.
- **Bearing** = the agent's inference about what it means for this project. Not in the paper.
- No paper was read in full. Pages read are listed per paper; anything outside them is unverified.
- These papers concern lattices, model networks, street networks, species data, and areal
  regression. None studies building-proximity graphs or label-error propagation. Every application
  to this project is an analogy.

## 1. Watts & Strogatz (1998), Nature 393: 440–442

File: `30918.pdf`. Pages read: 440–442 (whole letter).

**Says**

- Starts from a ring lattice of `n` vertices with `k` edges each and rewires each edge at random
  with probability `p`, "without altering the number of vertices or edges" (p. 440; Fig. 1, p. 441).
- The lattice is highly clustered with path length growing linearly in `n` (`L ~ n/2k`); the fully
  random graph is poorly clustered with path length growing logarithmically (`L ~ ln n / ln k`)
  (p. 440).
- A few rewired "short cuts" collapse path length while clustering stays near the lattice value
  (pp. 440–441; Fig. 2).
- Disease model: one infective individual at time 0; infectives are removed after one time unit;
  during that unit each infects each healthy neighbour with probability `r` (p. 441).
- The infectiousness at which half the population is infected falls quickly as rewiring increases,
  and the time to global infection tracks path length (p. 442; Fig. 3). "Infectious diseases are
  predicted to spread much more easily and quickly in a small world" (p. 442).
- Western US power grid, a physically built network: path length 18.7 against 12.4 for a random
  graph of the same size and mean degree; clustering 0.080 against 0.005 (Table 1, p. 441).

**Bearing**

- The disease model is structurally the per-link transmission rule the human chose (decision D1),
  so that rule has a precedent in the supplied evidence.
- It supports the direction the critic predicted for the rewiring test: a lattice-like graph
  spreads less than its rewired counterpart. The critic's claim is no longer unverified inference,
  though it is verified only for ring lattices.
- **Limit.** This rewiring keeps the number of edges but not each node's degree. The project's
  null keeps the full degree sequence, which this paper does not test.

## 2. Barthélemy (2011), "Spatial networks", Physics Reports 499: 1–101

File: `Barthelemy2011.pdf`. Pages read: 1–4, 42–44, 50–51, 84–86, 89–92. About 85 pages unread,
including section 2.2 (measures for spatial networks) and 3.2.1 (road and street networks).

**Says**

- A random geometric graph links points that lie within a set distance; it is the standard model
  for proximity graphs (p. 42).
- For such a graph there is a critical mean degree above which a giant connected component
  exists, with a fitted form `⟨k⟩c = 1 + b·d^(−γ)`, `b = 11.78`, `γ = 1.74`, `d` the dimension
  (p. 43).
- With uniform point density the degree distribution is Poisson-like and decays fast. With
  non-uniform density the degree distribution follows the local density, and "large density
  fluctuations can lead to spatial scale-free networks" (p. 43).
- Mean clustering of a two-dimensional random geometric graph is `1 − 3√3/4π ≈ 0.587`, independent
  of the number of nodes, against `~1/N` for a random graph. The stated reason holds "for most
  spatial graphs": long links are prohibited or rare (pp. 43–44).
- In spatial lattices path length scales as `N^(1/d)`; with enough long links it becomes `log N`
  (pp. 50–51).
- Bond percolation on a two-dimensional square lattice has threshold `p_c = 1/2`. Adding shortcuts
  lowers the threshold and changes the behaviour to that of a random graph (pp. 85–86; Fig. 77).
- An SIR epidemic on a graph maps to bond percolation on that graph, valid when infection times
  are sharply peaked. If the transmission probability exceeds the percolation threshold "an
  extensive number of nodes were infected" (p. 91).
- On a lattice above threshold, large regions stay unaffected; a small fraction of long-range
  links "is enough to 'homogenize' the system" and lowers the epidemic threshold (p. 91; Fig. 82).

**Bearing**

- Plugging `d = 2` into the p. 43 formula gives a critical mean degree of about 4.5 (agent
  arithmetic). A target mean degree below this would leave a uniform random pattern fragmented
  even with certain transmission, which constrains the choice of target degree.
- The p. 43 statement on non-uniform density supports critic issue 3: uneven packing of buildings
  appears as uneven degree, which the human's decision 2 counts as density.
- The percolation mapping means the chosen propagation rule has a **threshold in `t`** for each
  graph. Below it, the number flagged per seed is small and local. Above it, the number flagged
  scales with the size of the graph. The amplification factor therefore behaves very differently
  on either side, and rewired graphs are expected to have a lower threshold than spatial ones.
- **Limit.** The percolation results are for lattices and Watts–Strogatz models, not for irregular
  point patterns or degree-preserving rewiring.

## 3. Boeing (2019), "Urban spatial order", Applied Network Science 4:67

File: `Urban_Spatial_Order_Boeing_2019.pdf`. Pages read: 1–10 of 19. Open access (CC BY 4.0).

**Says**

- Measures street-network order for 100 cities from OpenStreetMap with OSMnx, at roughly municipal
  extent (pp. 4–5).
- Indicators: orientation entropy `H_o` of street bearings in 36 bins; orientation-order `φ` from
  0 (bearings uniform in all directions) to 1 (a single perfect grid); median segment length;
  circuity; average node degree; proportions of dead-ends and four-way intersections (pp. 5–6).
- Table 1 values (pp. 7–9):

  | City | φ | H_o | Avg node degree | Dead-ends | Four-way |
  |---|---|---|---|---|---|
  | Nairobi | 0.014 | 3.568 | 2.506 | 0.279 | 0.075 |
  | Las Vegas | 0.542 | 2.874 | 2.676 | 0.230 | 0.166 |
  | Phoenix | 0.586 | 2.801 | 2.795 | 0.186 | 0.171 |
  | US/Canada mean (Table 2, p. 10) | 0.427 | 3.003 | 3.090 | 0.116 | 0.334 |

- US and Canadian cities are on average the most grid-like; "spatial order" is described as a
  fuzzy concept, and visual order should not be confused with functional order (pp. 3, 9).

**Bearing**

- Supports the human's Claim 2 that order varies measurably between cities, **for streets at city
  scale**. It contains nothing on building point patterns, on Kibera, or on any specific suburb.
- Las Vegas and Phoenix are strongly oriented but have dead-end shares well above the US/Canada
  mean and four-way shares about half of it. City-wide, they are aligned but not well-connected
  grids. A "planned suburban grid" should be chosen from measured neighbourhood-level indicators,
  not assumed from the city.
- The entropy construction could be adapted to building or local street orientations as an
  arrangement metric. That adaptation is an agent proposal.

## 4. Zhang, Song, Luo & Wu (2023), "Geocomplexity explains spatial errors", IJGIS 37(7): 1449–1469

File: `Geocomplexity explains spatial errors.pdf`. Pages read: 1449–1457 (through methods). Results
section unread; the headline numbers below are from the abstract. Open access (CC BY-NC-ND).

**Says**

- Proposes a "spatial local complexity" indicator built from Moran-type local statistics on an
  area and its neighbours (pp. 1450–1453).
- Case study: regression models of income inequality (Gini coefficient) across Australian SA3
  statistical areas, using linear regression, support vector regression, and geographically
  weighted regression (pp. 1453–1457).
- Abstract: the indicator explains 17%–47% of errors in the aspatial models and 14% in the spatial
  model (p. 1449).

**Bearing**

- This is about **residuals of regression models on areal units**. It is not about classification
  errors, networks, or propagation. It supports the general idea that local spatial pattern is
  associated with model error; it is weak support for the human's Claim 3 as applied here.

## 5. Roberts et al. (2017), Ecography 40: 913–929

File: `487747661-Roberts-et-al-2017-Ecography.pdf`. Pages read: 913–920 of 17.

**Says**

- Random cross-validation on structured data underestimates predictive error; this persists, to a
  lesser extent, even when models with spatial terms are used (abstract; pp. 915–916).
- Block cross-validation is recommended wherever dependence structures exist (abstract, p. 913).
- In their simulation, blocks need to be "substantially larger than the range of the spatial
  autocorrelation in the model residuals" (Box 1, p. 918). The residual autocorrelation extent is
  a minimum, because structural overfitting can hide autocorrelation (pp. 919–920).
- Blocking can force extrapolation in predictor space and give overly pessimistic errors when
  only interpolation is intended; the modelling objective decides which is appropriate (pp. 915,
  917; Table 1, p. 914).
- To keep training data, each block can be its own fold (p. 920).

**Bearing**

- Directly supports the validation design for the sub-window model (decision D5): spatial blocks
  sized from residual autocorrelation, never random folds.
- Leave-one-site-out is extrapolation in the paper's terms and should be reported as such.
- **Limit.** Evidence is from ecological data and simulations.

## 6. Sirko et al. (2021), "Continental-scale building detection", arXiv:2107.12283

File: `Continental-Scale_Building_Detection_from_High_Res.pdf`. Pages read: 1–4. Accuracy results
unread.

**Says**

- Describes the pipeline behind Open Buildings: 516M footprints across Africa from 50 cm
  satellite imagery (abstract, p. 1).
- Named challenges include "settlements with many contiguous buildings not having clear
  delineations" and small buildings only "a few pixels wide" (p. 1).
- Labelling used a "dense building" class where an annotator "was not able to ascertain the exact
  boundary between individual buildings"; Fig. 3 shows such a case (p. 4).
- Evaluation regions included informal settlements (p. 4).

**Bearing**

- Open Buildings is least certain exactly where this project needs it: tightly packed informal
  settlement. Disagreement with OSM in Kibera would not show which source is wrong. This supports
  critic issue 8 and the human's decision to make node definition a pre-analysis gate.
- The human's Claim 4 ("the data is adequate") is not established by this paper. It needs the
  per-site comparison.

## 7. Iotti et al. (2017), "Infection dynamics on spatial small-world network models", Physical Review E 96, 052316

File: `PhysRevE.96.052316.pdf`. Pages read: 1–7 (whole paper). Added in the second batch.

**Says**

- Builds random geometric graphs (RGGs): `N` nodes uniform in the unit square, linked if closer
  than `R`; mean degree `πNR²`, Poisson degree distribution, clustering near 0.5865 (p. 2). Also
  builds "REDS" networks, a cost-constrained spatial variant with more skewed degrees (pp. 2–3).
- Applies two rewiring schemes at probability `p` per edge: the "standard" Watts–Strogatz scheme,
  which keeps mean degree but pushes the degree distribution toward Poisson, and a "conservative"
  scheme that preserves the degree distribution (p. 3).
- Warns against the simpler degree-preserving protocol that "crosses over" pairs of edges: in a
  spatial network the two new edges tend to join the same two regions, "introducing additional
  unwanted correlation structure" (p. 3). The conservative scheme was designed to avoid this.
- Degree-preserving rewiring leaves clustering, path length, assortativity, and community
  structure free to change, so comparing schemes separates "the influence of degree distribution
  from that of network structure more generally" (p. 3).
- Epidemic model: **SIS**, with infection probability `β` and recovery after one step; 50 of 1,000
  nodes infected at the start; mean degree 8; 100 networks per setting (p. 4).
- The estimated critical `β` is nearly flat at low rewiring and **rises** once rewiring exceeds the
  small-world range: "the network becomes less vulnerable to contagion when edges are randomized
  to a significant extent" (pp. 4–5; Fig. 3). The rise is smaller under conservative rewiring.
- For outbreak size and success rate the paper also says "rewiring makes disease spreading easier
  in both networks" (p. 5; Figs. 4, 5), and "RGGs seem insensitive to the differences between the
  rewiring schemes" (p. 5).

**Bearing**

- Direct methods precedent for comparing a proximity graph with a degree-preserving rewired
  version at matched mean degree. The lattice-or-RGG versus rewired comparison is established and
  is not a finding of this project.
- The p. 3 warning applies to `networkx.double_edge_swap`, which is an edge-crossing protocol.
  The warning is made for partial rewiring. Whether it matters for a fully mixed null (many swaps
  per edge) is not addressed in the paper; mixing should be checked.
- **The direction is not settled by this paper.** Its threshold result (rewiring raises the
  critical value) and its outbreak-size result (rewiring eases spread) point different ways, and
  the threshold result is opposite in sign to Barthélemy's lattice percolation result.
- **Limit.** SIS allows reinfection and reaches a steady state. The project's rule is a one-shot
  cascade. Thresholds for the two processes are governed by different graph properties (agent
  background knowledge, not from the paper), so these results do not transfer directly.

## 8. Behnisch et al. (2019), "Settlement percolation", Landscape and Urban Planning 191, 103631

File: `1-s2.0-S0169204618310661-main.pdf`. Pages read: 1–8 (whole paper).

**Says**

- Data: 48,791,451 building polygons for Germany from the official cadastre, after removing
  polygons under 10 m² and atypical polygons (p. 2).
- Median floor area 66.4 m²; median nearest-neighbour distance 9.61 m, mean 12.73 m, 90% below
  21.15 m (p. 2).
- Method: buildings are represented by the centre of the footprint, "disregarding the spatial
  extent"; any two within a clustering distance `l` join the same cluster (p. 2).
- A country-spanning cluster appears at a critical distance of (830 ± 10) m, identified from peaks
  in the sizes of the second, third, and fourth largest clusters (p. 4; Fig. 4).
- Robustness check: repeating the analysis without buildings under 50 m² gives almost the same
  clusters (p. 4).
- Critical distance varies by state from 60 m to 1,450 m; for the small city states "the peaks in
  their curves are noisy" and differences are "not meaningful" (p. 4; Table 1).
- The critical distance "contains different information than e.g. the average density"; it "takes
  the spatial organization of the buildings ... into account" (p. 5).
- Data inaccuracies cannot be excluded, and strong density inhomogeneity may cause problems
  (p. 6).

**Bearing**

- Precedent for building-centroid proximity graphs, for a minimum-area filter in the node
  definition, and for testing sensitivity to that filter.
- The second-largest-cluster peak is a usable method for estimating a critical value on real
  data. Here it would be applied to transmission probability `t`, not distance; that transfer is
  an agent inference.
- Small areas give noisy critical estimates. Kibera and a single suburb are small, so estimated
  critical `t` will carry finite-size uncertainty.
- **Limit.** This is percolation in the distance threshold over a whole country, with no
  propagation process and no degree control. The 830 m figure has no bearing on the per-site
  threshold `d`, which operates at the scale of neighbouring buildings.

## 9. Fagundes, Li, Ribeiro & Rybski (2025), "A Review of Percolation-like Transitions in Cities and Landscapes", Findings

File: `150358-a-review-of-percolation-like-transitions-in-cities-and-landscapes.pdf`. Pages read:
1–8 (whole article; body is pp. 1–4). Open access.

**Says**

- In urban studies "most works use the (a) clustering distance, (b) some sort of density, or (c)
  attributes of network edges as control parameters", with cluster sizes, their entropy, or the
  number of clusters as order parameters (p. 2; Table 1, p. 3).
- "The strict theory applies only to the limit of infinite system size" (p. 1).
- Percolation theory is "primarily based on randomness"; real systems have more complex spatial
  properties, and the differences need more research (p. 4).

**Bearing**

- Supports treating a distance threshold as a standard control parameter and reporting it. It
  gives no rule for choosing a per-site threshold.
- The infinite-size caveat reinforces the finite-size caution above.
- None of the twelve studies in its Table 1 is described as studying error or label propagation on
  building graphs. That is an observation about one short review, not evidence of a gap.

## 10. Macskassy & Provost (2005), "Suspicion scoring based on guilt-by-association, collective inference, and focused data access"

File: `CPP-04-05.pdf`. Pages read: 1–6 (whole paper). NYU working paper; footnote says it was to
appear at the International Conference on Intelligence Analysis, 2005.

**Says**

- Ranks people by estimated likelihood of being malicious in networks "linked by communications,
  meetings, or other associations (e.g., being in the same vicinity at the same time)" (p. 1).
- Relational-neighbour classifier: a person's score is the **weighted average** of the scores of
  their known associates, normalised by the sum of link weights (p. 2).
- Collective inference: all unknown scores are updated together by relaxation labelling for 100
  iterations; known-malicious people are fixed at 1 and others start at 0.01 (p. 2).
- Evaluated on synthetic data from a simulator built under a US Department of Defense programme;
  the authors "do not claim that the data fully replicate" real collection (p. 3).
- Some data sets contain people falsely tagged as malicious ("False bad", Table 3, p. 3).
- Performance is robust to moderate label noise but collapses when most initial labels are wrong
  (pp. 3–4; Fig. 1c). With high noise the scores "all end up with ranking much worse than random".
- "The static labels and the network structure would dominate the final scores to the point where
  initial priors had no effect" (p. 6).
- The reported performance "would not support direct action on all data sets" (abstract, p. 1).

**Bearing**

- This is the first supplied evidence of a concrete association-based scoring mechanism, and it
  shows erroneous seed labels degrading results through the network. It supports the premise of
  the project.
- **It is not the mechanism the project simulates.** The paper's rule averages over neighbours, so
  one erroneous neighbour's influence on a node shrinks as that node's degree grows. The project's
  cascade gives each link an independent chance to transmit, so exposure grows with degree. The
  two rules respond to degree in opposite directions. The project's rule is a stylised abstraction
  of guilt-by-association, not an implementation of this method.
- **Limit.** Nodes are people linked by communications and co-presence events, not buildings
  linked by residential proximity. The network is not spatial. Data is synthetic.

## 11. Cecchini, Cestnik & Pikovsky (2020), "Impact of network characteristics on network reconstruction", arXiv:2008.05886v1

File: `2008.05886v1.pdf`. Pages read: 1–8 (whole preprint). The README cites a 2021 Physical Review
E version under a slightly different title; only the arXiv preprint was read.

**Says**

- Studies errors in **inferring links** from time series of coupled oscillators: false-positive
  and false-negative link conclusions (pp. 1, 4).
- Simulations use Erdős–Rényi networks with 100 nodes and connection probability 0.15 (p. 4).
- False conclusions are more frequent for node pairs with shorter (indirect) shortest path length
  and higher "detour degree", the number of two-step paths between the pair (p. 4; Fig. 2).

**Bearing**

- Supports the general statement that local network structure affects inference error rates.
- **Limit.** The errors are about whether a link exists, not about node labels, and nothing
  propagates. Networks are non-spatial random graphs. Relevance to this project is loose.

## 12. Checks on the summaries in `inputs/evidence/README.md`

| README statement | Check against the paper |
|---|---|
| Iotti et al.: "Spread on random geometric graphs vs. Watts–Strogatz and degree-preserving rewired versions" | Accurate. |
| Iotti et al.: "shows the spatial-vs-rewired difference is established" | Accurate that a difference is established. The direction is mixed within the paper, and the model is SIS, not a one-shot cascade. |
| Behnisch et al.: "Building locations connected within a distance threshold; critical distance for a spanning cluster. Precedent for building-point proximity graphs" | Accurate. |
| Percolation review: "anchors the per-site distance threshold choice" | Partly. It shows clustering distance is a standard control parameter; it gives no method for choosing one. |
| Cecchini et al.: false-positive and false-negative rates depend on shortest path length and detour degree; non-spatial | Accurate. These are link-inference errors, not node-label errors. |
| Suspicion scoring (first wording): "This is the inference mechanism the propagation rule represents" | Not accurate as stated. The paper's mechanism is neighbour averaging with relaxation labelling; the project's rule is an independent cascade. See section 10. |
| Suspicion scoring (revised by the human): supports the premise that erroneous seed labels degrade inference; "its averaging rule differs from this study's cascade rule; the propagation rule here is a stylised abstraction" | Accurate. |
| Iotti and Behnisch listed under both Claim 1/2 and Claim 3 | Behnisch does not address error in GeoAI (Claim 3). Iotti does not address variation across cities (Claim 2). |

## 13. Not supplied as PDFs

Still known only from titles and the human's summaries: Boeing (2017); Pastor-Satorras &
Vespignani (2001); Janowicz et al. (2020); Li et al. (2024); Anselin (1995); Openshaw (1984);
Bhavnani et al. (2014); Weidmann & Salehyan (2013); Weber (2016); Suchman (2020); Map Kibera
project documentation.

## 14. What the evidence changes

| Earlier statement | Status after reading |
|---|---|
| Critic issue 2: rewired graphs will spread more than spatial ones | Supported for lattices under percolation-type spread (Watts & Strogatz pp. 441–442; Barthélemy pp. 86, 91). Mixed for random geometric graphs under SIS (Iotti pp. 4–5). Untested for a one-shot cascade on degree-preserving rewirings of irregular proximity graphs. |
| v4 expectation that rewired graphs have the lower critical `t` | Rests on Barthélemy's lattice result only. Iotti's SIS threshold moves the other way. Must be measured in the synthetic pilot, not assumed. |
| Rewired null via `networkx.double_edge_swap` | A spatial-correlation bias is documented for edge-crossing under partial rewiring (Iotti p. 3). Mixing needs checking. |
| Critic issue 3: uneven packing shows up as uneven degree | Supported (Barthélemy p. 43). |
| Critic issue 8: building units may not be comparable | Risk supported for the comparison dataset (Sirko pp. 1, 4). Minimum-area filtering and a sensitivity check have precedent (Behnisch pp. 2, 4). Not measured at either site. |
| Critic issue 11: spreading on spatial networks is well studied | Supported, now including degree-preserving rewiring of proximity graphs (Iotti). |
| v4 assumption A4: no supplied evidence describes a real propagation rule | Superseded. One documented research system exists (Macskassy & Provost). It uses a different rule from the project's. |
| Human Claim 2: structure varies measurably across cities | Supported for streets at city scale only (Boeing). |
| Human Claim 3: spatial structure produces systematic error in GeoAI | Weak, indirect support (Zhang et al.; Cecchini et al.). |
| Human Claim 4: the data is adequate | Not established. |
| Human Claim 5: association-based inference propagates suspicion through networks | Supported for one synthetic-data research system (Macskassy & Provost), with a mechanism that differs from the project's rule. |
| Any evidence gap or novelty | Not assessable. No literature search has been done. Eleven papers read, several only in part. |
