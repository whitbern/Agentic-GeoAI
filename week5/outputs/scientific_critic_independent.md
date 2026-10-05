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
