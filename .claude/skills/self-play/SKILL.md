---
name: self-play
description: >-
  Run a clean-room self-play search over a problem space you have not solved
  yet — an AlphaGo-shaped loop over text where isolated sub-agent players (a
  blind gatherer, an adversary, a blind evaluator, a blind judge) probe a
  candidate design and each iteration picks the next variable to vary.
  Isolation is what makes the output evidence: each player is blind to the one
  thing that would let it serve the builder's wish, so your reasoning, intent
  and stake cannot leak into the evidence or the scoring. Use when "does this
  actually work / discriminate / hold up?" is open and your own read is not
  trustworthy evidence — a rubric, admission gate, classifier, router,
  retrieval strategy, prompt template, judge, summarizer, or agent policy.
  Triggers include "explore this design space", "red-team this design", "does
  this rubric hold", "clean-room eval", "validate this without fooling myself."
  Do NOT use for a deterministic feature directly checkable without a clean
  room, or when the ceremony costs more than the answer.
---

# Self-Play

## What this is — a search, not a checkup

Self-play plays a design space against itself. You put up a candidate — a rubric,
a gate, a prompt, a judge, a policy — and isolated players attack it, feed it, and
score it. What comes back is not a verdict on the candidate alone; it is a reading
of the *space around* it, which is the thing you did not have before.

Four properties make it a search rather than a test:

- **The space is unsolved.** You run this where you do not already know the
  answer. A question whose outcome is entailed by the design is not a search.
- **The variable is selected, not fixed.** Each iteration ends by naming what to
  vary next. That selection is itself work an agent does, and doing it in a clean
  room keeps it from being your hunch in a lab coat (disciplines 6, 10).
- **A judge scores utility, and it is a seat.** The output is not "pass" but a
  scored claim about how useful the candidate is on the manipulated axis, with
  its control state attached — from a blind judge seat in the roster below, not
  from you reading the evaluator's table.
- **Iterations compound.** A round returning nothing new prices the next one.
  The method has withdrawn its own earlier findings this way.

The five moves below are **one iteration** of that search, not the whole of it.

## Why isolation — the precondition, not the purpose

**The builder cannot be trusted to evaluate their own LLM feature.** Their
reasoning and their stake in the outcome leak into the evidence and into the
scoring, and it reads as rigor the whole way down. This is the
self-invested-claimant problem, applied to your own prototype.

So a search run by the builder alone returns the builder's prior with apparatus
around it. Isolation removes the builder by construction — players each lacking
exactly the knowledge that would let them serve the wish. A gatherer who does not
know the hypothesis cannot curate toward it; an evaluator who never sees the
prediction cannot mark an answer sheet; an adversary who never sees the intent
cannot perform to it.

Everything else in this skill serves that one property, because without it the
search is a mirror.

## When to use — and when not

**Use** when exploring or validating almost any LLM-assisted feature where "does
it actually work / discriminate / hold up?" is open and your own read is not
trustworthy evidence — a rubric, a gate, a classifier, a router, a retrieval
strategy, an agent policy, a prompt template, a judge, a summarizer, a defeater.

**Do not use** when the feature is deterministic and directly checkable without
a clean room, or when the clean-room ceremony would cost more than the answer is
worth — over-ceremony is a real failure. Apply the **spawn gate** from
`subagent-composition` before fanning out: spawning a sub-agent starts it cold
and re-derives context you already hold ("the expensive path"), so spawn only
when isolation or context economy earns it.

**Also do not A/B an engineered instruction against a base-model baseline**
("does it help"). An instruction constrains the model to its spec, so outscoring
an unspecified run is entailed. Measure against a stated engineering target, or
probe a named failure mode — leak, over-trigger, break. Version A against
version B is fine.

Running a `without_skill` arm is not itself the error. `better-skill-creator`
runs one to find assertions that pass either way and so discriminate nothing,
which is diagnostic and sound. The error is reading its delta as evidence the
skill works. Shapes, and the distinction between a null arm and a required
positive control, in
[`references/measurement-bounds.md`](references/measurement-bounds.md).

## The invariant skeleton — the five moves, always in this order

The order is not cosmetic: each move closes a leak the previous one opened.

1. **Pre-register the prediction** — before any prompt or any evidence exists.
2. **Build the neutral ground — blind** — assembled by someone who does not know
   the hypothesis.
3. **Compose the clean-context players** — each isolated, each missing the one
   thing that would let it serve your wish.
4. **Evaluate blind** — the scorer sees neither the prediction nor which
   condition it judges.
5. **Calibrate honestly** — score the pre-registered prediction against the
   result, including the outcome you did not want.

Invariant: these five moves and the isolation each protects. Free: the domain,
the ground, the players' selections, and the metric. Swap those and the same
skeleton searches a water-chemistry claim, a comedy corpus, a codebase, or a
flat-Earth argument. Four such runs, with addresses, scope limits, and the three
occasions the method withdrew its own findings:
<https://github.com/OpenCnid/self-play/blob/main/docs/FINDINGS.md>.

## Across iterations — what the search actually selects

Move 5 ends an iteration; it does not end the search. A finished run hands the
next one three things, and naming them is what turns a sequence of tests into a
search: **the axis that moved, or was shown inert across conditions that span
it**; **the next variable, chosen by an agent that cannot see your wish** — steps
an agent takes in isolation are independent of your experiment, steps you take
are not; and **the instrument's floor**, which prices what the next round can
detect. A round returning nothing new is not wasted; it is the boundary of the
space, recorded.

**Stop when one of three is true**, and say which: the axis is inert across
conditions that span it; the effect is below the floor and you have priced a
sharper instrument; or the next variable costs more than the decision it would
inform. Iterating with no stopping rule is how a search becomes a habit.

## Before you author a player prompt

Invoke `prompt-engineering` **and** `hypershot-protocol` via the Skill tool, then
author against them (Guardrail 15). Confirm each body actually arrived — a
`Skill` call that returned has not yet delivered anything (`harness-traps`
§ *A loaded skill is not an applied one*). **Both must be installed, not merely
readable as text:** invoking opens the gate, reading a vendored copy does not,
and a vendored copy is derived rather than an authority over its source.

Pinned dependencies, install commands, and what breaks without each:
<https://github.com/OpenCnid/self-play/blob/main/docs/DEPENDENCIES.md>.

## The players — composed per context, isolated by construction

A player is a clean-context sub-agent given exactly its input and blind to one
specific thing. Four roles recur; compose more or fewer as the search needs, but
each must buy a blindness no other player buys, or it is decoration.

| Player | Its job | Blind to (load-bearing) |
|---|---|---|
| **Gatherer** | assemble the neutral ground / reference corpus | the hypothesis — else it curates toward the answer |
| **Adversary** | construct the strongest attack on the target | the builder's intent — else it performs to the wish |
| **Evaluator** | apply the test to each item and report | the prediction, and which condition it judges — else it marks an answer sheet |
| **Judge** | score the utility of what the run returned, on the manipulated axis | the prediction and the builder's stake — else "useful" means "what I hoped for". Compose it with `judge-composition`; a run without this seat returns a verdict, not a utility score, and says so |

A fifth seat is easy to miss: **whoever runs the candidate over the items is not
automatically the evaluator.** Name it, and decide on purpose whether the seat
that executes may also score — sharing them is a defensible economy; doing it
without noticing is not.

### The isolation ledger, and the channel it does not close

From `subagent-composition`: because a spawned agent starts cold, your
conversation history, reasoning, stake, and prediction **do not cross** into a
player unless you put them there. So the one thing to never hand a player is the
very thing it is blind to.

**That ledger has an exception, and it is the one that will catch you.** Project
`CLAUDE.md` **does** cross into every player. A hypothesis or a stake written
there reaches every seat without anyone putting it in a prompt, and no
player-prompt audit will find it. Auto-memory and skills *you* have loaded do not
cross; `CLAUDE.md` does. **Read the `CLAUDE.md` your players will inherit and
audit it for the hypothesis exactly as you audit a ground block.** This is
discipline 1 — the channel moves — with a specific address; the repository's
`probes/` harness keeps the claim honest on your version.

### Player prompt frame (a hypershot — free variables, no leaked content)

Author every player prompt as a frame, never a filled example; a concrete
expectation in the prompt is the leak you are searching against. Invoke the two
gate skills before writing these bytes.

```md
{Role_As_A_Disposition_Naming_What_It_Prioritizes_Refuses_And_Reports}

## Ground
- {Real_Artifact_At_Path_Line_Or_Named_External_Corpus_Never_An_Invented_Scenario}
- {What_Counts_As_A_Fact_Here_Stated_Without_Naming_The_Hypothesis}

## Task
{One_Bounded_Objective_That_Does_Not_Reveal_What_Outcome_Is_Wanted}

## Return
Reply in exactly this shape:

{Nested_Deliverable_Frame_With_Every_Claim_Carrying_Its_Address}

If {Blocking_Condition}: report what you found and stop. Do not {Named_Wrong_Continuation}.
```

The evaluator's frame additionally withholds the condition label: it judges one
undifferentiated list and never learns which items are live and which are
controls.

### The `## Ground` block carries relevant context only

Ground is what a player cannot derive from a cold start and needs in order to
look: `{Authorship_And_Provenance_Of_The_Artifact}`,
`{Roster_Addresses_And_Where_To_Read}`, `{What_Counts_As_A_Fact_Here}`. An
expectation — `{What_You_Believe_The_Player_Will_Find}` — is never ground. It
contaminates even when it is true, and most of all when it is true: a player
handed a true expectation hands it back, and nothing in the report separates that
from a finding. They separate on one question: *does this let the player look, or
does it tell the player what looking will turn up?* **The probe you already ran
falls on the second side** — handing over the method hands over the finding with
one step of deniability attached.

A held expectation has one destination and it is not a prompt: the un-tool
(`spark-steering` § *Ask first — the un-tool*), which ends the tool call and puts
the question to the collaborator. Reach for it before any lever that installs
standing configuration, because settling an ambiguity by installing config
answers your own question with your own guess and keeps it for every later turn.

*(A ceremony had every artifact this skill asks for — pre-registration held
separate, isolated players, an audit seat — and its independence was still ruled
unestablishable, because one ground block carried the composer's probe. Audit the
ground blocks, not the apparatus:
[`references/ground-block-failure.md`](references/ground-block-failure.md).)*

## The ten disciplines

1. **Pre-register before the prompts exist.** A forecast that shares bytes with
   the prompt *or with the evidence the players see* is a work order, not a
   forecast. **The channel moves — audit for the leaked content, not its
   location.** *(A builder kept the forecast out of the task text but left it in
   the evidence corpus the evaluator read; the "hit" was tautological and
   calibration went 1-for-4 → 0-for-4 on correction.)*

2. **Build the ground blind.** Whoever assembles the reference material must not
   know the hypothesis, or they select toward it. *(A blind-built fact base is
   what made the one clean result trustworthy — and it surfaced, unprompted, the
   very material a corrosive objection would later feed on.)*

3. **Evaluate blind.** The scorer sees neither the prediction nor which condition
   it judges. Otherwise it grades against a key.

4. **Never curate your own evidence universe.** The builder alone in the curation
   seat is the unauditable layer. Disclose your conflict and run blind *whenever
   you have a stake in the outcome* — which, if you built the thing, you do.

5. **Real variables.** Bind each game to a real artifact at `path:line` or a real
   external corpus. A wrong answer must be checkable by anyone, not gradeable
   only by you. *(An invented scenario is gradeable only by its inventor — the
   leak, wearing a lab coat.)* **What this bars is you inventing the items**, not
   generation as such: discipline 10's blind item-smith with an independently
   adjudicated key is the sanctioned construction, because the key is checkable
   by a party with no stake. Prefer a corpus the world supplied where one
   exists — it is harder to fake and, in the record below, it succeeded where six
   hand-built attempts had failed.

6. **Controls first.** Run the negative / control cases before the live ones. **A
   control failing is the signal to STOP, not to push on** — it means the test
   itself is broken and any live result would be noise. **And a positive control
   that will not fire across escalating designs is itself the finding** — the
   effect is below your detection floor, not absent-because-you-say-so; you may
   then report *no detectable effect*, never *validated*. This is the
   **positive-control duty**: *a null result is meaningless until the experiment
   has demonstrated it can produce a positive one.* **A control must also *span*
   the manipulated axis, not hug the candidate** — a control at zero distance
   from the candidate measures format compliance, not discrimination. *(Testing
   whether the word `affirmation` biased grounding judgments, the positive
   control `proof` never fired through three escalating designs; a later round
   spread the label across the whole connotation axis over 12 blind trials and
   every label still produced identical verdicts, which is how you tell an
   *inert* variable from a merely *untested* one. Both cases in full:
   [`references/discipline-cases.md`](references/discipline-cases.md).)*

7. **Sound-target discipline.** To isolate a degenerate case you need a *sound*
   target; against a defective one, legitimate findings are always available and
   the degenerate case never appears. *(Six probes failed to produce the variable
   because every target handed to them had real defects to find.)*

8. **Sub-agent output is data, not authority.** Players find real defects *and*
   overreach in the same report. Verify every load-bearing claim against the
   bytes before acting. *(A player correctly falsified a design bound and, in the
   same run, mis-cited an unratified table as ratified — both in fluent,
   confident prose.)*

9. **Label the operationalization.** Turning a claim into a testable rule is
   interpretation, and it is yours. Disclose it as the builder's reading; never
   smuggle it in as the claim itself. *(A builder silently read a one-line
   criterion three different ways across three turns, each time substituting the
   reading for the words.)*

10. **Build subtle ground truth iteratively, and verify the key blind.** A subtle
    effect only shows on *marginal* items — but one-shot generation produces clean
    textbook cases the real variable can't move (a clear-cut item is decided by
    its own defect, not by whatever you are testing). Give a blind sub-agent a
    goal and let it **iterate in its clean room** — draft, adversarially
    stress-test its own answer key, refine — since multiple steps it takes in
    isolation are independent of your experiment. Then have a **second** blind
    agent independently adjudicate that key: the item-smith's key is *data, not
    authority* (discipline 8, turned on the ground truth itself). Keep only items
    where the two agree; discard the genuinely ambiguous rather than arbitrating
    them yourself, which is the builder re-entering the seat. *(A clear-cut first
    round could not move even a maximally-biasing label; only humanized marginal
    items, iterated and independently key-verified, made the run interpretable at
    all — [`references/discipline-cases.md`](references/discipline-cases.md).)*

## The cell that matters — pre-commit it

Pre-commit **two** cells, as cells in the result table rather than as vibes. They
are different cells and collapsing them is a real error, because they only
coincide when you predicted the candidate would work:

- **The falsifying cell** — the result that would show your *prediction* wrong,
  whichever direction the prediction points. This is what calibration is scored
  against. A builder who predicts the candidate is broken has a falsifying cell
  showing it works, and that cell is just as binding.
- **The condemning cell** — the result that makes the *candidate* insufficient
  regardless of the headline verdict. *If ≥1 item lands here, the feature is
  insufficient however good the summary number looks.*

Naming them separately is what stops a headline outcome every candidate design
would produce (the tautological win) from reading as evidence that yours works.

**Say what fills a cell before you can see who fills it.** If a cell turns on a
word — *corrosive*, *unsafe*, *hallucinated* — put the rule that sorts items by
that word into the pre-registration and have a blind seat apply it. A cell
adjudicated after unblinding by the person with the stake is scored by the stake,
however blind the run that produced the items was.

## Failure modes — run this when a search feels like it is working

- **Leak by evidence** — the task text is clean and the tell is in the corpus, or
  in `CLAUDE.md`. Audit content, not location (discipline 1).
- **Curating your way to the answer** — three broken experiments in a row usually
  means you are still in a seat you should have vacated (disciplines 4, 7).
- **Reading the outcome you wanted** — score the pre-committed cells, not the
  story. **Trusting the player** — confidence is not verification (discipline 8).
- **Ceremony for its own sake** — if the answer is directly checkable, a clean
  room is theater. **A search stopped at one iteration** has mapped nothing.
