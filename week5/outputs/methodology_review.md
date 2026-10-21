# Scientific review of the methodology (v1, second draft)

Stage: 05 Methodology Critic
Packet: `outputs/prompt_packets/05_methodology_critic.md` (regenerated 2026-10-05)
Date: 2026-10-05
Target: `outputs/methodology_v1.md` (second draft, after human decision 8)
Verdict: **REVISE** (the design can test the question; seven specification gaps should be closed
before the pilot fixes operating values)

Nothing in this review is a finding. No data was examined and no code was run.

## 0. Independence statement

- **This review is not independent.** It was written in the same conversation, by the same
  agent, that wrote the methodology. The reviewer knew the reasoning behind every choice and had
  read `outputs/evidence_notes.md`.
- It is also written by an LLM of the same family as every earlier artifact. Under AGENTS.md
  rule 4, agreement between this review and the methodology is not scientific validation.
- Points where the review disagrees with the methodology and gives a checkable reason deserve
  more weight than points where it agrees.
- An independent rerun, in a fresh conversation given only packet 05, has not been done. The
  earlier stage did one (`outputs/scientific_critic_independent.md`), and it found issues the
  first review missed.

## 1. Inputs used

| Input | Use |
|---|---|
| `outputs/methodology_v1.md`, second draft (as embedded in packet 05) | The object under review |
| `outputs/research_questions.md` v8 (as embedded) | The frozen question, estimand, and decision rule the methodology must serve |
| `outputs/evidence_notes.md` | Known to the reviewer from the methodology stage. Not in the packet. Used only to check that citations match the notes |
| Human decision 8 | As quoted in the methodology, section 0 |
| Reviewer arithmetic and background knowledge | Marked **[reviewer arithmetic]** or **[background, unverified]** wherever used |

## 2. Assumptions made by this review

- **R1.** The frozen items in v8 (question, `D`, margin, decision table) and human decisions 1 to
  8 are not reviewed. Only their operationalisation is.
- **R2.** Every number I give is rough arithmetic, meant to show whether an issue is large enough
  to matter. None is a result.
- **R3.** Where a recommendation would add work or touch a ruling, I say so and leave the choice
  to the human.

## 3. Scores

| Dimension | Score (1–5) | Basis |
|---|---|---|
| Importance | 3 | Unchanged from the question review. The link to any real scoring system is by analogy (v8 A4) |
| Novelty / evidence gap | 2 | Not established. No literature search. The methodology makes no novelty claim, which is correct |
| Testability | 3 | The rule is fixed in advance. It is not shown that every row of the table can be reached at the sizes involved (issue 1), and the flags protect only one row (issue 2) |
| Identification strategy | 3 | Rewiring and the gradient are sound. The interval that decides the result is not yet a working method (issue 3) |
| Data adequacy | 2 | Nothing measured. One window of Kibera now stands for the site (decision 8) |
| Spatial validity | 4 | CRS, edge rule, autocorrelation, MAUP, and leakage are each handled or honestly marked as not demonstrated. Two geometric definitions are incomplete (issues 4, 5) |
| Robustness / validation | 3 | Good known-answer benchmarks. The validated case differs from every analysed graph (issue 8) |
| Feasibility | 2 | The bootstrap calibration as written is far beyond a semester's computation (issue 6) |

## 4. Issues

Ordered by severity. Evidence and inference are separated inside each item.

### Issue 1. It is not shown that the decision table's rows can be reached

- **LOCATION / TARGET:** 14.3; 12.1; 14.1 step 5. No precision analysis appears anywhere.
- **SEVERITY:** major (falsifiability).
- **WHY IT MATTERS:**
  - *Evidence from the methodology:* the reading depends only on whether the interval for `D`
    lies wholly inside, wholly outside, or across ±log 1.25, about ±0.223.
  - *Reviewer arithmetic, assuming a roughly symmetric 95% interval:* the "refuted" row needs a
    half-width below 0.223 even when `D` is exactly zero. That needs a standard error of `D`
    below about 0.11, or about 0.08 for `log Q` at each site if the two are similar.
  - *Inference:* if `log Q` at `N*` is noisier than that, the study can never refute H1-T. It
    can then only support it or be inconclusive, and the hypothesis is not falsifiable in
    practice.
  - Decision 8 makes this more pressing. A square that fits inside the settlement will hold
    fewer buildings than the whole settlement, and critical values on small areas are noisy
    (Behnisch et al. p. 4, as recorded in the notes).
- **EVIDENCE NEEDED:** the standard deviation of `log Q` across independent realisations at
  `N*`, for one pattern per segment. The pilot already computes it for the bootstrap
  calibration.
- **RECOMMENDED ACTION:**
  1. Add a pilot output, written before the freeze: for each segment, the smallest `|D|` that
     could be read as "supports" and whether "refuted" is reachable at all.
  2. If "refuted" is unreachable, say so in the protocol and in the write-up before any real
     result exists. This changes no ruling.
  3. Report it to the human at the freeze. No minimum `N*` is proposed here, because decision 8
     fixes the window.

### Issue 2. The qualifying flags guard "supports" and not "refuted"

- **LOCATION / TARGET:** 14.4; 12.4 reading rule.
- **SEVERITY:** major.
- **WHY IT MATTERS:**
  - *Evidence from the methodology:* a result is "not attributable to arrangement" if `D`
    changes sign across node definitions or sources.
  - *Inference:* when `D` is near zero, which is the "refuted" case, its sign will flip between
    variants by chance. Every refutation would then carry the flag, and the flag would mean
    nothing there.
  - *Inference:* the check that matters for a refutation is different: whether any sensitivity
    variant puts `D` outside the margin.
  - The flags as written can weaken a supporting result and cannot weaken a refuting one in a
    meaningful way. That is an asymmetry in how the hypothesis can fail.
- **EVIDENCE NEEDED:** none; this is a specification gap.
- **RECOMMENDED ACTION:** state the flag per row. For "supports": the sign or the row changes in
  any variant. For "refuted": the interval of any variant reaches a margin. This refines flags
  that decision 8 approved, so it should be shown to the human.

### Issue 3. Neither bootstrap scheme is likely to pass its own calibration, and the calibration tests the wrong property

- **LOCATION / TARGET:** 12.1, schemes W and T, MD26.
- **SEVERITY:** major. It decides whether the study can be anything other than inconclusive.
- **WHY IT MATTERS:**
  - *Inference on scheme W:* the graph is unchanged in every resample. Reweighting which core
    nodes are counted rescales cluster sizes but hardly moves the `t` at which the second-largest
    cluster peaks. In the rewired graphs, block membership has no spatial meaning, so the
    weights there are close to random noise. I expect an interval that is much too narrow. The
    methodology names this weakness and does not say how likely it is.
  - *Inference on scheme T:* joins between tiles add and remove neighbour pairs along every
    seam. With 25 tiles a large share of nodes sit near a seam. That shifts `t_c` in every
    resample in the same direction, so the bootstrap distribution may be centred away from the
    estimate.
  - *Gap in MD26:* the calibration compares standard deviations only. A percentile interval
    from a shifted distribution has the right width and the wrong position, and the decision
    table reads position.
  - *Gap in MD26:* calibration is checked for `log Q` on one pattern. The rule is applied to
    `D`, a difference of two sites that may behave differently.
  - If both schemes fail, the approved cap makes the study inconclusive whatever the data show.
    That is an acceptable outcome under decision 6, but it should be an expected one, stated in
    advance, and not a surprise at the end.
- **EVIDENCE NEEDED:** from the pilot, for each scheme and segment: the ratio of standard
  deviations already planned, and the mean of the bootstrap values minus the estimate.
- **RECOMMENDED ACTION:**
  1. Add a bias term to the calibration: the shift must be small relative to the bootstrap
     standard deviation. The fraction is a convention to fix now.
  2. State in the protocol that an uncalibrated bootstrap is a likely outcome.
  3. For the human, since it touches decision 7: whether a third candidate may be defined before
     the pilot. Deleting one block at a time and re-estimating on the remaining graph makes no
     seams and does change the wiring. It is a jackknife, not a bootstrap, so it needs a ruling.
     The equal-count windows in the "companion" paragraph are the other option the independent
     review preferred.

### Issue 4. The core, and so `N*`, changes whenever `d` changes

- **LOCATION / TARGET:** 5.1 (MD2 and the note on breaking the circle); 8.4; MD16; 12.4.
- **SEVERITY:** major (specification gap; cheap to fix).
- **WHY IT MATTERS:**
  - *Evidence from the methodology:* the core is the window shrunk by `w`; `w` is a multiple of
    `d`; `d` is set by the target mean degree.
  - *Inference:* raising the target from 8 to 10 or 12 under MD16 enlarges `d`, enlarges `w`,
    shrinks the core, and lowers `N*`. The same happens in the mean-degree, edge-to-edge, and
    k-nearest-neighbour sensitivity runs. The methodology handles one case, a change in the
    multiple, and not these.
  - Decision 8 says the core count "sets the target for suburb matching". If that count moves
    between runs, the suburb is matched within ±10% for one run and not for others, and the
    sensitivity runs change the node set as well as the factor they are meant to vary.
- **EVIDENCE NEEDED:** none.
- **RECOMMENDED ACTION:** fix `w` in metres once per site, from the primary `d`, and keep the
  same core node set in every run at that site. State that `N*` is the count of that fixed core.
  Report the buffer width in units of each run's `d`.

### Issue 5. The mean-degree ladder contains a value the lattice cannot take

- **LOCATION / TARGET:** MD15, MD16, 9.4 segment 1, 12.4 first row.
- **SEVERITY:** major (internal inconsistency; cheap to fix).
- **WHY IT MATTERS:**
  - *Evidence from v8 (O2):* a square lattice under a distance rule admits only certain degrees:
    4, 8, 12.
  - *Evidence from the methodology:* the ladder is 8, 10, 12, applied to "every graph in the
    study", and `d` is calibrated to within 0.05 of the target.
  - *Inference:* at a target of 10 the undisplaced lattice has no valid `d`. The lightly
    displaced lattices would reach 10 only by catching part of a ring of neighbours, which gives
    an arbitrary and uneven degree pattern. The regular end of the gradient would be undefined
    or distorted at that step.
- **EVIDENCE NEEDED:** none.
- **RECOMMENDED ACTION:** make the ladder 8, 12. Use 12 as the single mean-degree sensitivity
  value.

### Issue 6. The calibration and the tile bootstrap are not feasible as specified

- **LOCATION / TARGET:** 12.1 ("50 independent realisations", "200 for scheme T"); 12.2; U11.
- **SEVERITY:** major (feasibility).
- **WHY IT MATTERS:**
  - *Reviewer arithmetic:* calibrating scheme T needs 3 patterns × 50 realisations × 200
    resamples, each with an observed graph and a 20-member rewired ensemble. That is about
    630,000 threshold estimates, each of 100 edge permutations.
  - *Background, unverified:* in plain Python one such estimate on a graph of a few thousand
    nodes takes on the order of seconds. The total would be weeks of computation. The real-site
    run for scheme T, about 8,400 estimates, is hours and is manageable.
  - *Further gap:* 12.2 requires Monte Carlo error below one tenth of the interval half-width.
    Under scheme T each resample draws a fresh ensemble, so Monte Carlo noise is added to every
    bootstrap value and widens the interval. Under scheme W it is not said whether the same
    permutations are reused across resamples.
- **EVIDENCE NEEDED:** timing of one threshold estimate at a placeholder size. This needs no
  data and can be done with step 1.
- **RECOMMENDED ACTION:**
  1. Time the estimator first and write a computation budget into the protocol.
  2. Cut the calibration to what the budget allows and state the cut. For example, 20
     realisations, 100 resamples, and a 5-member ensemble inside the calibration only.
  3. State that edge permutations and the rewired ensemble are held fixed across scheme W
     resamples, so the interval holds no Monte Carlo noise.
  4. If the estimator is slow, say which compiled routine replaces the Python loop, and add it
     to the pinned packages.

### Issue 7. Two conditions in the decision logic have no operational definition

- **LOCATION / TARGET:** 14.3 ("similar gradient positions"); 14.4 ("`Q` does not vary with
  gradient position").
- **SEVERITY:** major for H1-G and for the "refuted" row.
- **WHY IT MATTERS:**
  - *Evidence:* v8 section 7 makes refutation conditional on the sites sitting at similar
    positions on the gradient. The methodology repeats the phrase and gives no measure.
  - *Evidence:* H1-G in v8 is that `Q` changes monotonically with the gradient metric. The
    methodology gives no test of monotonicity and no criterion for failure.
  - *Inference:* both would be judged after the results are seen, which is what the freeze is
    meant to prevent.
- **EVIDENCE NEEDED:** none.
- **RECOMMENDED ACTION:** fix both before the freeze. For example: positions are "similar" if
  the two sites' nearest-neighbour coefficients of variation differ by less than the spacing
  between adjacent gradient steps; H1-G holds if the rank correlation between step position and
  mean `log Q` has the same sign in each segment and overall. Any such definition is a
  convention and should be labelled so.

### Issue 8. The estimator is validated on a graph unlike any it will be used on

- **LOCATION / TARGET:** 13.1 first row; MD15; 9.3.
- **SEVERITY:** major for interpretation; not blocking.
- **WHY IT MATTERS:**
  - *Evidence:* validation is on a four-neighbour lattice, which has no triangles. Every analysed
    graph has mean degree 8 or more and, being a distance-band graph, is highly clustered
    (Barthélemy pp. 43–44, as recorded in the notes).
  - *Inference:* the second-largest-cluster curve is sharp on the lattice. On clustered and
    patchy graphs it may be broad or have several peaks, and the 1%-of-edges smoothing window
    was never tested there. The methodology says the lattice is "the easy case" and adds no
    harder one.
  - No supplied paper gives a known threshold for a harder graph, so a second known-answer test
    is not available from the evidence.
- **EVIDENCE NEEDED:** stability of the estimate on the synthetic patterns: its spread across
  repeated runs on one fixed clustered pattern, and its sensitivity to the smoothing window.
- **RECOMMENDED ACTION:** add a repeatability check to the pilot on one fixed pattern per
  segment, varying the seed and the smoothing window (0.5%, 1%, 2% of edges). Report the spread
  as a fraction of log 1.25. This is internal consistency, not validation against a known
  answer, and should be described that way.

### Issue 9. Size robustness is checked for `Q` on one pattern, not for a contrast between patterns

- **LOCATION / TARGET:** 12.3, residual size effect (M3).
- **SEVERITY:** major for interpretation of `D`.
- **WHY IT MATTERS:**
  - *Evidence from v8 section 6:* the observed and rewired graphs may respond to size
    differently, and "`Q` is a normalisation, not a proven one".
  - *Inference:* the M3 check varies size by ±10% on one pattern. It would detect a steep size
    effect. It would not detect a size effect that differs between an even pattern and a
    clustered one, and that difference is what enters `D` when two unlike sites are compared at
    one `N*`.
- **EVIDENCE NEEDED:** the difference in `log Q` between two synthetic patterns, one lattice-like
  and one clustered, at half `N*`, `N*`, and twice `N*`.
- **RECOMMENDED ACTION:** add that check to the pilot. If the difference between patterns moves
  by a large fraction of log 1.25 across sizes, say in the write-up that `D` is specific to the
  node count used. It needs no real data. It is an addition, so it is the human's call.

### Issue 10. Block layout is defined for the window, but only core nodes carry weight

- **LOCATION / TARGET:** MD25; scheme W.
- **SEVERITY:** minor to major, depending on the core share.
- **WHY IT MATTERS:**
  - *Evidence:* blocks are a 5 × 5 grid over the study region. Under scheme W only core nodes
    are weighted.
  - *Inference:* the 16 outer blocks of the grid lie mostly in the buffer. They hold few core
    nodes and will be merged under the 1% rule, leaving far fewer than 25 effective blocks.
  - *Reviewer arithmetic:* at mean degree 8 a window of `n` nodes is about 0.63 × √`n` wide in
    units of `d`. For 3,000 nodes that is about 34`d`; with a 4`d` buffer the core is about
    26`d` wide and holds under 60% of the nodes.
- **EVIDENCE NEEDED:** none.
- **RECOMMENDED ACTION:** for scheme W, lay the grid over the core. For scheme T, lay it over
  the whole window. State the minimum number of core nodes per block.

### Issue 11. One window of Kibera, placed by eye

- **LOCATION / TARGET:** 5.1 (MD1); section 19, item 9; U1.
- **SEVERITY:** major for what the result may be said to describe; not a defect of the
  methodology, which follows decision 8.
- **WHY IT MATTERS:**
  - *Inference:* "largest square inside contiguous built settlement" has more than one answer,
    and the choice sets `N*`, the suburb's size, and every synthetic pattern.
  - The block bootstrap describes variation inside the window. Nothing describes variation
    between possible windows.
  - The placement is recorded before any footprint is counted, which rules out tuning toward a
    result. It does not make the window representative.
- **EVIDENCE NEEDED:** none available inside the frozen scope.
- **RECOMMENDED ACTION:** in every downstream artifact, say "a window within Kibera", not
  "Kibera". Record a second candidate placement at the same time as the first, unused, so a
  later study can test it. The abstract's title and question name Kibera without this
  qualification and will need the same wording.

### Issue 12. Smaller points

- **SEVERITY:** minor (all).
- **Mixing criterion (9.2).** A 2% change in `t_c` between 10 and 20 swaps per edge may be
  smaller than its Monte Carlo error. State the criterion relative to that error.
- **Nearest-neighbour ties (9.6).** On an undisplaced lattice every node has four equidistant
  nearest neighbours. The bearing entropy then depends on the tie-break. Fix a rule, or compute
  the metric over all tied neighbours.
- **Edge effect in the metrics (9.6).** Say that nearest-neighbour searches for core nodes
  include buffer nodes. Otherwise distances near the core edge are overstated.
- **Multi-peak rule (MD19).** Say that it applies to rewired graphs too, and which combinations
  are run when both sites are multi-peak.
- **Fallback model (11).** One straight line across three segments assumes more than H1-G
  claims, which is monotonic change. A poor fit would then say little about arrangement. Say so
  beside the result.
- **Looking ahead (MD16).** The largest-component share is read on real graphs before the
  freeze. The methodology admits this. The values and the resulting choice should be committed
  in the same step as the measurement.
- **Imagery (5.1, 8.3).** The window is placed, and footprints are spot-checked, on the same
  imagery. Dense roofs that are hard to tell apart there are also the ones most likely to be
  mapped wrongly. The spot check is weakest where it is most needed.

## 5. Required checks

| Check | Answer |
|---|---|
| What would falsify the claim? | An interval for `D` wholly inside ±log 1.25, with the sites at similar gradient positions. Whether such an interval can occur at `N*` is not shown (issue 1), "similar" is undefined (issue 7), and the flags do not test a refutation properly (issue 2) |
| Is the spatial and temporal scale aligned? | Largely. Building-level nodes, local UTM, one snapshot per source. The two sites differ greatly in ground area and `d` by design, which decision 2 accepts. Kibera is now one window (issue 11). The core is not held fixed across runs (issue 4) |
| Could spatial autocorrelation or leakage inflate performance? | Not in the primary analysis, which makes no prediction. Autocorrelation of degree bears on the interval, and the block size is tied to its range, which is appropriate. The learning component shares no space between training and test, and says plainly that leakage control is described and not demonstrated |
| Are causal claims justified? | None is made between sites. Along the gradient a controlled-manipulation statement within the model is justified |
| Does the evidence discriminate among alternatives? | Partly. Source, node definition, and edge rule are varied. Arrangement is not separated from the clumping of dense patches, by ruling. Arrangement is not yet separated from a size effect that differs by pattern (issue 9) |
| Is a second dataset or independent benchmark available? | Yes for code correctness: three known values from the supplied papers. Yes at Kibera: three footprint sources. Doubtful at the suburb, where the two sources may share an origin. None for a clustered graph's threshold (issue 8) |
| Is the question answerable with the proposed data? | The gradient half: yes, with no real data. The cross-site half: only if the interval method works (issue 3) and the node count gives a usable precision (issue 1). Neither is known before the pilot |

## 6. What the methodology does well

Stated briefly, and with the caution in section 0 that the reviewer wrote it.

- Every agent choice is labelled and separated from human rulings.
- Citations match what the evidence notes record, and unsourced background is listed apart.
- The freeze before any real-graph outcome, and the rule on deviations, are concrete.
- The spatial-leakage requirement is not overstated.
- No novelty, causal, policy, or external-validity claim appears.

## 7. Unresolved issues

- **V1.** Whether "refuted" is reachable at `N*` (issue 1). Known only after the pilot.
- **V2.** Whether either bootstrap scheme can be calibrated (issue 3). Known only after the
  pilot. Whether a third, seam-free candidate is allowed is for the human.
- **V3.** Whether the size check between patterns (issue 9) is added. For the human.
- **V4.** The refinement of the approved flags (issue 2). For the human to confirm.
- **V5.** Computation time is unmeasured (issue 6).
- **V6.** This review is not independent (section 0).
- **V7.** I did not open the evidence PDFs. Citations were checked against the notes only.
- **V8.** My own timing and geometry figures are rough and unverified.

## 8. Suggested next scientific action

1. **Revise the methodology** for the gaps that need no ruling: fixed core (issue 4), ladder of
   8 and 12 (issue 5), block layout (issue 10), definitions for H1-G and "similar positions"
   (issue 7), the bias term and fixed random draws in the bootstrap (issues 3 and 6), and the
   smaller points in issue 12. Preserve the current draft in `outputs/history/`.
2. **Human view on three points:** a third interval candidate (issue 3), the between-pattern
   size check (issue 9), and the per-row flags (issue 2).
3. **Run step 1, the lattice validation, and time it.** It needs no data, it gates everything,
   and its timing settles issue 6.
4. **Add the reachable-outcomes table to the pilot outputs** (issue 1).
5. **Independent rerun of this review** in a fresh conversation from packet 05, before the
   methodology is treated as reviewed.
6. Packet 06 can then be regenerated. `src/orchestrator.py` already reads this file's name.

## 9. Verdict

**REVISE.**

The methodology is faithful to the frozen question and to decisions 1 to 8, and it separates
rulings, agent defaults, evidence, and unsourced background more carefully than is usual. Its
weakness is at the point where the result is decided. The interval that the decision table reads
is not yet a working method: one candidate scheme probably gives an interval that is too narrow,
the other probably gives one in the wrong place, the calibration would not catch the second
fault, and the calibration cannot be computed as written (issues 3 and 6). Nor is it shown that
the "refuted" row can be reached at the node count a single window of Kibera will give, and the
qualifying flags guard a supporting result but not a refuting one (issues 1 and 2). These bear
directly on falsifiability. Two inconsistencies are simple to fix and would otherwise surface in
the pilot: the core changes with every change of `d`, and the mean-degree ladder includes a
value the lattice cannot take (issues 4 and 5). None of this needs real data, none reopens a
ruling except where marked for the human, and none blocks the lattice validation, which should
start now. This review was written by the methodology's own author and is not independent; an
independent rerun is recommended before the design is relied on.
