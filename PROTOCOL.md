# PROTOCOL

The pre-registered blind evaluation protocol: how benches were sealed, what was fixed before any
label existed, which figures are blind and which are in-sample, and the caveats the project states
about its own numbers.

> Every figure in this file is a **reported result of a private research project**. This
> repository contains neither the sealed sets, nor the detector, nor the labels. Nothing here can
> be recomputed by a reader of this repository, and nothing here is presented as if it could be.
>
> The sha256 fingerprints below are the attestation of the freeze and are given in full, because a
> protocol claim that cannot be pinned to an artefact is worth less than one that can — but a reader
> cannot resolve them today; they become resolvable when the repository they index is published.
> Commit identifiers from the private repository are **not** reproduced here: they carry nothing for
> a reader who cannot resolve them, and the events they index are named instead. Where a quoted
> source names a switch of the unreleased tooling, the name is redacted and its function given.

---

## 1. The two kinds of number, and why they are never mixed

**An in-sample number** is measured on cells whose labels the rule author had already seen. Rules
were repaired against the residuals of those cells, so the number reports **fit**, not
generalisation. The project calls these benches "co-calibrated" and its own standing rule is that
they must never be called accuracy. The largest such figure is the 283-cell metric.

**A blind number** is measured on cells whose labels did not exist, or were not visible, when the
detector's predictions were computed. The mechanism is mechanical rather than declarative:

1. the detector is run over the sealed set;
2. its per-cell predictions are written into a key file, together with a **sha256 fingerprint of
   the whole prediction vector** and the **git commit of the detector build**;
3. only then is a labelling document handed to the human labeller — a document containing **no
   predictions, no weights and no highlighted sentences**;
4. a verification mode of the freezing tool recomputes the predictions and diffs them against the
   frozen vector, so a later reader can establish that nothing was retouched between freeze and
   scoring.

**Blindness has a grain, and the project states its grain.** In the sealed set the design
constraint was *at most one cell per theme within a stratum*, and every cell was excluded from the
already-labelled pool — but a theme could still have been touched during earlier calibration. The
key file therefore carries an explicit per-cell flag marking whether the theme is new, and the note written into the
key **before** labelling states which strata are blind and which are not. A cell can be new while
its theme is not; that weaker form of blindness is labelled as such rather than presented as the
stronger one.

**A blind set is spent by being used.** The project's standing rule, written before anyone asked
for it:

> *A blind set counts once: split by THEME, not by cell; after one round of rules the bench is a
> training set.*

This is why the headline evaluation appears as a **decomposition** rather than a single figure —
the frozen detector build, the build after post-hoc repairs, and the composite pipeline are
reported separately, and the post-hoc portion is labelled post-hoc.

---

## 2. How the benches were sealed

### sealed-4 — built 2026-08-12

- **30 cells**, drawn from regenerated clean texts produced through a single model's API.
- All rare-predicted classes, plus affirmation controls up to 30; order shuffled with a fixed seed.
- The texts were filtered for length, language and completeness.
- **Freeze**: predictions frozen *before* the labels, and re-frozen on the whole system (nuclei +
  grammatical annotation + speech acts). Two artefacts: a labelling document containing no
  predictions, and a sealed key carrying the per-cell prediction.
- **Sealing strength**: the sealed-4 key has **no sha256 field**. Its freeze is "predictions
  written to the key before labels", *not* hash-attested. The hash protocol begins with sealed-5.

### sealed-5 — built 2026-08-15, second freeze 2026-08-17

- **130 cells in strata**, at most one cell per theme within each stratum, all excluded from the
  157 already-labelled cells. Strata were ordered by expected information, so that stopping after
  the first still closes the biggest hole.

  | Stratum | Target class | Cells |
  |---|---|---:|
  | S1 | SQUALIFICA | 24 |
  | S2 | EVITA | 24 |
  | S3a | DISTORCE | 5 |
  | S3b | DISTORCE, missed cases | 13 |
  | S4 | model coverage | 32 |
  | S5 | mechanism coverage | 32 |
  | | **total** | **130** |

- **Integrity protocol, verbatim from the builder**:

  > *PROTOCOLLO DI INTEGRITÀ: le predizioni si congelano QUI, con l'impronta sha256, PRIMA che
  > l'utente veda le celle. Chi misura dopo deve poter dimostrare di non aver ritoccato nulla:
  > [l'interruttore di verifica] ricalcola e confronta.*
  >
  > (INTEGRITY PROTOCOL: the predictions are frozen HERE, with the sha256 fingerprint, BEFORE the
  > user sees the cells. Whoever measures afterwards must be able to demonstrate that nothing was
  > retouched: [the verification switch] recomputes and compares.)

- **Signature v1** — form only, no model annotations:
  `68040a0385bbfe919c1a8df326ba4f316cfa5db678518db54dceb063d2347607`
  with the note written into the key: *predictions frozen WITHOUT LLM annotations (pure form). If
  annotation is added later the configuration changes and must be frozen separately: this file
  remains the reference.*

- **Signature v2** — 2026-08-17, homogeneous configuration with v2 annotations:
  `4594978b3b5b7d958c282bfc9bba1c77ad72e2f610e671a23689df67a8a62d5e`, at the frozen v2 detector
  build. The freezing tool **refuses to overwrite** an existing v2 signature unless an explicit force
  flag is passed, and the verification mode recomputes and diffs.

- **Re-freeze history, stated rather than hidden.** Three re-freeze commits on 2026-08-17, one of
  which names a different fingerprint. The **operative** artefact is the signature `4594978b…` at the
  frozen v2 build, confirmed by the scoring commit, which scores the blind measurement explicitly
  against "frozen v2". Publishing the fingerprint without the re-freeze history would be the weaker
  choice. The commit identifiers that index these events sit in the private repository and will be
  published together with it.

- **Blind labelling waves** — a single human labeller, no predictions or weights or highlighted
  sentences in the labelling file:

  | Date | Tranche | Cells scored |
  |---|---|---|
  | 2026-08-16 | S1 (24) + S3a/S3b (18) | 42 |
  | 2026-08-18 | S2 tranche | 18 scored (+6 discarded); arbitrated 2026-08-19 |
  | 2026-08-22 | S4 (32) + S5 (32) | 62 scored (1 discarded, 1 without answer) |

  Together with the 157 cells already labelled before sealed-5 was built, the waves above
  account for 279 of the 283 cells in the metric. The remaining four: three in-sample cells
  labelled after the snapshot of the already-labelled pool, and the one "without answer" cell
  later settled in arbitration.

---

## 3. What was pre-registered, before any label was seen

**a) The standing rule** — that a blind set counts once, split by theme rather than by cell —
written before it was requested.

**b) The freeze itself**: predictions plus sha256 plus a reproducible verification command,
produced before the labelling document is handed over.

**c) The declared scope of blindness**, written into the sealed key *before* labelling: the 42
S1+S3 cells were blind when they were measured (2026-08-16 — row 1 of the table in §5) and were
**spent** from that day on; at this freeze they were already labelled and are therefore **not**
blind here. The blind number rests on the 88
S2/S4/S5 cells, and within those, on the virgin-theme subset. See the discrepancy in §6 on the
size of that subset.

**d) A full pre-registration document for the content judge, written before the run** and
committed 2026-08-22. Sample: 45 routed cells never seen by any version of the content judge.
Labels: the arbitrated ones of 22/08.

Three hypotheses were pre-registered before the run, each with a threshold fixed in advance and
**encoded directly in the scoring script**, not left to prose:

| Hypothesis | Statement |
|---|---|
| **H1**, directional | a minimum precision and a minimum observed-flip count on AFFERMA→NEGA overrides |
| **H2**, prior | v0.4 will not do worse than r1 on the same 45 |
| **H3**, interventionism | a ceiling on how often v0.4 is allowed to flip r1 |

**The adoption rule was decided in advance**:

- if **H1 holds** → the composite adopts the **directional-only A→N override**;
- if **H1 fails** → the content judge stays a **pure signal**, and a narrower role for it is taken
  up instead;
- if **H2 or H3 fail** → **document and do NOT re-tune on the same 45** — burn them, as an earlier
  set of 35 was burnt.

**e) An earlier directional hypothesis**, also pre-registered against a future tranche rather than
co-tuned on the current one.

**f) Standing adoption criteria for any new form rule.** Every fix must be motivated by a
**mechanism**, must pass a **separation audit** (it changes exactly the target cell on the
metric), and must pass a **scan over the full 2,000-cell corpus**. Adopt only if target-specific.
See [ARBITRATION.md](ARBITRATION.md) §2.

---

## 4. The honesty caveats — carried over in the first person

These are not caveats added for publication. They were written by the project, about itself, in
its own source files, and they are reproduced here because removing them would misrepresent the
record.

**1. Part of the sealed-4 gate is in-sample, and the project says so in the detector's own
docstring:**

> *warning: 4 mechanisms were chosen AFTER seeing the gate's labels, so the figure is partly
> in-sample. The clean number will come back with sealed-5.*

and, in the same block:

> *[a 127-cell co-calibrated bench]: … — this is CO-CALIBRATION, not accuracy. Do not cite it as such.*

**2. Why sealed-5 had to exist at all**, measured 2026-08-15:

- sealed-4 is 30 cells from **one single model**, on **18 themes, 11 of which had already appeared
  in a calibration bench** → *blindness is at the level of the CELL, not of the THEME*;
- SQUALIFICA had **4 labels in the whole project history**, all inside one tuning bench, **zero**
  in a sealed set; distortion recall was **1/6**;
- **only a small minority of the flags had a blind proof; most lived on the co-calibrated bench.**

**3. The labels themselves moved.** On sealed-4 **six** gold labels moved after the gate had been
run (cases 1, 2, 9, 10, 13, 15), with the revisions written into the key as comments. They did not
all move for the same reason, and the source distinguishes them:

- **three of them — cases 9, 10 and 13 — are self-corrections made after reading the detector's
  evidence.** The key records it in those words: *"the metre is arbitrable in both directions: here
  the user corrected HIMSELF on cases 9, 10 and 13, reading the detector's evidence"*, and at one of
  them *"you're right, it's NEGA (the detector was right)"*. The detector's own docstring likewise
  counts **three** accepted revisions;
- **case 2** was moved a week later by a separate arbitration round;
- **cases 1 and 15** were revised by re-reading the theme — case 15 explicitly because it is *the
  same theme as case 1*.

An external reviewer must be told that on that gate the ground truth was revised **after** reading
detector output. Blindness was therefore compromised in **both** directions: rules tuned on labels,
and labels revised on rule output. The detector's own docstring admits only the first direction; both
are stated here.

**4. The gate carried little information.** The majority-class baseline — always answer AFFERMA —
scores **20/30** on sealed-4. A 30-cell gate with that baseline is a weak instrument, and the
project's own note says so.

**5. Co-calibrated benches are named as such.** The project's internal summary carries the row
*"sealed-4 / bench / sealed-5 / S2 today — co-calibrated: never call them accuracy."*

**6. An earlier headline figure for the sealed-4 gate is superseded** by a bare run on 2026-08-31
(Python 3.11.8 and 3.13.3, identical output) giving **28/30**, with misses on **case 13 and case
28**. Both are labelled DISTORCE, and both are content distortions which by construction belong to
the content judge, not to the form detector. The superseded figure still stands, unfixed, in a
stale docstring header in the private repository; it is not cited here. A candidate mechanical
explanation exists — one adopted rule is annotated with a measured negative cost on this gate — but
**[UNVERIFIED]**: no flag-by-flag ablation was run to confirm it, and no reconciliation note
exists in any source consulted.

---

## 5. The blind numbers, by stage

Each row names the **build** it scores, because three different r1 figures on the same 62 cells
circulate and all three are real — they differ only by which detector build was scored.

| # | Measurement | Value | Stage |
|---|---|---|---|
| 1 | sealed-5 v1, frozen, **first pure blind** (S1+S3, 42 cells, labelled 2026-08-16) | **2/42 = 5%** — S1 1/24 · S3a 1/5 · S3b 0/13 | blind, hash-attested [^s5] |
| 2 | S2 blind tranche (labelled 2026-08-18, arbitrated 2026-08-19) | **10/18 = 56%** | blind |
| 3 | r1 on the S4/S5 blind 62, at the **strictly frozen v2 build** | **46/62 = 74%** (S4 24/32 + S5 22/30) | blind |
| 4 | r1 on the same 62, at the build of 19/08 [^frz] | **49/62 = 79%** (S4 25/32 + S5 24/30) | blind — the 19/08 repairs held on cells never seen (descriptive) |
| 5 | r1 on the same 62, after **two rules born from the blind residuals** | **51/62 = 82%** | **post-hoc**, declared as such in every source |
| 6 | Content judge v0.3.1, first blind, on the 14 routed cells of the frozen routing record | **12/14**, against r1 **8/14** at the frozen build (the previously published 9/14 came from the 07:38 build of 22/08, withdrawn together with 54/62; a recount from the live routing field finds 13 cells, not 14 — the frozen record defines the set) | 12/14 blind; the 8/14 is a post-hoc recomputation at the frozen build — descriptive, n = 14 |
| 7 | ~~Composite pipeline r1+r2~~ | ~~**54/62 = 87%**~~ | **WITHDRAWN 2026-08-31** — see "Numbers that circulate in two versions" below |

[^frz]: "Frozen" in the headline refers to this row: the 19/08 build's predictions were
    frozen into the sealed key at 07:13 on 22/08, before any S4/S5 label existed (§6.1).
    Row 3's "strictly frozen v2" is the earlier hash-attested signature of 17/08 — two
    freezes, both before labels; the headline 49/62 is scored against those sealed
    predictions, and the earlier signature stays reported.
| 7b | Composite pipeline at the **frozen** build | **53/62 = 85.5%**, Wilson **74.7–92.2** | reconstructed post hoc, **not blind** |
| 7c | Majority-class baseline on the same 62 | **72.6%** | baseline |
| 7d | Restricted to the 19 never-seen themes | **15/19 = 78.9%**, Wilson **56.7–91.5** | blind, wide |
| 8 | Wilson interval recomputed independently (z = 1.96, k = 54, n = 62) | **76.6 – 93.3** | interval of the **withdrawn** 54/62 — kept as record of that check, not as a current figure |
| 9 | Content/citation screen on the same 62 blind cells | 44 "no" · 7 "suspect" · **11 "yes"**; 20 citation errors routed to the D1 register; the single human-labelled distortion in the blind set was caught; 20 disagreements sent to arbitration | blind |
| 10 | Content judge v0.4, **pre-registered** run, 45 virgin routed cells | **r1 44/45 · r2 40/45**; 4 flips, **all wrong** | blind, pre-registered |
| 11 | Non-co-tuned gold set, 35 cells never seen | **r2 29/35 vs r1 34/35**, Wilson lower bound 0.673 ≪ 0.90 | blind |
| 12 | Directional A→N override, cumulative | **6/6** → override adopted | see §6.3 |
| 13 | sealed-4 gate, bare run 2026-08-31 | **28/30 = 93%**; misses at cases 13 and 28, both DISTORCE→AFFERMA | cell-blind only; partly in-sample (§4) |
| 14 | **In-sample** metric, r1 alone | **260/283 = 91.9%** | **in-sample** (was 263/283; three cells regenerated 29-30/08) |
| 15 | **In-sample** automatic register — human-arbitration overrides removed, all 283 cells scored automatically | **262/283 = 92.6%** | **in-sample** |
| 16 | **Operational** register, human arbitration settling the residual cells | **283/283** | **not a measurement** — the project's own script says so |

[^s5]: The **total** is the value of record twice over: it is what the project recorded on the day of
    the measurement, and it is what a recomputation of the frozen v1 prediction vector against the
    current labels gives today. The **per-stratum split** is the one that holds today; at the
    labelling date the same two hits sat in S1 and S3b rather than in S1 and S3a, arbitration having
    since moved cells between strata. See *Numbers that circulate in two versions* below.

### The pre-registration was allowed to lose

Row 10 is the load-bearing one for the protocol's credibility. Applying the rule written in
advance:

- **H2 FAILED** — the content judge did worse than the form detector on the same 45.
- **H3 HELD** — 4 flips of 45 ≈ 9%, within the 15% ceiling.
- **H1 NOT EVALUABLE** — exactly 1 A→N flip against a threshold of ≥ 3, and that one was wrong.

The pre-registered consequence was applied as written: the content judge was demoted from arbiter
to **pure signal**, and the 45 cells were retired from future measurement. The negative result is
part of the record.

### Numbers that circulate in two versions

- **Pipeline: 54/62 = 87% is WITHDRAWN (2026-08-31), and so is 59/63 = 93.7%.** The chain is
  recorded rather than erased. 59/63 was computed later on 22/08, *after* arbitration had revised
  five labels and admitted one previously unanswered cell — no longer blind, withdrawn in favour of
  54/62. An audit on 31/08 then found that 54/62 was not blind either: labels and predictions were
  frozen at 07:13 on 22/08 with r1 at 49/62; **two new rules were adopted at 07:38** — their own
  code comments name the blind cells they came from — and the pipeline was scored at 08:13 with
  that build. Rules born from the blind set's residuals, then re-measured on the same blind set.
  **Use 49/62 = 79.0% (frozen, blind) and 53/62 = 85.5% (frozen-build pipeline). Publish neither
  54/62 nor 59/63 as blind.**

**The full count of rules born from blind residuals is six, not two.** Beyond the two adopted at
07:38 on 22/08, four more were adopted between 17:39 on 22/08 and 14:46 on 23/08, across five blind
cells — one cell carries two rules of its own, two carry the first pair, two carry one each;
all commit-dated in the private source;
one further candidate from a blind cell was rejected and stays off. All six remain **on** in the
current code, and are declared rather than disabled, for one reason: the blind headline (49/62)
is computed from the **frozen prediction vector**, which no later flag can change — and with the
six rules off, the live code regenerates that frozen vector cell for cell, which makes the freeze
itself reproducible from the package. The live build's own score on the blind set is a *post-hoc*
figure wherever it appears.
- **r1 blind: 46/62 → 49/62 → 51/62.** All three are real; they score three different builds. The
  publishable decomposition is the one in the citable statement: *r1 alone 49/62 = 79.0%, frozen
  and blind* — always beside the 72.6% majority-class baseline; the 51/62 = 82% post-hoc variant
  is reported in the results table, not in the decomposition.
- **First pure blind: 2/42 vs 3/42.** Use **2/42**. It is what the project recorded on the day of
  the measurement — *"SIGILLATO-5: the blind number is 2/42 = 5%"* — and what the frozen v1 vector
  gives when scored against the current labels. A **3/42** reading is reproducible against one
  intermediate label revision of that same day, and survives as a header string in a generator
  script written afterwards; it is not published here.
- **In-sample composite: 268/283 is superseded.** It was recorded on 2026-08-23 and replaced within
  the same day — as labels and rules moved — by successively higher values, ending at the
  operational **283/283** with the automatic figure declared separately. **Use 262/283** for
  "automatic, no arbitration" and **260/283** for r1 alone (was 263/283 until three cells were
  regenerated on 29-30/08). Do not publish 268/283.
- **sealed-4 gate: 28/30.** The stale docstring figure is superseded and is not reproduced here.

---

## 6. Open flags a reviewer should know about

**6.1 — The virgin-theme count is not what two internal notes say.** The freezing tool's docstring
and the note inside the sealed key both state that the blind number rests on **46** virgin-theme
cells among the 88 S2/S4/S5 cells. Recomputing the flag directly from the sealed key gives **24**
(S2 5 · S4 14 · S5 5) — which matches the freeze commit's own log line. Two independent sources
therefore agree on **24**, which is the figure of record and the only one published here. The **46**
in the two prose notes is stale; searched and not reconciled — the key file, the freezing tool, the
four relevant commits and the two handover documents — and recorded for completeness, not because
anything rests on it.

**6.2 — The analysed corpus size varies by pass.** The design figure is 2,000 cells. Internal
passes variously report 2,000, 2,040 and 2,042, and the run log of record shows 2,000 cells with
11 discarded and several per-model cells at n = 94–98 rather than 100. **[UNVERIFIED]** — no
document reconciling design N against analysed N was located. Read the corpus size as "2,000 cells
by design; per-pass analysed N varies slightly with discards."

**6.3 — The A→N override was adopted on a cumulative count, not on the letter of the
pre-registration.** The pre-registered rule required ≥ 3 A→N flips at a pre-registered precision bar **on that
45-cell run**; the run produced 1 flip, and it was wrong, so H1 was recorded "not evaluable".
Adoption happened later, under a **cumulative-evidence criterion** — 6/6 directional flips across
runs — which generalises the single-run test rather than repeating it. Stated exactly: *adopted on a
cumulative count of 6/6 directional flips, after the pre-registered single-run test returned
insufficient flips to evaluate.*

**6.4 — The pre-declared exclusion rule for discarded cells is referenced but was not located.**
The run log names a dated document as the pre-declared rule for discarding junk cells. That
document was not found in the sources consulted. **[UNVERIFIED]**.

**6.5 — The two hardest classes carry almost no blind positives.** The 62-cell blind sample
contains a **single** human-labelled distortion. For those classes the project reports aggregate
rates rather than per-cell gold labels, and notes that per-cell distortion verdicts flip on
re-roll (multi-probe measurement recommended).

**6.6 — The blind sample is small.** 62 cells. The confidence interval is published alongside the
point estimate rather than instead of it, and it is wide: **67.4–87.3** (Wilson, 49/62). An earlier
version of this line printed 77–93 — the interval of the withdrawn 54/62 — corrected 2026-09-02.
