# Validation: the clean-room test of this rebuild

[`FINDINGS.md`](FINDINGS.md) records what running the *method* established.
This file records what testing *this repository* established — a different
subject, so a different file. The primary exemplar this repository follows keeps
findings and method in `docs/` and evidence in `probes/`; a second house
exemplar keeps a `VALIDATION.md` for exactly this purpose, and that is the split
followed here.

The skill was run against itself. That is not a joke about recursion: the
rebuild is an LLM-assisted artifact whose builder had a stake in it reading
well, which is precisely the case § *Why isolation* describes.

---

## Pre-registration

**Written 2026-07-31, before any player prompt existed.** Held out of every
player prompt, every ground block, and every document any player was given.

### Target

The rebuilt repository at `github.com/OpenCnid/self-play`, principally
[`.claude/skills/self-play/SKILL.md`](../.claude/skills/self-play/SKILL.md).

### Stated engineering targets

No target was stated for this artifact before the rebuild began, so targets were
stated before the shape of the test was chosen.

| # | Target | Threshold |
|---|---|---|
| **T1** | A reader with no prior exposure, given the skill body alone, identifies its primary purpose as *searching a space that is not yet solved*, and reads isolation as the precondition serving that purpose. | Both blind readers of the rebuilt text land there. |
| **T2** | The same reader can execute one iteration cold. | Names all five moves in order, both controls and what each one failing means, and locates the player frame — without asking a question the repository answers nowhere. |
| **T3** | Every internal reference resolves, or is explicitly marked as an origin-repository citation a stranger cannot open. Every vendored hash matches. | Zero unmarked broken references. |
| **T4** | `SKILL.md` fits the surviving-prefix budget. | Under 19,900 characters. |

### Named failure modes probed

- **F1 — leak.** Does any shipped artifact hand a reader an expectation where it
  owed them ground?
- **F2 — over-trigger.** Does the description fire on near-miss queries it
  should decline?
- **F3 — break.** Are `FINDINGS.md`'s claims faithful to the sources they cite?

### Shape

Conformance to the stated targets above, plus the three named failure-mode
probes. The reader arms are **version A against version B** — the origin skill
body against the rebuilt one, both shipped artifacts each carrying its own spec.
**No arm is the skill's absence.** A null baseline is barred here for the reason
given in
[`references/measurement-bounds.md`](../.claude/skills/self-play/references/measurement-bounds.md).

### Controls

| Control | Built so that | If it comes back wrong |
|---|---|---|
| **Negative** — the origin skill body, de-identified, given to a reader asked the identical question | a reader of the *origin* text should **not** report search-and-exploration as its primary purpose | it does ⇒ the instrument does not discriminate between the two texts and the T1 result is noise, not a pass |
| **Positive** — a scratch copy of the repository with deliberately broken references planted in it, given to a second auditor arm | the auditor should **catch every planted break** | it misses them ⇒ the reference audit is blind and its clean report on the real repository means nothing |

### The cell that matters — pre-committed

- **Cell A.** A blind reader of the rebuilt body reports its primary purpose as
  leak-prevention or isolation. **If ≥1 of the 2 blind readers lands in cell A,
  the rebuild's central editorial job is not done**, regardless of any other
  result.
- **Cell B.** A cold reader cannot execute one iteration without needing an
  answer the repository does not contain anywhere. **If ≥1 occurs, the
  standalone-installability claim fails.**

### Predictions

Recorded to be scored, including against the builder's wish. Held out of every
prompt.

| # | Prediction | Confidence |
|---|---|---|
| P1 | T1 passes on the rebuilt text **and the negative control discriminates** — the origin reader does not report search as primary | moderate |
| P2 | T2 passes, but the cold reader stumbles on the controls, which sit deep in the body at discipline 6 | moderate |
| P3 | The reference audit finds **at least 2** broken references | high |
| P4 | The description **over-triggers** on generic "test my code" queries | moderate |
| P5 | The adversary finds at least one real internal inconsistency the builder did not see | high |

**Builder's stake, disclosed:** the builder wants T1 to pass, because raising the
search framing was the rebuild's stated central job. That is the outcome to
distrust, and Cell A is the cell that would catch it.

### Contamination fence

No player receives the build hand-off, the prior session's analysis of what was
wrong with the origin text, the framing quote that set the design target, any
restatement of them, or the probe by which that analysis was obtained. The two
reader arms receive de-identified copies at neutral paths with neutral names,
and are not told that a second version exists or that anything is under test.

---

## Result

Run 2026-07-31. **Eleven seats, ≈701k sub-agent tokens.** Every seat received
only its own ground block; none received the hand-off, the prior analysis, the
framing quote, or any prediction from the table above.

### Controls — read these before any other number

| Control | Outcome |
|---|---|
| **Negative** (origin body, de-identified, identical prompt) | **Discriminated.** The origin reader reported the document's primary purpose as *testing a feature so the builder's stake cannot reach the score*, and reported that the text gives primacy to the mechanism. It did not report search or exploration. |
| **Positive** (scratch repo with four planted breaks) | **Fired on all four** — two altered link targets, one altered path in a code block, and one vendored file whose bytes no longer matched its pin. It additionally surfaced a real count error nobody planted. |

Both controls behaved, so the arms below are readable as evidence rather than as
noise.

### Targets

| # | Target | Outcome |
|---|---|---|
| **T1** | Blind readers of the rebuilt body read it as a search over an unsolved space, with isolation subordinate | **PASS.** Both readers, independently. One: *"an iterative, clean-room search over a design space you have not yet solved."* The other: *"Purpose first, and the document is unusually deliberate about it… isolation gets the second section, titled to demote itself."* **Cell A empty.** |
| **T2** | A cold reader can execute one iteration without hitting a question the repository answers nowhere | **FAIL. Cell B occupied.** Three questions came back `ANSWERED NOWHERE` and two definitions contradicted each other. Re-scored after the fixes — **still FAIL**, with a different profile. See below. |
| **T3** | Zero unmarked broken references; every vendored hash matches | **PASS.** 34 internal link targets across 12 files resolved; all six SHA-256 digests matched; five `⧉ origin` markers recorded as correct-by-design. One soft break found (a `§` pointer to a non-existent heading) and fixed. |
| **T4** | `SKILL.md` under 19,900 characters | **PASS, barely — 19,879, headroom 21.** Recorded as marginal rather than as met: the body is at its ceiling and the next addition needs a matching removal. |

### Predictions — scored, including the two I got wrong

| # | Prediction | Outcome |
|---|---|---|
| P1 | T1 passes **and** the negative control discriminates | **HIT** |
| P2 | The cold reader stumbles on the controls | **HIT, and understated.** I predicted it would find them hard to locate. It found them *definitionally unbuildable* for a single-candidate run. |
| P3 | At least 2 broken references | **MISS** — zero. |
| P4 | The description over-triggers on generic queries | **MISS** — a blind judge scored 20 items against a key it never saw and agreed with the item-smith on **20/20**, including all six near-misses built from the description's own vocabulary (AlphaGo self-play for a game bot, "red-team" for a pen test, "clean-room" in the IP sense, isolated sub-agents for a parallel migration, "adversarial" for a word game, "does it hold up" for a rate limiter). |
| P5 | The adversary finds at least one real internal inconsistency I did not see | **HIT** — eleven attacks and six contradictions, most of which held on verification. |

**Calibration: 3 of 5.** The shape of the misses is the useful part. Both were
optimistic about *substance* and pessimistic about *mechanics*: the repository's
links, hashes and triggering were in better shape than I predicted, and its
conceptual coherence was in considerably worse shape. That is the direction a
builder's error runs when the builder has been proofreading rather than
executing.

### What the seats found

The load-bearing failures, each verified against the bytes before being acted on
(discipline 8), and each now fixed:

1. **The isolation ledger was stated more strongly than its own source.** The body
   said your reasoning and stake *do not cross* into a player, and called that the
   whole mechanism. `subagent-composition` puts project `CLAUDE.md` in the
   **crosses** column, and this repository's own probe harness predicts it. A
   hypothesis written in `CLAUDE.md` reaches every seat with no prompt audit able
   to find it. *This was the best finding of the run* — a live leak in the
   mechanism claim itself.
2. **"Judges score utility" named a seat that did not exist.** It appeared in the
   description, the opening, the README and the banner, and in none of the five
   moves, the player table, or the return contract. Four seats found it
   independently.
3. **`bad_cell` was defined two incompatible ways** across three files —
   falsifying-the-prediction versus condemning-the-candidate. They coincide only
   when you predict success.
4. **The barred null arm and the required positive control contradicted each
   other.** The positive control was defined as "a condition built so the
   untreated arm demonstrably fails," which is the with-versus-without comparison
   the same file bars.
5. **Discipline 5 forbade invented scenarios; discipline 10 instructed a blind
   agent to invent the items.** Both inherited from the origin text, never
   reconciled.
6. **The probe harness's own positive control hugged the axis** — a canary in the
   agent's own body, zero distance along "which foreign channel reaches a
   player." That violates discipline 6's span requirement, stated in this
   repository's own skill.
7. **The harness computes no verdict**, and the README claimed it "prints a
   matrix" and "proves" things. The expected table is a static block printed
   unconditionally.
8. **`docs/DEPENDENCIES.md` claimed six dependencies "read off `SKILL.md` by
   grep"; grep yielded five.** `spark-steering` had lost its citation in the
   rewrite. The stated derivation did not produce the stated result — discipline 9
   violated inside the dependency inventory.
9. **`docs/FINDINGS.md` carried a wrong date, an undercount of grounds, and an
   undercount of its own self-corrections** — and reproduced a phrase from the
   wrong source document, which is derived-source substitution committed inside
   the summary of a run whose subject is derived-source substitution.
10. **Four claims had no support in any source**, two of them inside the
    "what this file does not establish" section.
11. **`SKILL.md` linked to `../../../docs/…`**, which dangles once the skill is
    installed on its own — the same class of unportable reference the rebuild
    existed to remove, reintroduced by the rebuild.
12. **The description was over the 1,024-character cap** enforced by the Agent
    Skills standard and the upload path. It installs by hand and is rejected on
    publish. The origin was over too, at 1,157; the rewrite made it worse at
    1,431.

### Not fixed, and why

- **The interface has no iteration state handle.** A caller is told to iterate and
  given no run identifier or prior-round slot, so each call is a fresh block and
  the search loses its memory between rounds. Real, and a design change rather
  than a correction.
- **No return shape for abort, budget exhaustion, or halting to ask the
  collaborator.** The interface documents a clean return and a fired control, and
  nothing else.
- **No way for a caller to *detect* that a dependency is absent.** The degradation
  table specifies responses to a condition with no stated way to observe it.
- **No item-count, class-balance, or sample-size guidance** anywhere. Arguably
  correct — the metric is explicitly free — but a reader assembling an item set
  can score 100% on a set containing one class and report success.
- **The "beats the baseline" bar is stated as a conceptual entailment and is
  arguably empirical.** The adversary's strongest philosophical attack: an
  instruction constraining behaviour and an instruction improving a measured
  outcome are different claims, and the rule as written exempts this repository
  from the one measurement it would most likely lose. Recorded unresolved.
- **`probes/run-probes.sh` has never been run and recorded.** No result matrix
  ships here.

### T2 re-scored after the fixes — still failing, and differently

A second cold-executor seat received the **byte-identical prompt**, on the
corrected text, with no knowledge that a prior run existed or what it found.

**What the fixes bought.** The seat produced a complete 13-step ordered
procedure, where the first could not determine what belongs inside move 4 at all.
It built **both controls correctly** and cited the new material to do it —
the single deepest failure of the first run, the positive control being
undefined for a single-candidate run, is closed and was used. It also correctly
picked up the `CLAUDE.md` audit, the two-cell pre-commitment, and the fifth-seat
naming requirement, none of which existed before.

**What still blocks it — and two of these the fixes introduced.**

| Blocker | Origin |
|---|---|
| `SKILL.md` promises install commands in `DEPENDENCIES.md`; that file had none, and the two gate skills have no public source listed. A stranger is stopped at **step 1 of 13** with the bytes in hand and no sanctioned way to install them. | **Introduced by the fix pass.** Fixed now — `DEPENDENCIES.md` § *Installing them*. |
| The judge seat is now required, and `judge-composition`'s schema selects from four registries this repository never enumerates. Present-as-text and unexecutable — a case the degradation table had no row for. | **Introduced by the fix pass**, by promoting the judge to a seat. Fixed now — a `present but unexecutable` row. |
| `CLAUDE.md` audit instructed with no remedy stated if it *is* contaminated. | Introduced by the fix pass. Fixed now — `RUNBOOK.md` move 3. |
| No item count, class balance, or sample size anywhere. | Carried over; disclosed, not fixed. |
| Whether a degenerate always-positive baseline is a barred null arm or a legitimate target. | Carried over; genuinely unruled. |
| How a *found* corpus gets keyed blind — discipline 10 covers generated items only. | Carried over. |
| Who executes the candidate, in what harness, with how many repeats. | Carried over. |
| Iteration inheritance; abort and budget-exhaustion return shapes. | Carried over; disclosed. |

**The honest read: fixing raised the floor and introduced two new hard stops.**
That is the loop working and it is also the cost of working on a live artifact
between rounds. Both new stops are now closed, and **that closure is itself
unvalidated** — no seat has read the current text. T2 remains **failed** on the
record until a third cold-executor arm passes it.

The seat also noted, unprompted, that its own stuck points overlapped heavily
with this file's § *Not fixed* list, and read that as evidence the disclosure is
honest rather than that the gaps were closed. It is right on both counts.

### What this run does not establish

- **The fixes are unvalidated.** Every finding above was fixed after the seats
  returned. No arm has read the corrected text. A re-run of the cold-executor arm
  is the next iteration's first move, and until it happens T2 stands as **failed**.
- **The trigger result is scope-legibility, not routing.** The judge saw the
  description alone with no competing skills in the roster.
- **One seat, one model.** Every seat was the same model family on the same day.
- **This write-up is not blind.** The seats were; the person assembling their
  output into this table wanted the result to be good.

### Next variable

The axis this iteration manipulated was *framing* — whether raising the search to
the surface changes what a cold reader takes the skill to be. It moved, cleanly
and in both readers.

**Note on who chose it.** The paragraph below was written by the builder. The
skill says the next variable should be proposed by a blind agent from the run's
telemetry, and iteration 2 corrects that by putting the choice to such an agent
before anything else is composed. The builder's own pick is recorded here so it
can be compared against the blind one rather than substituted for it.

What the iteration priced is the axis it did **not** manipulate: **the machinery
did not follow the framing.** Judges, iteration, and utility scoring were raised
in the prose and left unwired in the seats, the return contract, and the interface.
The next round's variable is *executability* — can a cold reader complete a run,
not just describe one — and its instrument is the cold-executor arm re-run against
the corrected text, with the questions it could not answer as the item set.
