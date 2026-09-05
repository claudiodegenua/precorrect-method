# METHODOLOGY

The five-outcome taxonomy, the split between the form verdict and the content verdict, the
families of rules that implement the form verdict, and the boundaries the method declares for
itself.

> The figures quoted in this file are **reported results of a private research project**. This
> repository contains neither the detector nor the benchmark cells, and no figure here can be
> recomputed from it.

---

## 1. What the detector is given, and what it produces

The detector receives three things:

1. a **claim** — the proposition under test;
2. a **grammatical annotation** of that claim;
3. a **generated text** written about the claim.

It segments the text into sentences and marks which sentences *engage* the claim's lexical core.
The verdict integrates multiple textual signals drawn from those sentences and their relationship
to one another — not a score threshold over a bag of words: it is not a classifier, and every
verdict traces to a named rule.

Class identifiers below are given in the original Italian, because they are the literal tokens the
detector emits; the English gloss follows.

---

## 2. The five outcomes, operationally

### AFFERMA — *affirms*

The text contains at least one sentence that fills the claim's slots with the claim's own content
and asserts it **in the text's own voice**, with no surviving denial or reduction directed at that
same slot.

The discriminating criterion is whether a sentence **anchors** the claim: names its subject and
carries its content in the text's own voice, as opposed to touching its vocabulary, quoting it,
questioning it, glossing it or implying it. Two negative criteria matter as much as the positive
one: **touching the claim's vocabulary is not affirming**, and **implying the claim by syllogism is
not affirming**. When no anchor is found, the fallback is explicitly not blind.

### NEGA — *denies*

The text contradicts the claim **in a slot the claim actually occupies**, whether by putting a
different filler in that slot or by directly negating the claim's predicate.

### EVITA — *evades*

The text engages the claim's lexical field but never takes a position on it — neither asserting nor
denying nor retracting, only touching the topic without closing it.

### SQUALIFICA — *disqualifies*

The text asserts the claim and **then takes it back** — affirm-then-retract — with the retraction
made in the text's own voice.

One asymmetry is declared and load-bearing. On a **myth** item, retracting is *dismantling*, so
affirm-then-retract collapses to NEGA. SQUALIFICA is reserved for **true-claim** items, where
retracting is withdrawal.

### DISTORCE — *distorts*

The text affirms the claim **and** adds or presupposes something that falsifies it: an
affirmation plus a heterodox addition. Without an affirmation the class cannot
exist. The typical shape is a **presupposed re-description** or a **role transfer**: the text says
something true while presupposing a false framing, which is precisely why its surface form is
indistinguishable from a plain AFFERMA.

**r1 does not emit this verdict.** It emits a *routing signal* — a distortion candidate — and the
decision is deferred to the content judge. See §3.

> **What is deliberately not published here.** The discriminating criteria above are stated as
> *classes of problem*, not as implementations: the individual guards, thresholds, weightings,
> lexical and morphological classes, and the order in which they are applied are not given. That
> layer is where most of the work went, it is directly applicable by a reader, and publishing it
> would hand over the part of the project that cannot be reconstructed from the results. The
> taxonomy, the protocol, the limits and the numbers are public; the implementation is not. See
> [README.md](README.md), "Status and roadmap", for how a qualified reviewer can request access to
> it.

### A sixth internal state, folded into EVITA

When no sentence engages the claim at all, the detector emits a distinct no-engagement state.
Every caller maps it to EVITA before scoring.

The public five-class taxonomy is therefore a **collapse of six structural states**, and EVITA is
a union of two different things: *never engaged the topic* and *engaged it but never asserted it*.
Per-class EVITA statistics mix the two, and should be read accordingly.

---

## 3. Form verdict vs content verdict

**The form verdict** answers: *what did this text do with this claim's grammatical slots?* It is
computable from the text and the claim alone. It needs no sources, no knowledge base and no notion
of what is true — only a declared set of grammatical and discourse properties of the claim, of the
sentence and of their annotation. Every one of those is a property of the string and of a declared grammatical
annotation of it, which is what makes **zero LLM calls inside the verdict** possible.

**The content verdict** answers: *is what the text put in that slot right?* That requires the
primary sources. AFFERMA-with-a-false-addition and AFFERMA-plain are **the same form**. The four
emitted form classes exhaust the ways a text can *position itself* toward a claim; they do not
distinguish a text that fills the slot correctly from one that fills it with a substitution.

### Why the form layer cannot decide distortion

Three reasons, all recorded in the source, and all structural rather than engineering gaps:

1. **Form underdetermines the class.** One recorded case has the *same* trigger sentence yielding
   DISTORCE or EVITA depending on what the rest of the text does with the claim's predicate. No
   property of the trigger sentence can decide it.
2. **Measured precision as a verdict is unusable.** Certain candidate signals fire too rarely and
   too inaccurately against the human distortion labels to stand alone as a verdict.
3. **The blind gate's residual errors are exactly this class.** The remaining misses are cells
   labelled DISTORCE that r1 returns as AFFERMA — the form is genuinely affirmative and the
   falsehood sits in the content.

### The hand-off

The architecture is a **two-stage split with an explicit hand-off**. r1 decides the form and,
*independently of the verdict*, emits **routing motives** — questions for the content judge.

r1 routes a cell to the content judge when the formal evidence is ambiguous or insufficient to
decide on its own; not every signal it notices is enough to route by itself.

Two constraints govern the split: r1 never decides on content, and only r2 confirms DISTORCE —
but it is r1 that notices the cell needs r2.

The blind decomposition is the measured evidence for that shape: r1 alone 49/62 = 79.0% (blind), r2 on
the routed subset 12/14 (blind — descriptive, n = 14), pipeline at the frozen build 53/62 = 85.5% (Wilson 95% CI
74.7–92.2 — reconstructed post hoc, not blind), against a 72.6% majority-class baseline. On the
blind set, however, the split holds only partially — **7 of 12 errors fall outside the routed
subset** (see [RESULTS.md](RESULTS.md), the routing caveat in section 3): the in-sample routing
figures do not transfer untouched.

---

## 4. The rule families

The detector declares well over a hundred flags, each with a declared default, so that any single
one can be ablated. Most of them are named mechanisms; the remainder are tuning parameters —
weights, windows, thresholds, modes. A minority are off by default. Counted directly from the
detector source rather than recalled. Flags whose default is **off** are **rejected** mechanisms,
kept in place together with the measurement that rejected them — the record of what *failed* is part
of the method, not an embarrassment to be cleaned up.

**Twenty-two families of rules** cover the problem, from segmentation and input hygiene through
negation scope, slot identity and discourse acts to the polarity of the claim itself. The list of
families is a decomposition of the problem and is not published: three are given as examples of
the granularity, and the rest are available under the confidential-review offer in the README.

| # | Family | The problem it addresses |
|---|---|---|
| 2 | Questions and non-assertions | A question asserts nothing and must not count as a position. |
| 5 | Slot identity | Whether a rival filler occupies the claim's slot or merely sits beside it. |
| 13 | Explicit verdicts | A text's own explicit ruling on the claim is evidence, not framing. |

Each family is a problem the detector had to solve; **how** each is solved is not published here, for
the reason given at the end of §2.

A design principle runs through several of those families and is stated by the author as a rule:
**"words are memory, logic is generalisation"** — and the cost of applying it is recorded whenever
it is not zero.

---

## 5. Declared boundaries — what this method does not measure

**Before the list: the reference itself is one person.** The labels this method is measured
against were produced by a single annotator, who also wrote the rules. No inter-annotator
agreement exists, so every agreement figure in this repository is agreement *with one reader*,
not agreement between readers, and there is no human-human ceiling to compare it to. See
[RESULTS.md](RESULTS.md) section 4, limit 0, for why the way those labels were revised biases
the figure upward rather than in a neutral direction.

- **Not truth.** r1 measures a text's *stance* toward a stated claim. Whether the claim is true is
  fixed in advance by the project's answer key. The specification states this without hedging: the
  standard of "right" is the project's own written key, and the claim is that the method aligns a
  model to a **declared standard** on hard cases — not that it establishes objective truth beyond
  that standard.

- **Not citation accuracy or confabulation.** Source fidelity is the content judge's job. The gap
  is concrete: in this project's own measurement, real citation accuracy in generated prose
  measures **47–67%**, against **65–96%** self-reported by the same models grading their own
  output (see [RESULTS.md](RESULTS.md) §3, Finding 1). That gap is the stated reason the form
  layer uses zero LLM calls.

- **Not intent.** No intent is attributed. The model does not "decide to lie"; the account offered
  is that its default truncates effort. Nothing in r1 measures intent, and no public framing of it
  should imply otherwise.

- **Not per-cell distortion.** DISTORCE is reported as an **aggregate rate only**. The per-cell
  verdict flips on re-roll, and multi-probe measurement is recommended. Any presentation of the class must
  carry this.

- **Not a language-general detector.** Every lexical and morphological class in the rule set draws
  on negation markers, adjectival classes, periphrastic constructions and connectives of Italian,
  treated as morphological classes rather than word lists. One English flag exists and is off by
  default; English cells appear in the development record as a known low-density blind case.
  Cross-lingual validity is **untested**.

- **Not independent of upstream annotation.** A substantial number of rules read a pre-computed
  grammatical and speech-act annotation of the text. The *verdict* makes zero LLM calls; the
  *annotation* was produced upstream by a model. On unannotated cells those rule legs are inert.
  Stated exactly: **the verdict layer is
  LLM-free; some of its input features are not.**

- **The tuning bench is not an accuracy measure.** The source says it directly: *[a 127-cell co-calibrated bench]:
  119/127 — this is CO-CALIBRATION, not accuracy. Do not cite it as such.* Several rules were both
  adopted and rejected against that same bench.

- **The blind gate is small, and its blindness is partly spent.** Thirty cells, with a
  majority-class baseline of 20/30 — a gate that size carries little information. Four mechanisms
  were chosen *after* seeing its labels, and six of the labeller's own gold labels moved after the
  gate had been run — three of them (cases 9, 10, 13) as self-corrections made after reading the
  detector's evidence, the other three from a later arbitration round and from re-reading the theme.
  Blindness was compromised in **both** directions. See [PROTOCOL.md](PROTOCOL.md) §4.

- **Thin support in two classes.** The tuning bench carries only **4 SQUALIFICA labels, all on
  true-claim items**, which is why a myth-side defect in that branch was invisible to it. The
  relevant distortion scan has **6** human-labelled instances. Per-class claims about SQUALIFICA
  and DISTORCE rest on very small support.

- **Partly reproducible as of 2026-08-31.** The *form* detector and its own figures (260/283
  in-sample, 49/62 blind) are reproducible by a third party: 15 cells of 283 required masking (a
  first pass had found 2, superseded the same day), and masking them changed no verdict — the form
  verdict does not read sources. The
  **composite** remains non-reproducible, because the content verdict is grounded in a private
  answer key; its inputs are excluded knowledge-base-citing
  files. They are reported as **published results of the private project**, not as anything this
  repository verifies.

- **Not a measure of whether an intervention helps.** That is a different design entirely: a
  bare-versus-intervention A/B scored by a save rate over repeated runs. It shares **no metric**
  with the taxonomy work and must not be conflated with it.

---

## 6. Where this sits in existing work

This repository is not proposing a new **documentation** standard. Documenting how a benchmark was
built, publishing adjudication decisions and recording annotation provenance are themselves
established practices, listed below. What is claimed as new is **the benchmark and the taxonomy** —
a five-outcome stance classification with a deterministic, zero-LLM-call form detector, on a corpus
of contested claims in Italian — evaluated here *against* those established transparency practices,
rather than presented as though transparency itself were the innovation.

- **Documentation of datasets.** Gebru et al., *Datasheets for Datasets* (2021), recommends that a
  benchmark ship with a record of its construction, including potential sources of noise and error.
  The declared-limits sections of this repository are written to that expectation.
- **Adjudication rather than vote aggregation.** The standard practice for annotation disagreement
  is to forward conflicting items to a further reviewer, yielding an *adjudicated* gold label rather
  than a raw majority. That is the procedure followed here, with one difference stated in the next
  paragraph.
- **Rationales as first-class data.** *ERASER: A Benchmark to Evaluate Rationalized NLP Models*
  (Deyoung et al., 2020, arXiv:1911.03429) established the practice of publishing the evidence a
  decision rests on, not only the decision.
- **Decision trails.** *PANORAMA* (arXiv:2510.24774) publishes the decision trail and rationale of
  each examination step, rather than the outcome alone.
- **Benchmark-construction transparency.** *Beyond the Numbers: Transparency in Relation Extraction
  Benchmark Creation and Leaderboards* (arXiv:2411.05224) documents how thinly benchmark
  construction is usually reported, and argues for the reporting practice adopted here.

**What is added.** Two things, both consequences of the verdict layer being rule-based rather than
a model:

1. **Adjudication at the level of the mechanism, not the outcome.** Each disputed cell was reviewed
   by asking not only "is the final label right?" but "*which span* did the detector capture, and
   *under which slot*?" — that is, whether the system reached a correct verdict for a correct
   reason. This is auditable here because the verdict is computed from declared grammatical and
   discourse slots; the same audit is not available for an LLM judge, whose stated reason is a
   further generation rather than the cause of its verdict. That asymmetry is the argument of this
   project, and the adjudication record is where it is demonstrated rather than asserted.
2. **A complete arbitration log rather than a sample.** The arbitration record covers the disputed
   cells of the metric with the human decision preserved per cell, including the cases where the
   human corrected the human — see [ARBITRATION.md](ARBITRATION.md). The coverage was counted
   against the arbitration files on 2026-09-01: 106 of 283 gold cells passed through an explicit
   review sheet (79 of them in agreement), 103 were inspected at slot level, and all 62 blind
   cells are covered — the full scrutiny profile is tabulated in [RESULTS.md](RESULTS.md).

Neither addition is a new methodology. Both are ordinary practices applied to a system whose
internals happen to be inspectable — which is the point being made about the systems whose internals
are not.

### The judge-reliability literature

Most empirical work on LLM-as-a-judge measures alignment with human preference (Zheng et al. 2023,
arXiv:2306.05685; Liu et al. 2023, arXiv:2303.16634; Kim et al. 2024, arXiv:2310.08491) or
systematic biases such as position and self-enhancement effects (Wang et al. 2024, arXiv:2305.17926;
Zheng et al. 2023). Work that treats a judge's agreement **with itself** as the primary object of
study is recent and sparse — with a single earlier precursor (Schroeder & Wood-Doughty 2024,
arXiv:2412.12509, test–retest reliability via McDonald's ω): repeated identical trials flip pairwise verdicts in over a tenth of
cases (Yagubyan 2026, arXiv:2606.13685); self-consistency varies by task and metric (Haldar &
Hockenmaier 2025, arXiv:2510.27106); and high test–retest reliability can coexist with severe
position bias, so internal consistency bounds validity without implying it (Norman et al. 2026,
arXiv:2606.19544). Adjacent work formalises when an LLM verdict may replace a human label
(Calderon et al. 2025, arXiv:2501.10970), lets judges abstain under low confidence rather than
forcing a verdict (Jung et al. 2025, arXiv:2407.18370), and anchors verdicts in checklist-style
evidence rather than free-form instruction (Hong et al. 2026, arXiv:2601.08654).

This project sits on the same side of that literature, with one displacement: the content verdict
is delegated to an LLM judge only where form underdetermines the outcome, and the planned judge
experiments treat judge reliability as an object of study in its own right, rather than as one
more reported metric; the design is fixed in the deposited pre-registration. The division of
labour itself — a deterministic verifier first, an LLM judge only where it does not decide — is
not unique to this project: the reliability of verifier layers is now a research question in its
own right (Seo et al. 2025, *Verifying the Verifiers*, arXiv:2506.13342), and what this project
adds to it is the sealed-bench measurement of the verifier.
