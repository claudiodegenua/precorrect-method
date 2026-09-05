# PROVENANCE

The generative-AI posture of the precorrect project: which models did what, what is human work,
which disclosure standard is followed, and a dated log.

> This file describes the **private research project**. This repository is a methodology write-up
> and contains none of the artefacts named below.

---

## 1. The posture, in one sentence

**AI as a tool for code and annotation; never as the source of a verdict or a label.**

The 200 claim/myth themes with their expected values, the 283 human gold labels, every arbitration
decision, the D1–D4 class rules and the pre-registration of the blind protocol are **human work**.

---

## 2. Disclosure standard

The project follows the posture of the **NLnet GenAI policy v1.1** as its standard for disclosure:
AI-assisted code and annotation are declared; the **evaluative judgments that constitute the
actual research claims are not AI-generated**.

Two further commitments follow from that standard and are stated here so a reviewer can check them
against the record:

1. **Where a model touched an evaluative step, the step is named and its output is marked as a
   candidate, not a verdict.** The content pre-screen is the clearest case: it is explicitly
   forbidden to infer the cell's label, and its output is recorded as *"distortion candidate —
   awaiting user confirmation, does NOT change the verdict"*.
2. **Where a figure depends on a model-produced input, that dependency is stated rather than
   folded away.** The load-bearing instance: *the verdict layer is LLM-free; some of its input
   features are not.*

---

## 3. Which model did what

| Role | Model, as declared in the sources | Verdict-bearing? |
|---|---|---|
| Candidate generation of the 200 claims (with expected value, source reference, source quote) | `claude-opus-4-8` | no — gated downstream |
| Three-judge consensus gate on candidates (true-or-false per source; plain fact; plausible citation) | opus, opus-critic, sonnet | no — plus a fail-closed skeptic pass and a deterministic source-existence check |
| The 2,000 benchmark cells under test | `claude-opus-4-8`, `claude-sonnet-4-6`, `claude-haiku-4-5`, `claude-fable-5`, `deepseek-chat` [^ds] | n/a — these are the subjects, not the judges |
| Bulk linguistic annotation of the cell texts | `deepseek-chat` | **input feature only** — see §4 |
| Content-distortion pre-screening (isolated agents, knowledge-base retrieval, labels not visible) | sonnet — 62 blind cells | no — emits candidates and reasons, never a label |
| The source-grounded second detector (r2) | sonnet agents and the deepseek API, benchmarked head to head | demoted to **signal** after measurement; one narrow directional override adopted under a pre-registered rule |
| The detector code and support scripts | Claude Code (Anthropic; opus / sonnet / haiku / fable builds, various versions across the window) | no — code, reviewed and measured before adoption |
| **The form verdict itself** | **none — zero LLM calls** | — |

[^ds]: **Discrepancy declared.** One project document declares `deepseek-chat`; the generation
    script defaults to `deepseek-v4-pro`; and no model string was written into the result files.
    **[UNVERIFIED]** — the deepseek version is not pinned in the data. Searched: the benchmark
    master document, both generation scripts, and both result files.

---

## 4. The one boundary that is easy to overstate

The verdict layer makes **zero LLM calls**. That claim is exact and it is narrow.

A substantial number of the detector's rules read a **pre-computed linguistic annotation** of the
text, and that annotation was produced upstream by a model. On unannotated
cells those parts of the rule set are **inert**, and the source says so explicitly.

Stated correctly: **the verdict layer is LLM-free; some of its input features are not.** Making
that dependency measurable by a third party — by replicating the annotator on an open local model
and publishing the delta — is an item on the roadmap, not a solved problem.

A second boundary belongs here too. The **DISTORCE column** of the published 2,000-cell table is a
composite in which an LLM content judge can override to distortion. "Zero LLM calls" holds for the
form detector; it does not hold for that column. See [RESULTS.md](RESULTS.md) §2.1.

---

## 5. Human work, itemised

- The 200 claim and myth themes and their expected values.
- The 283 gold labels. For the sealed benches the labelling was blind — the labelling files
  contained no verdicts, no weights and no highlighted sentences — and predictions were frozen for
  all six sealed benches, with a sha256 fingerprint for the latest; the earlier in-sample cells
  were labelled during development and carry no blindness claim (see PROTOCOL §3).

  An earlier deterministic detector in this project (2026-08-09, prior bench generation) scored
  92% on its 37-cell tuning set and 57% on a 30-cell sealed set: its rules had memorised the
  tuning cells (known items 60%, unseen 53%). It was demoted to a free triage signal, not a
  measure. The frozen-prediction and sealed-key discipline of the present bench exists because of
  that failure, and the blindness claims in this file should be read against it.
- The 21 arbitration decisions that settle residual cells, each tagged with the route by which the
  case reached the arbiter.
- The **D1–D4** class rules for content distortion.
- The definitional rules the detector implements — recorded in the source as quoted decisions, not
  as engineering choices.
- The pre-registration of the blind protocol: hypotheses, thresholds and the adoption rule, fixed
  and committed before the run.
- The rule rejections made **on principle against the metric**, and the self-corrections made
  against the arbiter's own earlier labels.

---

## 6. Log

Reconstructed from the project's own artefacts — commit history, sealed key files, run logs and
dated documents. **It is not a complete session-level log**: prompts were not archived, and full
model version strings are not recorded for every session. It is a reconstruction, and it is marked
as one.

Legend for **role**: *H* = human; *M* = model as tool; *M(subject)* = model under test; *D* =
deterministic code, no model.

| Date | Role | What |
|---|---|---|
| 2026-07-06 | H | Model set and benchmark design fixed and written down (5 models × 2 prompt styles) |
| 2026-07-11 | D + H | Corpus audit: both theme sets found padded with paraphrase duplicates (truths 100 → 86 distinct; myths 100 → 73 distinct); root cause named; public claim downgraded |
| 2026-07-11 | M(subject) | Bare true/false disposition bench, harsh vs gentle framing, 5 models |
| 2026-08-04 | H + M | First commit of the detector repository [^start] |
| 2026-08-10 | H | Arbitrated fix batch; item bank files restored to 100 items per set |
| 2026-08-10 | D | Two of the batch's mechanisms rejected by ablation (net regression) and set to off by default — the docstring announcing them was never updated [^drift] |
| 2026-08-12 | D | **sealed-4 built**: 30 cells, shuffled with a fixed seed, quality-filtered; predictions frozen into the key *before* labels (no sha256 field — hash attestation begins with sealed-5) |
| 2026-08-12 | H | sealed-4 labelling document produced, containing no predictions |
| 2026-08-13 | H | sealed-4 labels revised by the labeller after reading detector evidence (cases 9, 10, 13; later also 1, 2, 15) — revisions written into the key as comments |
| 2026-08-15 | D | **sealed-5 built**: 130 cells in six strata, at most one cell per theme per stratum; **signature v1** frozen (form only, no model annotations) with sha256 `68040a03…` and a verification mode that recomputes and diffs |
| 2026-08-15 | H + D | Comment-drift corrections: three inline figures found wrong and corrected against measurement. Standing rule adopted: **no figure may be sourced from a docstring or comment; every cited number must come from a run output or a frozen snapshot** |
| 2026-08-16 | H | **Blind wave 1**: 42 cells (S1 + S3a + S3b) labelled without seeing any prediction → first pure blind measurement, **2/42** ([PROTOCOL.md](PROTOCOL.md) §5) |
| 2026-08-17 | D | **Second freeze**: signature v2 `4594978b…` at the frozen v2 detector build, with annotations; the freezing tool refuses to overwrite an existing signature without an explicit flag. Three re-freeze commits on this date, one naming a different fingerprint — the operative artefact is stated in [PROTOCOL.md](PROTOCOL.md) §2 |
| 2026-08-18 | H | **Blind wave 2**: S2 tranche, 18 cells scored → 10/18 (arbitrated 2026-08-19). Consequence: the evasion branch over-fires and is **demoted from a verdict to a signal** |
| 2026-08-22 | H | **Blind wave 3**: S4 + S5, 62 cells scored |
| 2026-08-22 | M | Content pre-screen (sonnet) run blind over the same 62 cells: 44 no · 7 suspect · 11 yes; 20 citation errors routed to the D1 register; 20 disagreements sent to arbitration |
| 2026-08-22 | H | **D1, D2, D3 answered as class rules** — one human answer per family of flagged sentences |
| 2026-08-22 | D | Blind scoring against the frozen v2 vector: r1 46/62 frozen · 49/62 at the 19/08 build · 51/62 after two post-hoc rules · r2 12/14 on the routed subset · pipeline 54/62 = 87% *(this last figure withdrawn on 31/08 — see the 31/08 rows)* |
| 2026-08-22 | H | **Pre-registration document for the content judge v0.4 committed before the run**: H1/H2/H3 with thresholds, and the adoption rule fixed in advance and machine-encoded in the scoring script |
| 2026-08-22 | D | Pre-registered run executed on 45 virgin routed cells: **H2 failed, H3 held, H1 not evaluable** → the content judge demoted to pure signal; the 45 cells retired from future measurement |
| 2026-08-23 | H | **D4 extracted** from a 29-cell arbitration batch; applied at cell level it rejects 20 of the judge's own flagged cells as false positives |
| 2026-08-23 | D | Narrow directional override adopted at a cumulative 6/6 under the pre-registered rule — with the nuance that the single-run pre-registered test had returned too few flips to evaluate ([PROTOCOL.md](PROTOCOL.md) §6.3) |
| 2026-08-23 | D | Further rules measured and adopted or rejected under the 2,000-cell scan; one rule set to off for **measured inertia** — "risk of memorisation with no gain" |
| 2026-08-29 | H | Score table and citable-numbers document consolidated; the later, higher pipeline figure (59/63) ruled **no longer blind** and withdrawn in favour of 54/62 *(this figure itself withdrawn on 31/08 — see the 31/08 rows)* |
| 2026-08-30 | M + D | Content-judge coverage reaches 2,000/2,000 cells; run-log snapshot of the full table produced |
| 2026-08-31 | D + H | **Audit of the blind chain: 54/62 = 87% withdrawn.** Freeze at 07:13 on 22/08 with r1 at 49/62; two rules adopted at 07:38 whose code comments name the blind cells they came from; pipeline scored at 08:13 on that build. Replaced by **49/62 = 79.0%** (frozen, blind) and 53/62 = 85.5% (frozen-build pipeline), with the **72.6%** majority-class baseline published beside them |
| 2026-08-31 | D | **Reproducibility package for the form detector verified green** in a separate process: 260/283 in-sample, 49/62 frozen blind, and 53/62 for **r1 at the packaged build** (the live build has since moved) — a numerical coincidence with the frozen-build *pipeline*'s 53/62 (PROTOCOL.md §5 row 7b), which is a different measurement; sha256 manifest; two negative tests. Knowledge-base cell masking: a first pass found 2 of 283, the same day's full read found **15 of 283** (script-generated); masking changed no verdict |
| 2026-08-31 | D | **Bare run of the sealed-4 gate: 28/30** (Python 3.11.8 and 3.13.3, identical), misses at cases 13 and 28, both content distortions belonging to the content judge |
| 2026-08-31 | H + M | A later pass by the same author, run against the frozen artefacts rather than from memory, finds the docstring's gate figure stale and **corrects it downward**; standing instruction issued to flag any published number found superseded |
| 2026-08-31 | H + M | Release perimeter carved out of the private repository: knowledge-base-citing inputs excluded, with the consequence that the headline in-sample metric is **not** reproducible from the perimeter — stated openly rather than worked around |
| 2026-08-31 | H + M | This methodology repository written from verified extracts of the private sources |
| 2026-09-01 | D | **Redaction test on a third-party quotation.** One cell of the metric quoted 472 characters (82 words) of a 1922 translation still in copyright — the only such block found. Redacted and re-run: **no verdict changed**, all three figures identical (260/283 · 49/62 · 53/62). The public package therefore keeps the full 283-cell denominator, with the redaction declared and its test reproducible |
| 2026-09-01 | D | **Significance and power computed**: the blind 79.0% is not distinguishable from its 72.6% baseline (p = 0.160); power at n = 62 is 57%, rising to 97% at n = 175 *(power figures withdrawn 2026-09-02 — H1 had been set equal to the critical threshold 52/62, and the computation was never versioned; see the 2026-09-02 correction row)* |
| 2026-09-01 | D + H | **Arbitration coverage counted** against the files: 106/283 cells passed a review sheet, 79 of them in agreement, 103 inspected at slot level, all 62 blind cells covered — refuting the harsher earlier statement of the label-noise limit |
| 2026-09-02 | D | **In-sample four-form figure corrected: 254/269 → 253/269 = 94.1%.** The +1 rested on one cell (s4-058) whose live text is a malformed JSON wrapper that escaped the 31/08 regeneration filter; 253 is recomputable by a third party from the packaged manifest and the only figure consistent with 260/283 · 49/62 · 53/62 at the same build. The packaged text for this cell is a regenerated replacement — a different text, not a substring of the live one — with a self-consistent sha: without this note the defect is invisible to a third party |
| 2026-09-02 | D | **The 9/14 on the routed cells replaced by 8/14.** The published 9/14 came from the 07:38 build of 22/08 — the build withdrawn together with 54/62; at the frozen build r1 scores 8/14 on the frozen routing record (r2: 12/14). A second 9/14 circulating in a reconstruction file uses a different cell set whose identity check failed, and is not a source |
| 2026-09-02 | D | **Composition of the 14 DISTORCE decisions counted** from the screener's own reply, recorded at decision time rather than reconstructed: 5 decided purely on source contradiction (document-checkable), 3 on both grounds, 6 purely on hermeneutic types against the declared standard; recorded ground exists for all 14. Published in ARBITRATION.md §1 |
| 2026-09-02 | D | **Power figures corrected.** The 57/84/97% triple published on 2026-09-01 corresponds to a hypothesized +11-point effect that appears nowhere in the record; exact one-sided binomial power against the observed +6.4-point effect is 22/36/56% at n = 62/100/175, reaching 80% at n ≈ 284 and 97% at n ≈ 562. Every quoting file corrected; the sealed-bench size re-specified accordingly |
| 2026-09-03 | D | **Finding 4 (under challenge) corrected; its published version withdrawn.** The sentence in this repository reported each model's own-denominator hold rate while attributing it to “the 38 items every model accepted” — the common-denominator table of an intermediate run, which yields different rates: a splice of the kind the project's own warning box describes. Rebuilt from the run of record (2026-09-01): the common-denominator comparison is **66 items** — fable 66/66 · opus 65/66 · deepseek 65/66 · sonnet 56/66 · haiku 18/66 — against control flips of 0–1.5%; the own-denominator rates are reported separately and labelled as such. That the run is incomplete is now stated |

[^start]: **Discrepancy declared.** One provenance document states the project "started
    2026-06-14". The version history of the detector repository gives a first commit of
    **2026-08-04**. The 2026-08-04 date is the one supported by the artefact and is the one used
    here; the earlier date is likely to refer to preparatory work outside that repository, but no
    document reconciles the two. **[UNVERIFIED]**.

[^drift]: This is the origin of the project's own rule against sourcing figures from comments. The
    same file records an inline comment stating a cost that measurement later contradicted —
    a measurement that contradicted a comment claiming the rule was free — and two further
    instances of the same failure mode, corrected the same way. Every number in this repository
    comes from a run output or a frozen snapshot, not from a docstring.

### Volume of work, as an empirical figure

**91 commits since 2026-08-04**, counted directly from the repository's version history rather than
recalled, and reported as such. The point of the figure is the same as the point of this log: it is
reconstructed from an artefact, not remembered.

---

## 7. What this log does not cover

- **Prompts are not archived.** The log records what was done and by which class of model, not the
  text of the instructions given. A stricter provenance regime — logging every prompt and its
  unedited output — is declared as the standard going forward, not claimed retroactively.
- **Model version strings are incomplete.** Where a commit message did not record the build, the
  build is not reconstructed here.
- **Sessions before 2026-08-04 are not itemised.** See the start-date discrepancy above.
