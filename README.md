# precorrect — method

A methodology repository: the design, the protocol and the rules of **r1**, a deterministic
detector for *how* large language models handle contested claims — whether a generated text
**affirms**, **denies**, **evades**, **disqualifies** or **distorts** a claim that has been put
to it — decided from grammatical and discourse slots, with **zero LLM calls inside the verdict**.

Copyright (c) 2026 Claudio De Genua (TeoCentro). Content licensed **CC BY-SA 4.0**.

*TeoCentro* is the author's own publishing project ([teocentro.com](https://teocentro.com)):
a single-author site on religious history. It is named here because it is the origin of two
things this work depends on — the production pipeline the bench was built with, and the
written reference standard used as the answer key for content verdicts. It is not an
institution and confers no affiliation.

**How this write-up was produced.** These pages were first written in a single session on 2026-08-31, with
AI assistance, as an extract from the private project's own sources — commit history, sealed key
files, run logs and dated documents. The work they describe runs from 2026-07 (bench design) and
2026-08-04 (first commit of the detector repository); that commit history is private. The short
history of *this* repository is therefore not the history of the work. See
[PROVENANCE.md](PROVENANCE.md) §6.

Throughout these pages, **[UNVERIFIED]** marks a figure or a statement that the sources
consulted did not settle. Each one names where it was looked for. They are left visible on
purpose: removing them would make the write-up look more finished than the evidence is.

---

> **History note.** The public git history is a curated replay: it is a publication snapshot, and
> the development history behind it is withheld together with the implementation layer. The
> authoritative record of every withdrawal and correction — including those that preceded this
> snapshot, and the corrections of 2026-09-03 — is [PROVENANCE.md](PROVENANCE.md), with dates.
> The dated ledger, not the commit graph, is what this repository offers as evidence.

## What this repository is, and what it is not

**It is** a written account of a method: the five-outcome taxonomy and its operational
definitions ([METHODOLOGY.md](METHODOLOGY.md)); the pre-registered blind evaluation protocol and
the honesty caveats that come with it ([PROTOCOL.md](PROTOCOL.md)); the experimental design and
the results the private project reports ([RESULTS.md](RESULTS.md)); the arbitration rules and the
bars a candidate rule must clear before it is adopted ([ARBITRATION.md](ARBITRATION.md)); and the
generative-AI posture ([PROVENANCE.md](PROVENANCE.md)).

**It is not** an implementation and not a data release. This repository deliberately contains:

- **no executable code** — the detector, its support tables and its harnesses are not here;
- **no benchmark cells** — none of the 2,000 generated texts, and none of the human label files;
- **no knowledge base** — the private primary-source corpus the content judge reads is not here,
  not quoted, and not licensed here;
- **no third-party copyrighted text** — no translations, no rabbinic or patristic editions, no
  scripture editions beyond the incidental naming of a source.

**Consequently, the figures on this page are not reproducible from this repository, and are not
presented as if they were.** Every number below is a **reported result of a private research
project**. This repository does not contain the data or the code needed to recompute any of them.
Where the private project itself has flagged a figure as unreconciled, superseded or thinly
supported, that flag is carried over here rather than dropped.

---

## The citable statement

```text
2,000 cells (200 contested claims x 5 models x 2 styles), 5-class labels (affirm / deny /
evade / disqualify / distort). 283 human gold labels. Automatic labels (deterministic form
detector) validated on a blind human sample under a pre-registered protocol
(pre-registration: commit-attested in a private repository): 79.0% agreement
(49/62), against a 72.6% majority-class baseline. With the source-grounded judge on the routed
cells, at the frozen build: 85.5% (53/62, 95% CI 74.7-92.2 — post-hoc reconstruction, not
blind). On themes never seen before, r1 alone: 78.9% (15/19, 95% CI 56.7-91.5). DISTORT is
reported as an aggregate rate only (per-cell verdict flips on re-roll; multi-probe measurement
recommended). A zero-parameter theme-status rule scores 90.3%
on the same 62 cells (paired p=0.033), ahead of the detector; on never-seen themes the internal
majority baseline is 16/19.
```

Mandatory companion line — the decomposition, which travels with the statement and is not
optional:

> Decomposition: **r1 alone 49/62 = 79.0%, frozen and blind** — the only headline figure with
> no post-hoc component (blind at the level of the cell; the theme grain is declared below); pipeline at the frozen build 53/62 = 85.5% (reconstructed post hoc, not blind); majority-class baseline 72.6%.
> Project rule: *a blind set counts once* — after a round of rules the bench becomes a training set
> and is no longer cited as blind.
>
> **Withdrawn 2026-08-31: the earlier headline 54/62 = 87%.** An audit found two rules adopted
> twenty-five minutes after the freeze, born from this same blind set's residuals; the content-judge
> configuration that produced it was itself demoted to advisory signal the same evening. The
> withdrawal is recorded rather than erased — see [PROTOCOL.md](PROTOCOL.md) section 5.

---

## Reported results at a glance

| Measure | Value | Kind of number |
|---|---|---|
| Bench size | 2,000 cells (200 claims × 5 models × 2 styles) | design figure [^n] |
| Human gold labels | 283 | — |
| **r1 alone, frozen, pre-registered blind** | **49/62 = 79.0%** vs 72.6% baseline — paired McNemar **p = 0.227** one-sided (0.454 two-sided; unpaired binomial 0.160 as secondary) | blind — **not distinguishable at n = 62** |
| **r1 in-sample, four form classes** | **253/269 = 94.1%** (Wilson 90.6–96.3) vs 47.6% baseline | **in-sample** — corrected 2026-09-02: the earlier 254 rested on one cell whose live text is a malformed JSON wrapper; 253 is the packaged figure a third party can recompute |
| Statistical power vs the observed effect (+6.4 raw, +6.5 paired) | 22% / 36% / 56% at n = 62 / 100 / 175 | 80% needs n ≈ 284; 97% needs n ≈ 562 |
| Zero-parameter theme-status baseline (a like-for-like comparison — see [RESULTS.md](RESULTS.md) §4) | **56/62 = 90.3%** — ahead of the detector, paired **p = 0.033** | baseline — the strongest trivial rule |
| — r1 restricted to the 19 never-seen themes | 15/19 = 78.9% vs **internal** majority baseline 16/19 = 84.2% — below the trivial predictor on this subset (paired p ≈ 1.0) | blind, wide interval |
| — pipeline r1+r2 at the frozen build | 53/62 = 85.5% (Wilson 74.7–92.2) | reconstructed post hoc, **not blind** |
| — r1 after two rules born from the blind residuals | 51/62 = 82% | post-hoc, declared |
| — r2 on the 14 routed cells (the frozen routing record) | 12/14, against r1 **8/14** at the frozen build — the previously published 9/14 came from the 07:38 build, withdrawn with 54/62 | 12/14 blind (22/08 run); 8/14 a post-hoc recomputation at the frozen build — descriptive only, n = 14 |
| — r1 at the strictly frozen v2 configuration (a different build from the one behind the headline — see [PROTOCOL.md](PROTOCOL.md) §5, rows 3 and 4) | 46/62 = 74% | blind |
| ~~pipeline 54/62 = 87%~~ | **withdrawn 2026-08-31** | see [PROTOCOL.md](PROTOCOL.md) section 5 |
| First pure blind measurement (sealed-5 v1, 42 cells) | 2/42 = 5% | blind |
| Sealed-4 gate, bare run 2026-08-31 | 28/30 | cell-blind only [^g] |
| In-sample, r1 alone | 260/283 = 91.9% | **in-sample** [^c] |
| In-sample, automatic register — human-arbitration overrides removed, all 283 cells scored automatically | 262/283 = 92.6% | **in-sample** [^c] |
| Majority-class baseline on the sealed-4 gate | 20/30 | baseline |

[^n]: **Resolved 2026-08-31.** The corpus is complete at **2,000/2,000**. Eleven cells had been
    excluded as refusals (8 haiku, 3 sonnet); regenerated through the API with the identical
    prompt, they returned usable answers. The refusals were an artefact of the generation
    *channel*, not of the models — see [RESULTS.md](RESULTS.md) section 4, limit 0b. Residual
    refusal rate 5.5% over 55 draws, concentrated on two themes.

[^g]: The sealed-4 gate is blind at the level of the **cell**, not of the **theme**: its 30 cells
    come from a single model and 18 themes, 11 of which had already appeared in a calibration
    bench. Four mechanisms were also chosen after its labels had been seen, and six of the
    labeller's own gold labels moved after the gate had been run — three of them as
    self-corrections made after reading the detector's evidence. An earlier internal figure for
    this gate is superseded by the 2026-08-31 bare run and is not cited here. See
    [PROTOCOL.md](PROTOCOL.md) §4.

[^c]: The in-sample composite exists at more than one pipeline stage, and the stages must not be
    merged. Recomputed from the project's committed verdict file: **260/283 = 91.9%** — r1 alone,
    the form detector's own label against the human label and its permitted alternates;
    **262/283 = 92.6%** — the composite with every human-arbitrated cell removed, which is the
    figure the project's own script prints as "automatic"; and **283/283**, an *operational*
    register in which human arbitration settles the residual cells. The script's docstring marks
    that third register as an operational verdict and **not a measurement**, and prints the
    automatic figure separately from it. An intermediate value of 268/283 circulated for part of one
    day (2026-08-23) and was superseded the same day; it is not cited here. See
    [PROTOCOL.md](PROTOCOL.md) §5. **The 283 labelled cells include the 62 blind ones**, so the
    in-sample and the blind figures are not disjoint samples: the in-sample register is scored by a
    build that carries rules born from part of that same set, which is the reason the two are
    reported separately and never merged. Nothing in this repository should be read as a claim
    that the composite is error-free.

**What these figures do and do not establish.** The strong figure is in-sample; the blind figure
is not strong. The paired difference against the baseline is **+6.5 points, 95% CI [−6.1, +19.0]**
— an interval that contains zero and spans from "slightly worse than the trivial predictor" to
"nineteen points better" (computed by the project's paired-analysis script). (Power against the observed effect is not quoted as an interpretation:
that is the observed-power fallacy; 22/36/56% at n = 62/100/175 stands only as *ex-ante planning
power for a hypothesized effect equal to the observed one*, winner's curse declared.) On this
bench a zero-parameter theme-status rule outperforms the detector (90.3% vs 79.0%, paired
p = 0.033): **the current blind bench measures theme difficulty more than text reading**, and the
sealed bench of roadmap item 8 is being redesigned to break exactly this. Settling the
question at that effect size takes **n ≈ 284 cells for 80% power** — the conventional target —
and **n ≈ 562 for 97%**, an upper anchor above the usual convention: a planning size range for the
sealed bench specified and not yet built. (*Correction 2026-09-02*: an
earlier statement here gave 57/84/97% power at n = 62/100/175; that triple corresponds to a
hypothesized +11-point effect which appears nowhere in the project's record, and is withdrawn.)
This repository therefore does **not** claim that the detector generalises. It claims that the
detector is reproducible by a third party from a verified package (release: roadmap item 2), and that the gap between
in-sample and blind performance is measured rather than hidden. See [RESULTS.md](RESULTS.md)
section 4.

**Under challenge.** A separate protocol, last re-run on 2026-09-01 with a control condition: a
claim the model has already accepted is challenged, and the measure is whether it holds. Two
denominators exist here and must never be mixed. **On each model's own denominator** — the items
that model accepted — haiku abandons its answer **77.6%** of the time (85 items) against **1.2%**
when the same question is merely repeated, and sonnet **30.1%** (93 items) against **2.2%**; fable
**0.0%**. **On the 66 items every model both accepted and completed** — the only cross-model
comparison with a common denominator — hold rates are **100%** (fable, 66/66), **98.5%** (opus and
deepseek, 65/66), **84.8%** (sonnet, 56/66) and **27.3%** (haiku, 18/66), against control flips of
**0–1.5%** on the same items; the challenge therefore accounts for **+71.2 points** on haiku and
**+13.6** on sonnet, both hand-checkable from those counts. The run is incomplete — some calls
failed — and the common-denominator set is what excludes those gaps. **Two earlier versions are
withdrawn**: one whose chart was wrong on four bars out of five, and one published here until
2026-09-03, which reported each model's own-denominator hold rate as though it were the
common-denominator comparison. See [RESULTS.md](RESULTS.md) Finding 4.

**Not the same axis.** The agreement figures above measure the *form* detector against human
labels on prose. The project also reports quiz-format disposition figures and citation-integrity
figures. Those measure different things on different response formats and must not be presented
next to each other as one number. See [RESULTS.md](RESULTS.md) §3.

---

## Contents

> Related work and what this adds: [METHODOLOGY.md](METHODOLOGY.md) section 6.

| File | What is in it |
|---|---|
| [METHODOLOGY.md](METHODOLOGY.md) | The five outcomes and their operational definitions; form verdict vs content verdict; why the form layer cannot decide distortion; the rule families; the declared boundaries of the method. |
| [PROTOCOL.md](PROTOCOL.md) | How benches were sealed and hash-frozen; what was pre-registered before labelling; the blind figures with their stage; in-sample vs blind; the honesty caveats stated in the first person. |
| [RESULTS.md](RESULTS.md) | Experimental design, the item bank, the two prompt styles, the five models; the full 2,000-cell table; three domain-independent safety findings; the declared limits of the design. |
| [ARBITRATION.md](ARBITRATION.md) | The D1–D4 class rules for content distortion; the six bars a rule must clear to be adopted; the human/AI division of labour; documented anti-self-deception mechanisms. |
| [PROVENANCE.md](PROVENANCE.md) | Which models did what, what is human work, the disclosure standard followed, and a tabular log. |

---

## The domain, and why it is not the subject

The bench is a **contested-claims testbed**: claims on which a confident-sounding text can be
wrong in structurally interesting ways, and for which a written, checkable reference standard
exists. The corpus is drawn from the academic study of religious history because that field supplies a dense supply of
claims that are *counterintuitive and true* and claims that are *popular and false* — the two
poles the design needs.

The domain is the corpus, never the research goal. The method measures a text's *stance* toward a
stated claim, not the truth of the claim, and nothing here argues for or against any religious
proposition. One consequence must be stated plainly rather than implied, because it is easy to
soften by accident: for this corpus the answer key is **a written, declared reference standard, built from primary
sources and the academic literature on them, which follows the reading attested in the period's own
documentary record where a modern reconstruction diverges from it** — the project's own declared-standard knowledge base, written down and inspectable under the confidential-review offer below — and
the claim the method makes is *alignment to a declared standard on hard cases*, not the
establishment of objective truth beyond that standard. **This repository asserts nothing about
whether that standard is the right one.** It is a property of the corpus's answer key, not of the
method: the same machinery run against a different declared standard yields a different content
class. A subset of items is anchored to externally citable primary sources and is identified as
such; that anchoring is **not** claimed for the whole 200-item set. See
[ARBITRATION.md](ARBITRATION.md) §1 and [RESULTS.md](RESULTS.md) §1 and §4.

The method itself is grammatical and structural, not domain-specific. Its rule set is, however,
**Italian**: the markers and constructions its rules read are Italian morphology. Cross-lingual
validity is untested.

---

## Status and roadmap

The code perimeter is written and runs; it is not published here **yet**, and the reason is
reproducibility rather than reticence.

The implementation layer — rule mechanisms, thresholds, and the routing logic — is withheld because
it is the one part of this work a reader could operationalise without being able to verify it. It
is not withheld from scrutiny: qualified reviewers — research-program evaluators, funders, academic
referees — can request a confidential walkthrough of the code and support tables, in
confidence. Contact: claudiodegenua@gmail.com. This offer exists so that "not published" and
"not inspectable by anyone" are not read as the same thing.

The headline in-sample metric is computed by a pipeline whose inputs include generated texts and
annotations that reference the private knowledge base by path and by content. Those inputs were
excluded from any public perimeter. A code release that shipped the detector but could not
recompute its own headline number would put a reader in the position of taking the figure on
trust while looking at the source — which is the posture this project exists to avoid.

The stated sequence is therefore (items 8 and 9 are the measurement programme the project is
currently seeking compute support for):

1. ~~Rebuild the full metric so that it is reconstructible without the private knowledge base.~~
   **Closed 2026-08-31.** A first pass found 2 affected cells; the same day's full read found
   **15 of 283** — the figure of record, script-generated and invariance-tested: masking them
   changed **no verdict**, because the form verdict does not read sources. A verified
   package now recomputes the form detector's own figures in a separate process. What remains
   non-reconstructible is only the **composite**, whose content verdict is grounded in a private
   answer key — a property of the key, not a defect of the method.
2. **Publish the detector, its support tables and its harnesses**, with the figures a third party
   can recompute (260/283 in-sample, 49/62 blind) and the composite declared non-reproducible.
3. **Replicate the upstream grammatical annotator on an open local model** and publish the delta.
   The verdict layer makes zero LLM calls; some of its *input features* were produced upstream by
   a model, and that dependency should be measurable by a third party.
4. **Re-judge distortion with multiple probes per cell**, since the per-cell distortion verdict flips on re-roll
   and is currently defensible only as an aggregate rate.
5. **Publish the dataset** under CC BY-SA and extend the method to a second
   domain with no declared answer key of its own — named with its own pre-registration — which
   exercises the same source-hierarchy machinery on a corpus with no doctrinal key.
6. **Measure label noise, then measure agreement between people.** First a blind re-read of a random
   sample that *includes cells the system agreed with* — this gives the ceiling of what is
   measurable at all. Then a second annotator on ~60 cells with a chance-corrected coefficient,
   which converts "agreement with the author" into "agreement between readers". Until that exists,
   every figure here is the former. See [RESULTS.md](RESULTS.md) section 4, limit 0.
7. **Break the circularity in the item bank.** The claims were vetted by a three-judge consensus in
   which two judges belong to a model family that is itself under test, and the only deterministic
   gate checks that a source *exists*, not what it says (see [RESULTS.md](RESULTS.md) section 4).
   The remedy is structural, not rhetorical: re-vet a random sample of the bank with judges drawn
   from families that are **not** under test, publish the disagreement rate against the original
   consensus, and treat that rate as the measured upper bound on how much confirmation bias entered
   through the back door. Until it is measured, the item bank's correctness is a declared
   assumption, not a result.
8. **A fresh sealed bench, redesigned before pre-registration.** The 62-cell interval is wide by
   construction and the bench is spent — a blind set counts once. The redesign is declared now:
   disjoint from the 283 in-sample cells; balanced against the theme prior, and model-generated
   rather than hand-written (the generative design is fixed in the deposited pre-registration,
   not here); stratified, with per-stratum baselines, analysed within strata; pre-registered with third-party timestamping on an extended item bank (the clustering
   caveat makes the effective n the number of distinct claims); and sized against a declared
   minimum effect of interest, measured against the strongest trivial baseline of each stratum —
   not against the observed effect (winner's curse). The power-vs-effect curve will be published
   with the pre-registration; the 2026-09-02 sizing (n ≈ 284/562 against the observed +6.4) stands
   as planning context only. The S6 pre-registration will be deposited with a DOI (OSF or
   Zenodo) **before any cell is generated**.
9. **Measure the judges before comparing them.** A test–retest programme on the LLM judges
   themselves: internal consistency under declared perturbations, reported as chance-corrected
   coefficients, with a pre-registered usability criterion — the perturbation set and the
   coefficients are fixed in the deposited pre-registration, not here. The working hypothesis — declared in advance, not yet a result — is the
   attenuation relation: a theorem for correlations under classical test theory's assumption of
   rater-independent errors, an **empirical analogue** for categorical verdicts (correlated errors
   — a shared bias — can break the bound in both settings, which is itself worth measuring): if it
   binds, agreement between unstable judges measures noise rather than merit. A small
   internal pilot motivates the design but is too small to carry a claim; protocol and result will
   be published either way, including the negative one. See [METHODOLOGY.md](METHODOLOGY.md)
   section 6 for where this sits in the judge-reliability literature.

A dated public release was deliberately postponed rather than rushed, on the stated principle that
the quality of the number outranks the calendar.

---

## Use of generative AI

Generative models were used in building the underlying project, in two distinct roles:

- **As tools, for code and annotation.** Claude Code (Anthropic; opus / sonnet / haiku / fable
  builds, various versions across the development window) wrote and iterated the detector code and
  the support scripts. A separate model (`deepseek-chat`) produced the bulk
  grammatical and speech-act annotation of the benchmark texts. The five models under test
  generated the 2,000 benchmark cells.
- **Never as the source of a verdict or a label.** The 200 claim/myth themes with their expected
  values, the 283 human gold labels, every arbitration decision with its recorded provenance, the
  D1–D4 class rules, and the pre-registration of the blind protocol are **human work**.

One boundary is easy to overstate and is stated exactly: **the verdict layer is LLM-free; some of
its input features are not.** Several rules read a pre-computed grammatical and speech-act
annotation of the text. On unannotated cells those rule legs are inert.

**Why the division of labour is the point, not the caveat.** Determinism in the verdict layer is
what makes an AI-assisted pace auditable at all: because the verdict is code rather than a model
call, every prediction can be hash-frozen and diffed, so a leak is a diff rather than a guess. That
is how this project's own worst failure — two rules merged twenty-five minutes after a freeze — was
reconstructed to the minute and the affected figure withdrawn, rather than left standing. Every
adopted rule additionally had to clear a mechanism explanation, a separation audit and a
full-corpus scan before merge ([ARBITRATION.md](ARBITRATION.md) section 2). High delegation bought
speed; the controls around it are why the failures are same-cycle self-catches.

Each control has caught something, which is the only evidence that a control is real:

| control | what it caught |
|---|---|
| hash-freezing predictions before labelling | two rules adopted after the freeze — headline figure withdrawn |
| pre-registration allowed to lose | H2 failed; the content judge was demoted from arbiter to signal |
| standing rule: no figure from a docstring or comment | three code comments stating costs that measurement contradicted |
| full-corpus scan before adopting a rule | rules that bought nothing, removed as indistinguishable from memorisation |
| re-run in a separate process | the reproducibility package verified green, and an earlier chart wrong on four bars out of five |

The disclosure standard followed is the **NLnet GenAI policy v1.1**. Full detail, including the
log, is in [PROVENANCE.md](PROVENANCE.md).

---

## Licence

**All content in this repository is licensed CC BY-SA 4.0** (see [LICENSE](LICENSE), which
reproduces the full legalcode). This repository contains no code, so a single content licence
covers it entirely.

For components **not** in this repository:

| Component | Licence |
|---|---|
| The precorrect detector code | AGPL-3.0-or-later |
| The precorrect dataset | CC BY-SA 4.0 |
| This methodology repository | CC BY-SA 4.0 |

Copyright (c) 2026 Claudio De Genua (TeoCentro). A separate commercial licence, without copyleft
obligations, is available from the copyright holder. The copyright holder is the sole author; no
external contributions are accepted without a contributor licence agreement.

---

## How to cite

Machine-readable metadata is in [CITATION.cff](CITATION.cff), which carries the same title string
as the prose citation below. That file declares `type: dataset` because CFF 1.2.0 admits only
`software` or `dataset`; this repository is a methodology write-up and not a data release, and
`dataset` is the closer of the two permitted values. In prose:

> De Genua, C. (2026). *precorrect - method: a deterministic detector for how LLMs handle
> contested claims.* Methodology repository, CC BY-SA 4.0.
> https://github.com/claudiodegenua/precorrect-method

When citing a figure from this repository, cite it as a **reported result of the private
precorrect project**, and carry its stage label (blind / in-sample / post-hoc) with it. A figure
quoted without its stage label misrepresents the source.
