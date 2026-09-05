# RESULTS

> **These are reported results of a private research project. This repository does not contain
> the data or code needed to reproduce them.**

Experimental design, the aggregates of the 2,000-cell table (the full table is reserved — see
§2), four domain-independent findings relevant to safety evaluation, and the declared limits of
the design.

---

## 1. Experimental design

### 1.1 Framing

The testbed is a set of **contested claims with verifiable primary sources**. Each item carries a
source reference and a verbatim source quote, and is polarised in two directions:
**counterintuitive-but-true** (expected value: true) and **popular-but-false** (expected value:
false).

### 1.2 How the 200 claims were selected

The item bank is **model-generated and gated** — a "born-correct" pipeline: claims were generated,
deduplicated, and filtered by a multi-judge consensus plus a deterministic check that the cited
source exists.

Resulting bank:

| Set | Items | Expected value | Generation batches |
|---|---:|---|---|
| truths | 100 | all true | three batches: 29 · 43 · 28 |
| myths | 100 | all false | two batches: 48 · 52 |

**Post-hoc thematic labelling**, five categories:

| Theme | Truths | Myths |
|---|---:|---:|
| A — Christology / divine Name / identity of God | 23 | 11 |
| B — Eschatology / judgement / afterlife | 5 | 2 |
| C — Halakhah / practice / commandments | 30 | 17 |
| D — History and politics of the Second Temple / priesthood | 12 | 10 |
| E — Exegesis / Old-to-New-Testament narrative | 30 | 60 |

Generation proceeded in batches until all 100 were covered at N = 100.

### 1.3 The two prompt styles

Both prompts are in **Italian**; only the topic string changes between cells.

| Style | What it is |
|---|---|
| **mainstream** | A naive base prompt with **no system prompt and no constraints**: it asks for a documented overview of the topic, with context and citation of the relevant sources. |
| **professional** | A professional, rigour-instructed system prompt derived from a production system prompt with all grounding, retrieval and style components **stripped**, plus an appended mandatory-epistemic-methodology layer: cite the source, admit limits, grade each claim on a declared certainty scale, and do not treat training as a source. The experimental fact that matters is the instruction set with retrieval removed; nothing in the design turns on where the prompt came from. |

The contrast is therefore **naive open prompt vs. professional, rigour-instructed prompt with no
retrieval or grounding attached**. Measured side effect (n = 60 per arm): mean length **1,120.9
words** (mainstream) vs **1,432.4** (professional).

**Compute regime**: **one generation per cell, no repetitions**, for the content phase. The
earlier bare-quiz phase used three repetitions with a majority vote.

### 1.4 The five models

| Tag | Declared version |
|---|---|
| opus | `claude-opus-4-8` |
| sonnet | `claude-sonnet-4-6` |
| haiku | `claude-haiku-4-5` |
| fable | `claude-fable-5` |
| deepseek | **conflicting** — one document declares `deepseek-chat`, the generation script defaults to `deepseek-v4-pro`, and no model string was recorded in the result files. **[UNVERIFIED]** |

Design: 200 claims × 5 models × 2 prompt styles = **2,000 cells**.

---

## 2. The 2,000-cell table

Header of the run of record: `cells 2000 · discarded 11 · with content-judge coverage 2000`, with
discards distributed `haiku 8, sonnet 3`.

A **discard** is a junk cell: an environment artefact, or a refusal that contests the *format* of
the professional prompt (detected on the first three sentences). Cells that contest the *premise*
are **kept**, because contesting the premise is itself a disposition. `n` below is
post-exclusion. `DIST%` is the distortion rate over cells covered by the content judge, with
a Wilson 95% interval.

The full per-model, per-style, per-set table (twenty rows, each with a Wilson interval on the
distortion rate) is **reserved for the project's own publication**: it is the
quotable artefact and will appear with the write-up that presents it properly. The aggregates
needed to interpret every figure quoted here are in the README table. Content-judge coverage is
100% on every row of the reserved table.

### 2.1 This table is a composite, not pure detector output

This must be stated before the table is used for anything. The human label takes precedence where
one exists and the cell reached the content judge; otherwise the automatic verdict applies. Two
cells carrying a DISTORCE gold label never reached the content screener, so no ground was ever
recorded for them and the composite carries them under their form verdict — which is why the
composite counts 14 DISTORCE against the 16 that gold-label precedence would give. The exception
is stated rather than smoothed: it is the kind of gap a reader is entitled to find declared.

**"Zero LLM calls" holds for the form detector itself. It does not hold for the DISTORCE column of
this table.**

### 2.2 The 283 labelled cells

Two different distributions live over the same 283 cells, and they are given separately because they
are not the same object.

**The human gold labels.** A label admits a preferred verdict plus permitted alternates; thirteen
cells carry a dual label. The precedence rule the project applies when a dual label must collapse to
one class is *dual X/DISTORCE → DISTORCE*, so both readings are shown.

| Class | DISTORCE precedence | First-preferred label |
|---|---:|---:|
| AFFERMA | 126 | 130 |
| NEGA | 127 | 132 |
| DISTORCE | 16 | 6 |
| EVITA | 11 | 12 |
| SQUALIFICA | 3 | 3 |

**The composite verdict on the same cells** — this, and not the gold labels, is what supplies the
283 labelled cells to the reserved table of §2: AFFERMA 128 · NEGA 127 · DISTORCE 14 · EVITA 11 ·
SQUALIFICA 3.

Both readings support the same caveat, which is the reason the distribution is printed at all:
per-class claims about SQUALIFICA and DISTORCE are weak — three instances, and at most sixteen,
across the whole labelled set.

### 2.3 Companion axis — citation integrity

A separate extraction measured, for every rabbinic and authorial citation in the corpus, whether
it matches a real source. The counts are reserved for the project's own publication; the figure
that matters here — judge-estimated citation accuracy against deterministic source verification —
is Finding 1.

---

## 3. Four domain-independent findings

### Finding 1 — an LLM judge does not distinguish real citations from plausible invented ones

This is the strongest domain-independent result, and it is measured against a **deterministic**
gate — fetch plus existence check — not against another model.

- Single-model gate run (n = 20): real citation-ok at the deterministic gate **56% (58/104)**,
  against **96% estimated by the LLM judge**. By citation type: **biblical 100% · rabbinic 61% ·
  author 48% · vague 0%**.
- Multi-model gate run (n = 20): deterministic citation-ok **opus 47% (judge 80) · sonnet 50%
  (judge 81) · fable 56% (judge 86–96) · deepseek 57–67% (judge 65)**. Judge-estimated accuracy
  therefore spans **65–96%** across models against **47–67%** verified — the LLM judge
  overestimates by roughly **30 points** on three of the four models measured (deepseek's judge
  estimate is roughly aligned with its verified rate),
  and the strongest model is the worst on author attributions — 53 across 20 paragraphs, more than
  half invented. Confabulation is **localised in author attributions and vague claims, never in
  scripture verses**. Haiku was not measured in this run.
- **Quality is not sensitive to fabrication at all** (n = 18): paragraphs with high real citation
  accuracy (≥70%) get a quality score of **10.00**; paragraphs below 70% get **9.82** — a delta of
  **+0.18**.
- On the **grounded re-judgement snapshot** (587 truth cells + 612 myth cells, not the full 2,000),
  among the cells the doctrinal judge marks compliant, only **72%** of truth cells (257 of 357) and
  **60%** of myth cells (199 of 330) are also correct on the rhetorical-veil axis — 28% and 40% are
  veiled but pass. The denominators are the compliant subsets of that snapshot, not of the corpus.
- A snippet-based checker's "false claim" flags are wrong **71% of the time** (10 of 14 were false
  positives: text flagged as a false claim that was not one).

**Safety reading**: LLM-as-judge is systematically fooled by *plausibility*, which is exactly the
failure mode it is being asked to detect. Deterministic verification is not an optimisation here;
it is the only measurement that holds.

> **A splice to avoid.** A second, differently-denominated measurement exists and must not be
> merged with the range above: counting the *whole* apparatus (biblical + rabbinic + author
> citations together) over the corpus, per model and per output style. Its counts are reserved
> with those of §2.3. That measurement spans a lower range and therefore
> cannot be the breakdown of the 47–67% range — sonnet reads 50% in one and materially lower in the
> other. The
> two are different experiments with different denominators. Report each under its own
> denominator, or report only the first.

### Finding 2 — the verdict depends on prompt framing, and rigour instructions do not help

Measured on the full bench and on a separate true/false bench; the detail is reserved for the
project's own publication. The conclusions: instructing a model to be epistemically rigorous
**shifts the failure mode rather than removing it** — affirmation of truths falls for all five
models, denial of myths does not improve, and the displaced mass moves into distortion and
disqualification. On the bare bench the strongest model **mirrors the framing** of the question
itself: a softer framing does not help reasoning, it trades credulity on myths for recovery on
truths, roughly one for one. The one protocol-matched pair quoted in this repository and in the
application: acceptance of counterintuitive truths **10%** under a dry framing against **48%**
under a gentle one, single-call.

### The routing tail — a priority queue, not a detector

The detector does not decide distortion: most distorted cells carry no textual marker at all. A
small subset of its signals form a tail whose precision is
**1.45× the base prevalence** [1.15–1.80] — an **exploratory, post-selection** figure: the tail
unions three routing motives selected among fourteen tested, and only one survives a Holm
correction. Read it as a lead, not a measurement: it takes **13% of the cells** and concentrates **19% of
the distortions** in them. That is useful as a **priority queue for the content judge**, and it is
not a detector — a distinction that also rules out an earlier and more flattering recall figure,
which was never published here and should not be.
On the automatically labelled 1,882-cell set the lift falls to **1.09×** [0.94–1.25], i.e. to
chance. Reported at both denominators because they disagree.

⚠️ The split "the detector decides alone / the detector asks" holds **in-sample only** (98.8% at 61%
coverage). On the blind set it falls to 85.1–85.4%, and **7 of 12 errors fall outside** the routed
subset. The frozen build also reproduces **59 of 62** labels rather than all 62 — there was
uncommitted work between the last commit and the freeze — so the routing figures on the blind set
are **indicative, not blind**. (METHODOLOGY.md §3 quotes the in-sample routing figures; this
caveat governs them.)

### Finding 3 — closed-question accuracy does not transfer to open prose; refusal becomes evasion

The same model, on the same claims, answers a closed one-word quiz and writes an open overview
very differently — and the residual error changes *direction* with the format. The figures are
reserved for the project's own publication; the effect replicates on non-theological facts.

**Safety reading**: a binary accuracy metric on closed questions is blind to the dominant failure
of the same model in the generative regime, where the error converts into avoidance and softening
rather than into denial.

> **Not one axis.** These quiz figures and the agreement figures in [PROTOCOL.md](PROTOCOL.md)
> measure different things on different response formats. The project's own note on the seam:
> *"the 'denying' of the quiz becomes evasion in writing."* Presenting a quiz-recovery percentage
> next to a detector-agreement percentage, without saying they are different measurements, reads
> as inflation.

---

### Finding 4 — under challenge, models abandon a correct answer, and a control condition shows the challenge is what causes it

Last re-run on 2026-09-01 under a single protocol, with a **control condition** that no earlier run
had. A model is given a claim it has already accepted, then challenged; the measure is whether it
holds its answer.

**Two denominators exist and must not be mixed.** Each model is pressed only on the items it
accepted when asked bare, so those per-model denominators differ and their rates are *not*
comparable across models. The cross-model comparison is the common-denominator one: the items that
**every** model both accepted and completed.

| model | holds — common denominator (66 items) | flips on a bare repeat (same 66) |
|---|---|---|
| fable | **100%** (66/66) | 0.0% |
| opus | **98.5%** (65/66) | 1.5% |
| deepseek | **98.5%** (65/66) | 0.0% |
| sonnet | **84.8%** (56/66) | 1.5% |
| haiku | **27.3%** (18/66) | 1.5% |

**The control condition is what makes this attributable.** Repeating the same question *without*
any challenge flips **0–1.5%** of the time on those same items. The effect of the challenge is the
difference, not the raw rate: **haiku +71.2 points, sonnet +13.6, fable 0.0** — each recomputable
by hand from the counts in the table. On its own denominator, the same asymmetry is larger: haiku
abandons its answer **77.6%** of the time over the 85 items it accepted (control 1.2%), sonnet
**30.1%** over 93 (control 2.2%), fable **0.0%** over 100.

**The run is incomplete.** Some calls failed and were not all retried; the common-denominator set
is precisely what excludes the resulting gaps, which is why it — and not the per-model table — is
the figure of record.

**Two prior versions of this experiment are withdrawn.** The first mis-stated four bars out of five
— most consequentially, deepseek was reported as collapsing when in fact it holds; that figure came
from a moving model alias measured once. The second, published in this repository until 2026-09-03,
reported each model's own-denominator hold rate as though it were the common-denominator
comparison, and cited an intermediate run's item count: a splice of exactly the kind §3's own
warning box describes. The table above supersedes both.

## 4. Declared limits of the design

0. **One annotator, and the review effort was unevenly distributed.** All 283 gold labels were
   produced by one person, who is also the author of the rules and of the item bank. There is no
   inter-annotator agreement, no chance-corrected coefficient, and therefore **no human-human
   ceiling against which the reported agreement could be read**. That remains the principal
   validation this work lacks.

   The distribution of review effort is stated exactly, because an earlier and harsher version of
   this limit — "cells were re-read only where the system disagreed" — was **checked against the
   arbitration files on 2026-09-01 and found to be false**:

   | | cells |
   |---|---|
   | human intervention in the verdict | 27 / 283 |
   | passed through a review sheet | **106 / 283 (37.5%)** |
   | — of which the detector and the annotator already **agreed** | **79** |
   | inspected at the level of the captured span and its slot | **103** |
   | never reopened after the first pass | 177 |

   The blind set is the best-covered part: one review sheet covers **all 62 blind cells** with
   slot-level inspection. What remains is therefore not a filter that admits only agreeing errors,
   but a **decreasing asymmetry of scrutiny**: disagreements received the most attention, agreeing
   cells received a documented but lighter pass, and 177 cells were seen once. Any residual
   inflation of measured agreement comes from that gradient, and its size is unmeasured. The remedy
   is unchanged and cheap: a blind re-read of a random sample including agreeing cells.
0b. **Our own generation channel induced refusals, and we mistook them for model behaviour.**
   Eleven cells had been excluded from the corpus as refusals. Regenerated through the API with the
   identical prompt, the corpus returns to **2,000/2,000**; the residual refusal rate is **5.5% over
   55 draws**, concentrated on two themes. The cause was neither the prompt nor the model but the
   **channel**: generation through a command-line "clean room" led the model to address its
   environment, where the API did not. This is a limitation of our measurement instrument, and it
   is recorded here rather than in a footnote because an instrument that changes what it measures
   is the failure this project exists to detect.
1. **The 2,000-cell table is a composite metric, not pure deterministic output.** See §2.1.
2. **The form detector's blind gate**: 28/30, both misses being content distortions which by
   construction belong to the content judge. Four mechanisms were chosen after seeing that gate's
   labels; the tuning bench figure (119/127) is **co-calibration, not accuracy — explicitly "do
   not cite it as such"**.
3. **The blind result is not statistically distinguishable from its baseline, and the bench is too
   small to settle it.** Tested on 2026-09-01, re-tested 2026-09-02. The primary test against the
   baseline is the **paired exact McNemar** (the two classifiers are scored on the same cells):
   b = 10 / c = 6 on 16 discordant cells, **one-sided p = 0.227** (two-sided 0.454). The one-sample
   binomial p-values below are retained as secondary and are unpaired; the H1 direction follows
   from the hypothesis itself but the analysis plan was written after the measurement, so the
   two-sided values are also declared. Cells treated as independent; the clustering caveat of
   §4.16 applies, so the effective n is below the nominal one and every p-value in this table is,
   if anything, too small:

   | measure | k/n | % | Wilson 95% | baseline | p | reading |
   |---|---|---|---|---|---|---|
   | r1, frozen, blind | 49/62 | 79.0% | 67.4–87.3 | 72.6% | 0.160 † | not distinguishable |
   | pipeline, frozen build | 53/62 | 85.5% | 74.7–92.2 | 72.6% | 0.013 | significant, **not blind** |
   | never-seen themes only | 15/19 | 78.9% | 56.7–91.5 | 72.6% ‡ | 0.372 | not distinguishable |

   ‡ against the overall majority baseline. Within this subset the internal majority baseline is
   higher (16/19 = 84.2%) and the detector falls below it; both readings are published in the
   README table.

   † unpaired binomial, secondary. The statistic of record for this row is the **paired McNemar
   p = 0.227** one-sided (0.454 two-sided) reported in §4.3 and in the README.
   | r1 in-sample, four form classes | 253/269 | 94.1% | 90.6–96.3 | 47.6% | <0.001 | significant, **in-sample** [^i253] |
   | zero-parameter theme-status baseline | 56/62 | 90.3% | 80.5–95.5 | — | **0.033** paired, **in the baseline's favour** | ahead of r1 |

   Paired differences, published together: r1 − majority **+6.5** [−6.1, +19.0]; r1 − theme-status
   **−11.3** [−21.4, −1.2] — the second interval excludes zero. McNemar counts, reproducible by
   hand from this table: vs majority **b = 10 / c = 6**; vs theme-status **b = 2 / c = 9**.
   Never-seen complement: on the 43 non-virgin cells r1 scores 34/43 against an internal majority
   baseline of 29/43 — the margin over the majority baseline lives on the seen themes.

   [^i253]: Corrected 2026-09-02 from 254/269: the +1 rested on cell s4-058, whose live text is a
   malformed JSON wrapper that escaped the 2026-08-31 regeneration filter; 253/269 is the packaged
   figure, recomputable by a third party, and the only one consistent with 260/283 · 49/62 · 53/62
   at the same build.

   Stated without softening: **the only strong figure is not blind, and the only blind figure is not
   strong.** And on this bench a zero-parameter theme-status rule — reading the same theme-status
   input the detector receives, and nothing else — outperforms the detector (90.3% vs 79.0%, paired
   p = 0.033): the current blind bench measures theme difficulty more than text reading. The sealed
   bench of the roadmap is being redesigned to break exactly this; its design is fixed in the
   deposited pre-registration rather than here. This is also a question of statistical power. For a
   hypothesized effect equal to the one observed (+6.4 points), *ex-ante planning* power at n = 62
   is **22%**, 36% at n = 100 and 56% at n = 175, reaching **80% at n ≈ 284** — the conventional
   target — and **97% at n ≈ 562**, an anchor above the usual convention: a planning size range for
   the sealed bench specified and not yet built. Power against the effect actually observed is not
   quoted as an interpretation of the null result — that is the observed-power fallacy; the paired
   interval is the reading that counts. *Correction 2026-09-02*: an earlier version of this
   paragraph gave 57/84/97% at n = 62/100/175; that triple corresponds to a hypothesized +11-point
   effect which appears nowhere in the project's record, and is withdrawn. Nothing here should be read as a claim that the detector
   generalises; what is claimed is that it is reproducible by a third party, and that the
   in-sample / blind gap is measured rather than hidden.

4. **Blind pre-registered validation**: 62 cells. r1 alone, frozen: **49/62 = 79.0%** against a
   **72.6%** majority-class baseline — a margin of about six and a half points, which is the honest
   size of the effect on this bench. Pipeline at the frozen build 53/62 = 85.5% (CI 74.7–92.2). The
   earlier 54/62 = 87% is withdrawn: two of its rules were born from this set's own residuals. Not
   to be re-run or extended — a blind set counts once; a *new* sealed bench is the only way to
   narrow the interval.
4b. **The item bank is model-generated and largely model-vetted.** Only the source-existence gate is
   deterministic, and it checks **existence only**; truth, plain-fact status and plausibility all
   rest on a three-judge LLM consensus in which two of the three judges are the same model family
   being tested. Residual contamination is documented: at one revision the clean set stood at **92 items** (the
   source states the item count, not a contamination rate; the file holds 100 lines today); an audit found the question set still carrying wrong keys, consistent with a cited
   ~11% wrong-key rate; and the anti-circular cleaner is itself unreliable in the "false"
   direction (71% wrong flags).
5. **Generator/domain confound**: the theology items were produced by one model, the generic
   control set by another — so the domain comparison is confoundable with the generator.
6. **Corpus deduplication history.** On 2026-07-11 both sets were found inflated with paraphrase
   duplicates: truths 100 → **86 distinct**, myths 100 → **73 distinct**. Root cause: deduplication
   on an exact short-prefix match with no avoid-list. Files were subsequently returned to 100
   lines each, but a semantic deduplication pass for conceptual duplicates that diverge lexically
   is explicitly deferred. **[UNVERIFIED]** — no document states how the top-up back to 100 was
   validated for distinctness. **Treat "200 distinct claims" as unverified; the audited figure of
   record is 86 + 73 distinct.**
7. **Mixed register in the myths set**: 48 items are expert/niche false claims, 52 are folk-popular
   beliefs. The source states that a set of 100 distinct *folk* myths is probably not feasible —
   the folk pool is finite.
8. **Strong theme imbalance** (§1.2): eschatology has 5 truth items and 2 myth items; exegesis has
   30 and 60. Per-theme percentages therefore rest on very few distinct items in some cells. A
   top-up plan to n = 70 per theme is declared but was not complete.
9. **The standard of "correct" is the project's own declared key**, not objective truth beyond it —
   stated as non-negotiable in the specification. The judge must be blind to the arm and must not
   self-evaluate.
10. **Protocol and judge sensitivity**: the project states there is **no single true point
    estimate** for the quiz and content axes — quiz varies 10–47% with wording, set and n; content
    varies 53–98% with the judge. What is publishable is the law, the ranking and honest ranges,
    not fine figures. Only the study/reasoning axis is stable.
11. **Coverage is thin outside a few cells**: the coverage matrix declares that only about **7
    cells are solid** (N ≥ 50, three repetitions); the whole "generic content" line is empty.
12. **The content phase has no repetitions** — one generation per cell — unlike the bare-quiz phase.
13. **The 283-cell composite is not reproducible without the private knowledge base**: it depends
    on generated-text and annotation files that carry knowledge-base paths and content, and were
    excluded from any public perimeter. A knowledge-base-free recomputation is an open task with no
    released figure.
14. **The deepseek model version is not pinned in the data.** See §1.4. **[UNVERIFIED]**.
15. **The pre-declared exclusion rule for discarded cells is referenced but was not located** in
    the sources consulted. **[UNVERIFIED]**.
16. **A clustering caveat on the confidence intervals, added here rather than declared by the
    project**: the Wilson intervals in §2 are computed over *cells*, treating the 10 cells derived
    from one claim (5 models × 2 styles) as independent. Cells are clustered by item, so the
    effective sample size for per-theme statements is the number of distinct **claims**, not the
    number of cells.

### A note on adjacent controls

The project maintains a standing inventory of its own controls with statuses — solid /
directional / corrupt-and-needing-rerun / refuted / missing. It flags an LLM-judge citation
estimate as **optimistic by roughly 30 points and "do NOT use"**; marks three runs as corrupted by
judge failure and requiring re-run; and lists as its top-priority open question whether the
quality judge credits false citations. None of this contradicts the taxonomy work, but it bounds
what may be said around it: **the deterministic citation gate figures are marked solid; the
judge-derived figures are marked unusable, and only the former appear above.**
