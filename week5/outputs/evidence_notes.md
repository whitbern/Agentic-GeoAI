# Evidence notes: what the supplied papers say

Date: 2026-10-04 (third batch added the same day; sections 12 to 19 and 23 are new)
Previous version: `outputs/history/evidence_notes_v1.md` (eleven papers)
Source files: `inputs/evidence/*.pdf` (read only; not modified)
Purpose: record what each paper states, with page references, so later stages can cite evidence
instead of the one-line summaries in `inputs/evidence/README.md`.

## How to read this file

- **Says** = stated in the paper on the pages listed. Page numbers are the journal's own, or the
  preprint's own where only a preprint was supplied.
- **Bearing** = the agent's inference about what it means for this project. Not in the paper.
- Pages read are listed per paper; anything outside them is unverified. Several papers were read
  only in part, and three of the third batch were read by keyword search around the passages
  that matter here. That is stated per paper.
- The third batch (sections 12 to 19) was read from text extracted with `pdftotext`. Figures were
  not read. Tables whose columns were scrambled by extraction are reported only where the paper's
  own prose repeats the value, or where the PDF page was checked by eye (stated where done).
- These papers concern lattices, model networks, street networks, species data, areal regression,
  footprint data quality, and building graphs used for classification. None studies label-error
  propagation on building-proximity graphs. Every application to this project is an analogy.

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

## 12. Ghadiri, Saramäki & Hiraoka (2026), "Epidemic reproduction numbers in spatial networks", arXiv:2603.22150v1

File: `2603.22150v1.pdf`. Pages read: 1–11 (whole preprint, text only; equations and figures were
partly garbled by extraction). Preprint dated 23 March 2026; no sign of peer review in the file.

**Says**

- Compares Erdős–Rényi graphs with random geometric graphs (RGGs). Both have a Poisson degree
  distribution, so "any difference in dynamics in these two network models arises from the
  presence of spatial structure" (p. 3).
- Process: continuous-time SIR and SI from one randomly chosen infected node, mapped to a static
  "epidemic percolation network". Transmission probability `T = β/(β+γ)` (p. 2).
- On a tree, the basic reproduction number is `R0 = T⟨k⟩` (Eq. 4, p. 3).
- Triangles lower `R0`: a neighbour of the first node can be reached by a two-step path before
  the direct link transmits, so paths "compete" (pp. 3–4; Fig. 1B).
- As `T⟨k⟩` grows, `R0` in the RGG approaches "approximately half" of its Erdős–Rényi value.
  "This is due to the spatiality alone, as the average degree, the degree distribution, the
  transmission probability, and the network size are the same for both graphs" (p. 6; Fig. 3C).
  Simulations use `N = 10,000` (Fig. 3 caption, p. 4).
- The main difference "arises from the contribution of loops of various lengths in transmitting
  the infection" (p. 6).
- Reproduction number per generation: decays exponentially on Erdős–Rényi graphs; on RGGs it
  falls faster at first and then stays near 1, because the number of nodes reached grows with the
  square of the generation number (pp. 6–7, 9; Fig. 4).
- A "heterogeneous spatial network" model varies spatiality (a temperature parameter) and degree
  heterogeneity (negative-binomial dispersion) separately. "Regardless of degree heterogeneity
  ... in a network with spatial structure, R_g goes to unity for large enough g" (p. 8; Fig. 5).
  These runs use `N = 5,000` and mean degree 6 (Fig. 5 caption, p. 9).

**Bearing**

- Supports the mechanism behind the arrangement effect: at the same degree distribution, short
  loops in a proximity graph waste transmission on nodes that are already reached. This is the
  human's stated use of the paper (README, Claim 1), and the summary there is accurate.
- The heterogeneous-network result is the closest supplied evidence that a spatial effect persists
  when degree heterogeneity is varied independently. It is shown for the per-generation
  reproduction number, not for a threshold.
- **Limit 1. No threshold result.** The paper reports reproduction numbers, not critical
  transmission probabilities. It does not show which graph has the lower critical `t`, so it does
  not settle the direction of `Q`.
- **Limit 2. Different quantity from the project's amplification factor.** `R0` here counts nodes
  infected *directly* by the first node in continuous time. In the project's one-shot cascade the
  seed attempts every neighbour, so the first step gives `t` times the degree on any graph, and
  loops matter only from the second step (agent inference). The paper's `R0` ratio therefore does
  not contradict the v7 statement that the arrangement effect tends to 1 at low `t`; Fig. 3C is
  described as diverging from 1 as `T⟨k⟩` grows, which points the same way.
- **Limit 3.** Erdős–Rényi against RGG is a comparison of two model families with the same degree
  distribution. It is not a degree-sequence-preserving rewiring of one given graph.
- **Limit 4.** Uniform random points only. Nothing on lattices, clustered patterns, or real
  building data.

## 13. Okyere, Lu & Brunn (2025), "Evaluating the Quality of Open Building Datasets for Mapping Urban Inequality", arXiv:2508.12872v1

File: `2508.12872v1.pdf`. Pages read: 1–25 (whole body, text only); appendix figure captions on
p. 26. Preprint. The prose and tables disagree with each other in places (noted below).

**Says**

- Compares Google and Microsoft machine-derived footprints with OSM in Accra, Nairobi, Caracas,
  Berlin, and Houston, "assuming OSM is closer to the ground truth" (pp. 1, 12).
- Study areas are metropolitan administrative units: "the Caracas and Nairobi district polygons",
  Harris County for Houston (p. 8). Nairobi is projected to UTM zone 37S (p. 11).
- **Google results are reported for Accra, Caracas, and Nairobi only** (Table 2, p. 15). Houston
  is assessed with Microsoft data only, and Berlin with a hybrid dataset (pp. 7, 21–22).
- Nairobi polygon counts: OSM 500,238; Microsoft 325,532 (Table 1, p. 14).
- Nairobi, Microsoft against OSM: 41.29% of Microsoft polygons overlap an OSM polygon; 74.87% of
  OSM polygons have no overlapping Microsoft polygon, the highest of the five cities; mean IoU of
  matched pairs 0.5165 (p. 14). Only overlaps covering at least 51% of the machine-derived polygon
  count as matches (p. 20).
- Nairobi, Google against OSM: 31.63% of Google polygons overlap; 59.18% of OSM polygons are
  unmatched; mean IoU 0.4038 (pp. 15–16).
- Houston, Microsoft against OSM: 89.39% of OSM polygons are overlapped; mean IoU 0.8415 (p. 14).
- Median Microsoft footprint area: Nairobi 111.34 m², Houston 216.52 m²; 10th percentile 30.60 m²
  and 86.13 m² (Table 3, p. 16).
- Stated limitations: five cities; "Dependency on OSM as a reference resource implies its
  correctness, which varies over different regions"; acknowledged model biases "especially in
  informal communities" (p. 24).

**Bearing**

- At city scale the OSM and Microsoft counts for Nairobi differ by about 35% of the OSM count
  (agent arithmetic from Table 1), with OSM the larger. The 30% trigger in the data gate
  (decision 6) is therefore plausible at Kibera. This is an inference from a city-wide figure;
  nothing in the paper is specific to Kibera or to any informal settlement.
- The matching results show that, in Nairobi, machine-derived and OSM polygons often do not
  describe the same objects. A count comparison then compares differently defined units. This
  supports the earlier caution that a count difference does not say which source is wrong.
- The paper reports Google-only results for neither of its Global North cities. Houston uses
  Microsoft data, and Berlin uses a hybrid said to draw on both providers. That is **consistent
  with** the ruling that the suburb gate uses OSM and Microsoft only. The paper does not state
  that Google Open Buildings lacks United States coverage; that remains unsourced.
- Footprint size distributions differ between Nairobi and Houston. A single minimum-area filter
  will remove different shares of nodes at the two sites, which bears on the node definition.
- **Limit 1.** OSM is assumed to be the reference. The paper cannot say which source is correct.
- **Limit 2. Internal inconsistencies.** The Nairobi total for Google in Table 2 (325,532) is
  identical to the Microsoft total in Table 1, and the Accra percentages in the prose on p. 15 do
  not match Table 2. The Google count for Nairobi is not used here. Values quoted above are those
  the prose and table agree on.
- **Limit 3.** README says the paper analyses "completeness in informal areas". The analysis is
  by hexagonal cells across whole metropolitan units; informal areas are not separated out in
  the pages read.

## 14. Herfort, Lautenbach, Porto de Albuquerque, Anderson & Zipf (2023), Nature Communications 14: 3985

File: `A_spatio-temporal_analysis_investigating_completen.pdf`. Pages read: 1–3 in full; passages on
pp. 6, 8, and 9 located by keyword search. Methods (pp. 9–12) and supplementary material unread.

**Says**

- A random-forest model predicts building area per 1 km² grid cell; OSM completeness is OSM
  building area over predicted area, for 13,189 urban centres (pp. 1, 3).
- OSM building completeness exceeds 80% in 1,848 urban centres and is below 20% in 9,163 (p. 1).
- Regional means of urban-centre completeness: North America 64%, Sub-Saharan Africa 30%, global
  24% (p. 3; Table 2).
- "more than 50% of all building edits in Sub-Saharan Africa were related to organized
  humanitarian mapping activities" (p. 3).
- Within cities, mapped grid cells cluster. **Las Vegas is named** as an example of a "divided
  city": "a few spatially clustered neighbourhoods which are mapped very well, whereas large
  parts of the city remain unmapped" (p. 6; Fig. 6, type 2a). For such cities "the overall
  completeness value hardly reflected the local completeness values" (p. 6).
- "For some places, the building footprints in OSM might already come from an imported dataset"
  (p. 9).
- Limitations: urban centres only; Microsoft footprints are part of the training data, and
  "the remaining biases of the Microsoft buildings dataset ... might lead to ... over-estimated
  OSM building completeness" (pp. 8–9). Model performance is assessed by 20-fold spatial
  cross-validation; grid-level r² is 0.67 for Sub-Saharan Africa and 0.70 for North America
  (Table 3, p. 9; other rows were not reliably extracted).

**Bearing**

- Direct support for checking completeness **within** a site and not only in total: a city-level
  or site-level figure can hide unmapped patches. The paper shows this at 1 km² resolution
  between neighbourhoods; applying it inside a study area well under 1 km² is an inference.
- **Relevant to the suburb.** Las Vegas is one of the two metros named in `inputs/problem.md`,
  and the paper describes its OSM building coverage as patchy. OSM building counts in a candidate
  suburb could be close to complete or close to zero. The footprint source at the suburb cannot
  be assumed to be OSM.
- The remark on imported footprints gives weak, general support to the independent review's
  concern that OSM and Microsoft may share an origin in some United States areas. It says
  nothing about any specific metro.
- **Limit.** Completeness is a model estimate at 1 km², partly trained on Microsoft footprints.
  It is not a building-by-building check, and it is not independent of the Microsoft data.

## 15. Yeboah et al. (2021), "Analysis of OpenStreetMap Data Quality at Different Stages of a Participatory Mapping Process", ISPRS International Journal of Geo-Information 10: 265

File: `ijgi-10-00265-v3.pdf`. Pages read: 1–11 in full; pp. 13–18 in part; Table 2 and Table 3 on
p. 14 checked by eye against the PDF page because the extracted columns were scrambled.
Appendix unread.

**Says**

- Seven slum sites in Bangladesh, Kenya, Nigeria, and Pakistan, mapped in two stages: remote
  tracing from satellite imagery (Stage 1), then field verification (Stage 2) (pp. 3, 5–6).
- **Sites are anonymised.** The two Kenyan sites are "Nairobi site 1", about 12 km from the
  central business district, and "Nairobi site 2", about 7 km from it; structures at both are
  described as iron sheet or tin with iron-sheet roofs (p. 7). **The word Kibera does not appear
  in the pages read.**
- Completeness at a time point is the share of buildings in the final, field-verified OSM state
  that were already present and never edited afterwards (Eq. 1, p. 10). It is measured against
  the project's own final map, not against an outside source.
- "the completeness achieved by remote mapping largely depends on the morphology and
  characteristics of slums such as building density and rooftop architecture, varying from 84% in
  the best case, to zero in the most difficult site" (abstract, p. 2).
- Table 2 and Table 3 (p. 14), checked by eye:

  | Site | Area (km²) | Buildings | Final density (per km²) | Stage 1 building completeness growth, count (area) |
  |---|---|---|---|---|
  | Nairobi site 1 | 0.5 | 6,444 | 12,888 | 32% (15%) |
  | Nairobi site 2 | 0.6 | 7,470 | 12,450 | 64% (64%) |
  | Dhaka site | 0.3 | 6,722 | 22,407 | 0% (0%) |
  | Karachi site | 0.4 | 2,566 | 6,415 | 0% (0%) |
  | Ibadan site 2 | 1.2 | 2,047 | 1,706 | 66% (60%) |

- Zero completeness at the Dhaka site is attributed to "extreme" building density, and at the
  Karachi site to complex rooftop architecture (p. 11).
- Completeness is achievable in remote mapping "for sites with morphology that are more regular
  with less building density" (p. 17).
- Completeness maps before remote mapping, after it, and after fieldwork are shown for two sites,
  Ibadan site 2 and Karachi (Figs. 7 and 8, p. 13). Figures were not read.
- Stated limitation: the scope is slums (p. 18).

**Bearing**

- Supports the ruling that completeness be checked inside the Kibera site. The paper shows that
  remote-mapping completeness falls with building density and roof complexity **between sites**.
  That it also falls in the densest parts **within** a site is an inference by analogy; the paper
  reports one figure per site.
- If that inference holds at Kibera, the buildings most likely to be missing are those in the
  densest patches. Those are the high-degree nodes in a distance-band graph, and decision 7 places
  the spatial concentration of high-degree nodes inside the arrangement effect. Uneven
  completeness would therefore bias the primary estimand, not only the node count (agent
  inference; this is the confound the README flags).
- **Density alone does not predict completeness.** The two Nairobi sites have nearly equal
  building density, and their Stage 1 completeness growth was 32% and 64%. A density map cannot
  stand in for a completeness check.
- Kibera's OSM data comes from community mapping on the ground (per `inputs/problem.md`; the Map
  Kibera documentation has not been supplied). The paper's low figures describe remote tracing
  **before** field verification, and its final state is 100% by construction. The paper therefore
  motivates the check; it does not predict its result.
- The two Nairobi sites give an order of magnitude for planning only: about 12,000 to 13,000
  buildings per km². Whether either site is Kibera is not stated, and the figure is not a
  measurement of the study area.
- **Limit.** No outside reference; seven sites; whether a mapped building is one structure is not
  tested.

## 16. Sun, Hu, Lakhanpal & Zhou (2023), "Spatial cross-validation for GeoAI"

File: `886644935-2023-GeoAIHandbook-SpatialCV.pdf`. Pages read: 1–12 (whole chapter text; preprint
pagination). Preprint of Chapter 10 in the *Handbook of Geospatial Artificial Intelligence*.

**Says**

- Random cross-validation "could lead to an overestimate of model performance on geographic
  data, due to the existence of spatial autocorrelation"; a model can be seen as having "peeked"
  at nearby validation data (pp. 1–2).
- Spatial cross-validation "is not always better". For within-area prediction, random
  cross-validation is preferred. For between-area prediction, spatial cross-validation "is likely
  to provide a more realistic evaluation". The choice "depends on which strategy better mimics
  the real application scenario" (pp. 2–3).
- Four families of method: clustering-based, grid-based, geo-attribute-based, and spatial
  leave-one-out with a buffer (pp. 3–7). Python implementations named: `spacv`, `MuseoToolbox`
  (p. 4).
- "There is no single best way for specifying the parameter values" such as grid size and buffer
  distance (p. 7).
- Two worked examples. Chicago domestic-violence rate, random forest, Moran's I 0.648: random
  cross-validation scores about 5% to 8% higher than the spatial methods. New York obesity
  prevalence, deep network, Moran's I 0.740: 8% to 31% higher (pp. 7–12).

**Bearing**

- README summary is accurate. This is a suitable primary citation for spatial-leakage control,
  alongside Roberts et al. (2017).
- The fallback learning component trains on synthetic patterns and tests on two real sites. In
  the chapter's terms that is between-area prediction with no shared space between training and
  test data, so the leakage the chapter describes cannot arise in that form. It also means the
  fallback gives no demonstration of leakage control, as decision 5 already states.
- If the sub-window form runs, grid-based splitting is the matching method. The chapter gives no
  rule for cell size.
- **Limit.** Two examples on areal census units. No graph or point-pattern case. `spacv` and
  `MuseoToolbox` are outside the libraries listed in `inputs/problem.md`.

## 17. Lei, Liu, Milojevic-Dupont & Biljecki (2024), Computers, Environment and Urban Systems 111: 102129

File: `2024-ceus-gnn-building.pdf` (accepted manuscript, 48 pages; manuscript pagination).
Pages read: 1, 15–16, and 28–30, located by keyword search. Results, discussion, and limitations
unread.

**Says**

- Predicts building attributes (storeys, type, construction period, material) with graph neural
  networks in Boston, Melbourne, and Helsinki (p. 1).
- "each building is conceptualised as a node"; each node "is linked to its 10 nearest buildings",
  with the number of neighbours treated as a tunable setting (p. 15).
- "the resultant graph is randomly divided into 70% for training and 30% for testing" (Fig. 4
  caption, pp. 15–16).
- A separate experiment in Boston replaces random sampling with sampling by local spatial
  autocorrelation class, because identical nearby buildings in both sets "could artificially
  inflate model accuracy" (pp. 28–29).

**Bearing**

- Precedent for building-level graphs in current GeoAI work. The edge rule is **k-nearest
  neighbours**, which is the project's robustness check (decision 2), not its primary
  distance-band rule.
- The main evaluation uses a random split on a spatial graph, and the authors themselves flag the
  inflation risk. It is an example of the leakage problem, not of its control.
- **Limit.** Nothing on propagation, thresholds, or rewiring. Read only in part.

## 18. Jia, Zhang & Narahara (2024), CAADRIA 2024, vol. 2: 39–48

File: `caadria2024_263.pdf`. Pages read: 39–48 (whole paper, text only).

**Says**

- Classifies residential building patterns with a graph convolutional network. The model is
  trained on 2,196 residential blocks (97,601 buildings) in Shenzhen and then applied to 1,742
  blocks in Hong Kong (pp. 41, 47).
- "we extract the centroid of each building's footprint as a node and connect all the nodes
  within a residential block" (p. 40). The connection method is **Delaunay triangulation**, with
  the Euclidean distance between centroids as edge weight (pp. 43–44).
- One graph per block. Blocks are labelled by volunteers as scattered, array, or mixed (p. 44).
- Reported accuracy 94.91% on the Shenzhen test split and 93.77% on Hong Kong (p. 47).

**Bearing**

- Precedent for footprint centroids as nodes.
- **Not a precedent for a distance threshold.** README says the graph connects centroids "within
  a distance" and locates the study in Hong Kong. The paper uses Delaunay triangulation inside
  each block, trains in Shenzhen, and tests in Hong Kong.
- A Delaunay graph has a mean degree near 6 whatever the point pattern (agent background
  knowledge, not from the paper). It would hold degree nearly constant by construction, which is
  why it is not interchangeable with the distance-band graph used here.
- **Limit.** Graphs stop at block boundaries. No propagation.

## 19. Liu & Song (2025), Land 14(7): 1469

File: `land-14-01469.pdf`. Pages read: 1 in full; passages on pp. 3 and 6–8 located by keyword
search. About 25 pages unread, including results and limitations.

**Says**

- "Our approach converts each cadastral plot into a graph whose nodes are building centroids and
  whose edges reflect Delaunay-based proximity" (abstract, p. 1).
- Applied to 8,973 plots in Nanjing's historic walled city, giving seven plot types (p. 1).
- Graphs are built only for plots with at least three buildings (p. 3). Delaunay edges longer
  than 200 m are removed (p. 7).
- Stated limitation: validated on one 2016 Nanjing dataset (p. 3).

**Bearing**

- A second precedent for centroid nodes with Delaunay edges, again one small graph per plot.
- Shows that a hybrid rule (Delaunay with a length cut-off) is in use. The project does not use
  one.
- **Limit.** Unsupervised classification; no propagation; read only in part. The README entry for
  this paper ends mid-sentence ("alternative"), so the human's intended use of it is not known.

## 20. Checks on the summaries in `inputs/evidence/README.md`

| README statement | Check against the paper |
|---|---|
| Ghadiri et al.: "Compares spread on Erdős–Rényi and random geometric graphs; attributes the difference mainly to loops of various lengths carrying transmission" | Accurate (p. 6). The quantity compared is a reproduction number, not outbreak size or a threshold. |
| Ghadiri et al.: "Theoretical basis for the arrangement mechanism" | Reasonable for the mechanism. It gives no threshold result and is a preprint. |
| Lei et al.: "GNNs used because they model spatial relationships between buildings" | Accurate (p. 1). Edges are 10 nearest neighbours; evaluation uses a random split. |
| Jia et al.: "connected within a distance; precedent for this study's graph construction (Hong Kong)" | Not accurate on two points. Edges are a Delaunay triangulation within each block. Training is in Shenzhen; Hong Kong is the test city. Centroid nodes are a precedent; the distance rule is not. |
| Liu & Song: "Building centroids as nodes with Delaunay-based proximity edges; alternative" | Accurate as far as it goes. The entry is cut off mid-sentence. |
| Sun et al.: "Random CV overestimates performance on geographic data due to spatial autocorrelation; compares four spatial CV methods" | Accurate. The chapter also says random cross-validation is preferred for within-area prediction (pp. 2–3). |
| Okyere et al.: "Compares Google and Microsoft AI-derived footprints, including Nairobi; analyzes polygon size distributions and completeness in informal areas" | Partly. Nairobi is included at district scale. Size percentiles are for Microsoft data. Informal areas are not analysed separately in the pages read. |
| Herfort et al.: "OSM building completeness across 13,189 urban areas" | Accurate. |
| Herfort et al.: "over half of Sub-Saharan African urban building data comes from humanitarian mapping" | Close. The paper says more than 50% of building *edits* in Sub-Saharan Africa relate to organised humanitarian mapping (p. 3). |
| Herfort et al.: "regions with large building stocks are often under-mapped" | Not located in the pages read. Unverified. |
| Yeboah et al.: "Remote-mapping completeness depends on slum morphology (building density, rooftop architecture), from 84% to zero across sites" | Accurate (abstract). The figures are for remote tracing before fieldwork. |
| Yeboah et al.: "Flags a potential confound: under-mapping may be systematically worse in dense areas, biasing degree" | The dependence on density is the paper's finding, across sites. The degree-bias confound is the human's inference, and a sound one. Within-site variation is not reported. The sites are anonymised; Kibera is not named. |
| Iotti et al.: "Spread on random geometric graphs vs. Watts–Strogatz and degree-preserving rewired versions" | Accurate. |
| Iotti et al.: "shows the spatial-vs-rewired difference is established" | Accurate that a difference is established. The direction is mixed within the paper, and the model is SIS, not a one-shot cascade. |
| Behnisch et al.: "Building locations connected within a distance threshold; critical distance for a spanning cluster. Precedent for building-point proximity graphs" | Accurate. |
| Percolation review: "anchors the per-site distance threshold choice" | Partly. It shows clustering distance is a standard control parameter; it gives no method for choosing one. |
| Cecchini et al.: false-positive and false-negative rates depend on shortest path length and detour degree; non-spatial | Accurate. These are link-inference errors, not node-label errors. |
| Suspicion scoring (first wording): "This is the inference mechanism the propagation rule represents" | Not accurate as stated. The paper's mechanism is neighbour averaging with relaxation labelling; the project's rule is an independent cascade. See section 10. |
| Suspicion scoring (revised by the human): supports the premise that erroneous seed labels degrade inference; "its averaging rule differs from this study's cascade rule; the propagation rule here is a stylised abstraction" | Accurate. |
| Iotti and Behnisch listed under both Claim 1/2 and Claim 3 | Behnisch does not address error in GeoAI (Claim 3). Iotti does not address variation across cities (Claim 2). |

## 21. Not supplied as PDFs

Still known only from titles and the human's summaries: Boeing (2017); Pastor-Satorras &
Vespignani (2001); Janowicz et al. (2020); Li et al. (2024); Anselin (1995); Openshaw (1984);
Bhavnani et al. (2014); Weidmann & Salehyan (2013); Weber (2016); Suchman (2020); Map Kibera
project documentation.

The Map Kibera documentation matters more after the third batch. Whether Kibera's OSM buildings
were traced remotely, surveyed on the ground, or both decides how far Yeboah et al. applies.

## 22. What the evidence changes

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
| Any evidence gap or novelty | Not assessable. No literature search has been done. Nineteen papers read, several only in part. The third batch adds further prior work on spread in spatial against non-spatial networks (Ghadiri et al.) and on building graphs (Lei; Jia; Liu & Song), which makes a novelty claim harder to support, not easier. |
| v7 threat: the arrangement effect is forced toward 1 at low `t` (agent derivation) | Consistent with Ghadiri et al. (p. 6; Fig. 3C as described in the text), for a related but different quantity. Still to be checked in the pilot. |
| Mechanism for an arrangement effect at fixed degree distribution | Supported for uniform random points against Erdős–Rényi graphs: short loops reduce onward transmission (Ghadiri pp. 3–6). Not shown for thresholds, for clustered patterns, or for rewirings of a given graph. |
| v7 5.5: mean clustering coefficient as an arrangement metric | Clustering of a random geometric graph is about 0.587 whatever the number of nodes (Barthélemy pp. 43–44; Iotti p. 2). The metric is therefore expected to be nearly flat along the random part of the gradient, as the independent review suggested. |
| Building-centroid graphs with a **distance threshold** | Precedent remains Behnisch et al. only. The three GeoAI building-graph papers use k-nearest neighbours (Lei) or Delaunay triangulation (Jia; Liu & Song). |
| v7 data gate: three footprint sources at both sites | Not supported for the suburb. The one supplied comparison that includes a United States city uses Microsoft data only there (Okyere pp. 14–15). Decision 7 sets two sources at the suburb. |
| v7 data gate: OSM as the default source at both sites | Weakened for the suburb. OSM building coverage in Las Vegas is described as a few well-mapped neighbourhoods among unmapped areas (Herfort p. 6). |
| v7 data gate: a count difference above 30% triggers a source switch | The trigger is plausible in Nairobi: city-wide, the Microsoft count is about 35% below the OSM count, and most OSM polygons have no matching Microsoft or Google polygon (Okyere pp. 14–16). A switch away from OSM on counts alone could move the study to the weaker source. |
| Completeness judged by a site total | Not sufficient. Site and city totals hide unmapped patches (Herfort p. 6), and remote-mapping completeness in slums varies with density and roof form (Yeboah pp. 2, 11, 14). |
| Human Claim 4: the data is adequate | Still not established. The third batch shows what must be checked; it measures nothing at either study site. |

## 23. Background statements in the independent review: sourced or still unsourced

The independent review (`outputs/scientific_critic_independent.md`) marked several statements
**[background, unverified]**. Status after the third batch:

| Review statement | Status |
|---|---|
| Bond percolation threshold of the four-neighbour square lattice is exactly 1/2 (issue 5) | **Sourced.** Barthélemy (2011), pp. 85–86. This is the value decision 7 uses to validate the estimator. |
| The cascade rule is equivalent to keeping each link with probability `t` (issue 5, point 4) | **Partly sourced.** Barthélemy p. 91 states the mapping of an SIR epidemic to bond percolation, valid when infection times are sharply peaked. The project's one-step rule fits that condition (agent inference). Confirm on one graph in the pilot. |
| Clustering coefficient is nearly constant on unstructured points (issue 12) | **Sourced** for random geometric graphs (Barthélemy pp. 43–44; Iotti p. 2). |
| Finite-size shifts scale roughly as `N^(-3/8)` for two-dimensional percolation and `N^(-1/3)` for random graphs (issue 1) | **Unsourced.** Nothing in `inputs/evidence/` states either exponent. Fagundes et al. (p. 1) and Behnisch et al. (p. 4) support only the general point that small systems give noisy, shifted estimates. |
| A fully rewired graph has threshold near `⟨k⟩ / (⟨k²⟩ − ⟨k⟩)` (issue 2) | **Unsourced.** Pastor-Satorras & Vespignani (2001) is listed in the README but not supplied, and it concerns a different process. |
| An eight-neighbour square lattice has a bond threshold near 0.25 (issue 2) | **Unsourced.** The reviewer called it a recollection. |
| Graphs in which high-degree nodes link to each other percolate earlier (issue 4) | **Unsourced.** Iotti p. 3 says only that degree-preserving rewiring leaves assortativity free to change, which supports reporting it. |
| Google Open Buildings does not cover the United States (issue 6) | **Unsourced, but consistent with** Sirko et al. (Africa only in the 2021 paper) and Okyere et al. (no Google-only results outside the Global South). |
| OSM buildings in many United States areas were bulk-imported from Microsoft or county data (issue 6) | **Unsourced for any metro.** Herfort p. 9 says only that some OSM footprints "might already come from an imported dataset". |

No design choice in `outputs/research_questions.md` v8 depends on an unsourced row. Where an
unsourced statement motivates a check, the check is listed there as a concern for the methodology
stage, with the need for a source stated.
