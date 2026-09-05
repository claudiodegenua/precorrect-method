# ARBITRATION

The class rules that constrain the one label the form detector cannot decide; the bars a candidate
rule must clear before it is adopted; the division of labour between the human author and the
generative models; and the mechanisms the project uses against its own self-deception.

> Figures in this file are **reported results of a private research project**. Nothing here can be
> recomputed from this repository.

---

## 1. The D1–D4 class rules

All four constrain a single label — **DISTORCE** (content distortion), the label the form detector
cannot decide and which it therefore *routes* to the content judge.

They are **class rules**: one human answer decides an entire family of flagged sentences at once,
rather than each case being adjudicated on its own. This is what makes the arbitration auditable —
a reader can check the rule, not just the outcome. The rules are four, were decided by the human
author rather than by a model, and are applied per class rather than case by case.

### D1 — citation error vs content error

Separates a wrong reference, a misattributed passage or an inverted technical mapping from a
distortion of the claim itself, so that the distortion class measures damage to the thesis rather
than bibliographic slips. Citation problems go to a separate register instead.

### D2 — taking a position vs epistemic caution

Separates a text that hedges or declines to commit from one that takes a substantive position, so
that ordinary academic caution is never read as denial.

### D3 — reported speech

Separates a heterodox reading the text merely *reports* from one it adopts as its own, so that
quoting or attributing a position is not confused with holding it.

### D4 — direct damage

The newest rule (2026-08-23), and the one that closed the largest arbitration batch. Turns
DISTORCE from an **error predicate into a harm predicate**: a false or imprecise detail counts only
if it directly damages the figure, fact or norm the claim is about — falsehood alone, elsewhere in
the text, is not enough. Applying it rejected a substantial share of the content judge's own
flagged cells as false positives, with the rejection written into the pipeline rather than silently
dropped.

### Scope

The four rules sit on top of a typology of eight distortion types — among them contradiction of
primary sources and generalisation unattested in the sources, the two the split below turns on —
and on top of a standing evidentiary rule: every "yes" must rest on a source, drawn from the
reference standard or from a primary source, that contradicts the sentence; where the reference
standard is silent the verdict is "suspect", never "yes" from memory.

**Silence never converts to a verdict.**

**What DISTORCE is defined against, stated plainly.** The operative rules name their standard, and it
should be read as they wrote it rather than as a neutral phrase: for this corpus the answer key is
the project's own **written reference standard**, built from primary sources and the academic
literature on them. Where a modern reconstruction diverges from the reading attested in the
period's own documentary record — including legal and juridical texts of the period — the key
follows the **attested** reading. "Standard" here is used in its documentary sense — the reading
the sources attest — not in a
denominational one. D4 makes the class a *harm* predicate
relative to that standard rather than an error predicate. That is a property of
**the corpus's answer key, not of the method**: the same machinery run against a different declared
standard produces a different content class, which is precisely why the roadmap's second domain is a
corpus with no doctrinal key. Nothing in this repository argues that the standard is the correct one;
it reports what the method does relative to a standard that is written down and inspectable.

**How the 14 DISTORCE decisions actually split across that typology.** The 14 are the
composite-verdict DISTORCE cells (16 under gold-label precedence; the two others never reached the
content screener, so no ground was ever recorded for them — see RESULTS §2.2 for the precedence
rule). Counted 2026-09-02 from the structured per-sentence type field, a mandatory part of the
screener's reply schema — recorded at decision time by construction, not reconstructed: **5**
cells were decided purely on the first type — a claim contradicting the knowledge base or a
primary source, checkable against the cited document (distinct from the citation-error register
of rule D1, which is a separate track); **3** on a decisive sentence carrying both a citation error and a hermeneutic type; **6**
purely on hermeneutic types, judged against the declared standard above. The split is published so
a reader can see how much of the content verdict rests on document-checkable ground (8 of 14 at
least in part) and how much on the declared key (9 of 14 at least in part — the two overlap on the
3 mixed cells). Two declared choices govern the reading: (1) the documentary/hermeneutic mapping —
type (a) alone counts as documentary; if type (e), a universal unattested in the sources and
checkable against a corpus, were counted as documentary too, the split would read 9/1/4; (2) the
count is taken on the decisive sentence named by the screener. This is the reading **least
favourable** to the documentary framing: on the 38 cells the screener itself affirmed, the split
is 16/11/11. Reproducibility note: the aggregate verdict registry carries a decisive-sentence
field that differs from the screener's own in 10 of 11 overlapping cases — a recount must use the
screener's own output files, not the aggregate.

---

## 2. When a rule is adopted, and when it is rejected

Every candidate rule is an environment flag with a declared default, so adoption is reversible and
ablatable. Six bars, all of them documented with the measurement that produced the decision.

### Bar 1 — separation on the labelled metric

How many of the 283 labelled cells change verdict. Recorded as a ratio in the adoption note — for
example *"separation 1/283"*.

### Bar 2 — target-specificity on the full 2,000-cell scan (the decisive bar)

The detector is re-run over **every cell of the corpus with no LLM annotations** ("pure form"), and
two scans are diffed. A rule is adopted only if its footprint outside the target is either **zero**
or a coherent same-family twin. Under this bar, adoption requires a scan over the entire corpus,
and only a small number of candidate rules cleared it, each with a minimal footprint; a family of
guards whose cost was invisible on the labelled benches but visible on the full scan was rejected,
with one narrow exception re-adopted after its own dedicated scan ([METHODOLOGY.md](METHODOLOGY.md)
§4).

The lesson from the rejections is written into the source rather than kept as folklore:

> *REJECTED after the counter-check on the 2,000-cell SCAN (the three metrics could not see the
> cost because the labelled cells have redundant N). Lesson: [the corpus-wide scan] is mandatory
> for every guard on this route.*

### Bar 3 — measured inertia kills a rule too

One rule is set to off with the note *"MEASURED 23/08: inert on its target case → OFF, risk of
memorisation with no gain."* A rule that buys nothing is removed on the grounds that it is
**indistinguishable from memorising the cell**.

### Bar 4 — a high flip-precision threshold, fixed in advance, for anything that *overrides* a verdict

Enforced in both directions: a candidate override with a strong absolute score but weak
flip-precision was refused and demoted to a human arbitration queue rather than auto-overriding —
the best absolute score lost to the precision bar — while a directional override that cleared the
pre-registered precision bar on a cumulative count was adopted and written into the pipeline with
its provenance attached.

### Bar 5 — pre-registration when the rule will touch blind cells

Hypotheses, thresholds and the adoption rule are fixed before the run and **encoded in the scoring
script**, not left to prose. On execution against a set of previously untouched cells, H2 failed,
H3 held and H1 was not evaluable — the content judge was demoted from arbiter to signal. **The
pre-registration was allowed to lose.** See [PROTOCOL.md](PROTOCOL.md) §3 and §5.

### Bar 6 — a signal never silently becomes a verdict

The content judge, measured at **29/35 against the form detector's 34/35** on a non-co-tuned gold
set (Wilson lower bound 0.673), was demoted to a signal and encoded as one: *"r2 no longer
overwrites r1 automatically: it stays recorded as a SIGNAL."* The same pattern is applied to
individual form rules moved *from verdict to signal*, and to a re-description rule that exists only
in signal mode.

The standing directive keeps all six in force:

> *the usual discipline: 2,000-cell scan per family, adoption only if target-specific,
> pre-registration when you touch blind benches, user arbitration on doubtful cases. No calendar
> pressure: the quality of the number outranks the date.*

---

## 3. Division of labour: human and model

### Human work

1. **The label sets.** All 283 labelled cells carry the human labeller's field. Sub-benches:
   sealed-4 (30 cells), sealed-5 (42 cells), the tuning bench, and the 62 blind cells labelled on
   2026-08-22.
2. **The blindness protocol applied to his own labels.** Labels were given without seeing any
   prediction: the labelling file contained no verdicts, no weights and no highlighted sentences,
   and the predictions were already frozen — hash-attested for the sealed-5 share, written before
   the labels existed for sealed-4 (see [PROTOCOL.md](PROTOCOL.md) §2). Labels admit a preferred
   verdict plus permitted alternates.
3. **Every arbitration.** 21 of the 283 cells are settled by explicit human decision with tracked
   provenance, tagged by *how the case reached him*:

   | Route | Cells |
   |---|---:|
   | content-judge flag → human arbitration | 14 |
   | second-detector dissent → human arbitration | 3 |
   | pilot contradiction → human arbitration | 2 |
   | pilot disqualification → human arbitration | 1 |
   | residual analysis → human arbitration | 1 |

4. **The D1–D4 class rules** (§1) — including the willingness to make them cost him: D4 turned a
   substantial share of the judge's own flagged cells into rejected false positives.
5. **The definitional rules the detector implements**, recorded in the source as quotations rather
   than as engineering choices: questions are excluded from the taxonomy; DISTORCE cannot exist
   without an affirmation; *words are memory, logic is generalisation* (the principle that bans
   word lists in favour of structural rules); and retraction is read asymmetrically — on a TRUTH it
   is SQUALIFICA, on a MYTH it is a refutation, i.e. NEGA.
6. **Self-correction against his own earlier labels** — with the correction written into the key
   rather than quietly applied, and recorded in the frozen answer registries: *"the metre is arbitrable in both directions: here the user
   corrected HIMSELF on cases 9, 10 and 13, reading the detector's evidence"*, and at one case
   *"you're right, it's NEGA (the detector was right)"*.
7. **Rejecting rules on principle, against the metric.** One inference rule was refused with a
   stated reason and no numeric justification: *"an EXPLICIT inference is the text taking a
   position, not evasion."* Another was kept on *"by the user's choice of principle (no word
   lists)"* even after its measured cost was corrected from "zero" to −1 on the bench.
8. **The scope and honesty boundaries**, written into the specification under a heading of
   non-negotiables: the standard of "correct" is a **declared standard**, not "objective truth
   beyond the standard"; and the judge must never self-evaluate.

### Model work

| Task | Model, as declared in the sources |
|---|---|
| Theme generation (truths and myths, with expected value, source reference and quote) | model-generated candidates, then a multi-judge consensus gate, a fail-closed skeptic pass, and a deterministic source-existence gate (models named in [PROVENANCE.md](PROVENANCE.md) §3) |
| The 2,000 benchmark cells under test | `claude-haiku-4-5`, `claude-sonnet-4-6`, `claude-opus-4-8`, `claude-fable-5`, `deepseek-chat` — 5 models × 2 prompt styles |
| Bulk linguistic annotation of the cell texts | `deepseek-chat` |
| Content-distortion pre-screening (isolated agents, knowledge-base retrieval, no labels visible) | sonnet — 62 blind cells |
| The source-grounded second detector | sonnet agents and the deepseek API, benchmarked head to head, with the cost of each run recorded |
| The code itself | written in Claude Code sessions, with an explicit scope fence separating one session's ownership from another's |

### The line between them

The line is **structural, not rhetorical**: models produce candidates, evidence and code; **they
never produce a verdict of record.** Three artefacts enforce it:

1. The detector makes **zero LLM calls inside the verdict** — stated on the first line of its
   source.
2. The content pre-screen is **forbidden to infer the label at all**: *"Do not deduce the cell's
   label at all. Its "yes" never changes anything on its own — it is recorded as a distortion
   candidate awaiting confirmation, and does not change the verdict.
3. Every non-detector verdict carries a **provenance string** naming its origin. Over the 283
   labelled cells: **form detector 256 · human arbitration 21 · directional override 6.**

---

## 4. Documented anti-self-deception mechanisms

Items **(f) 4**, **(g)** and **(h)** below are not measured on the detector's own benches. They come
from two standing documents of the wider project — a guardrail list and a multi-verifier audit — and
they govern the **content-verification pipeline around** the detector rather than r1 itself. They are
included because they are the mechanisms that catch the author's own errors; they are named as
project-declared and are separately sourced from everything else in this file.

**a) Naming the contaminated metric out loud.** The detector's own docstring refuses to let its
best number be read as accuracy: *"[a 127-cell co-calibrated bench]: … — this is CO-CALIBRATION, not accuracy. Do
not cite it as such"*, and *"4 mechanisms were chosen AFTER seeing the gate's labels, so the figure
is partly in-sample."*

**b) Marking the operational verdict as not a measurement.** The composite script's own docstring:
*"METHOD NOTE: this file is the pipeline's OPERATIONAL verdict, not a blind measurement — the cells
decided by user arbitration are marked with their source."* The script prints the composite and,
**separately**, the figure with all human-arbitrated cells removed. The number the project treats
as its honest accuracy figure is the blind, pre-registered one.

**c) Sealed benches with frozen predictions.** Predictions committed before the human saw
anything — hash-attested from sealed-5 onward; the blind gate reported separately from the tuned
one, always both.

**d) Reporting your own number going down.** An independent audit on 2026-08-31 found the
sealed-4 figure in the docstring stale and corrected it downward, identifying the two misses as
content distortions belonging to the content judge — under a standing instruction: *"flag in one
line if a number published elsewhere turns out to be superseded by your measurements … The true
numbers are the only asset we cannot afford to get wrong."*

**e) The 2,000-cell scan as a falsifier.** Its documented function is precisely to catch rules that
*look* free on the labelled benches because those benches carry redundant evidence (§2, Bar 2).

**f) Nine standing guardrails.** The load-bearing ones:

1. *Never trust a key or a citation asserted by a bare LLM* — the writing model shares the bias it
   is supposed to measure.
2. *Never a single verifier* — a same-model dual lens still errs materially on borderline cases.
3. **Model diversity must be calibrated, not maximal.** Alternative content-judge architectures
   were compared, and the one adopted is documented, with its measurements, as the best of those
   tried.
4. **Real source fetch is mandatory.** Alternative retrieval methods were compared, and the one
   adopted is documented, with its measurements, as the best of those tried.
5. *Measure ONLY on validated keys* — numbers on unvalidated keys are noise. Applying this rule
   dissolved an earlier "heavy bias" finding.

**g) A self-audit that publishes its own weakest layers.** Six pipeline layers scored 0–10 against
the project's own data, with the two weakest called out rather than buried: **source grounding
3–4/10, "weak link"**, and **human vetting 2–3/10, "absent"**. It also records an A/B in which the
"improved" version **lost**, and reports that as the finding.

**h) Refusing to call the residue irreducible.** A set of 84 uncertified questions was re-framed
as *"it is the LLM that hesitates, NOT an objective property"*, and the pilot bore it out: **7/10
resolved with real web sources, 5 of which flipped the original answer key** — the check found the
author's own keys wrong, not only the model's answers.

**i) A separate register for known-corrupt runs.** A standing inventory of controls with statuses
— solid / directional / corrupt-and-needing-rerun / refuted / missing — listing three results as
refuted and naming as the highest-priority missing control the one most damaging to the project's
own claims.

**j) Auditing the corpus for inflation.** A full dump found both theme sets padded with paraphrase
duplicates (100 → 86 truths, 100 → 73 myths), named the root cause, and downgraded the public
claim accordingly. See [RESULTS.md](RESULTS.md) §4.6 — the distinctness of the re-topped-up sets is
**[UNVERIFIED]**.

**k) Structural bans on self-serving inference.** Where the reference standard is silent the
verdict is "suspect", never "yes" from memory; the screener never judges whether a citation
exists, only the thesis; one rule is kept off because turning it on would violate a standing
user rule; and the project keeps a standing inventory of its own broken components, listing each
broken checker with the tests to redo because of it — including the ban on using a broken
checker's false verdicts to "correct" content.

---

## 5. What is missing from the record

The arbitration **dossiers** themselves — the transcripts in which individual cases were put and
answered — were never committed to the private repository. Only their generator scripts and the
**frozen answer registries** are under version control. The rules in §1 are drawn from those
registries. A public release that needed to quote an arbitration transcript would have to
regenerate it.
