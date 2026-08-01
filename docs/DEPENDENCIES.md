# Dependencies

Every skill, rule, and record `SKILL.md` names, where its pinned copy came from,
and what stops working without it.

**Vendored copies are derived artifacts.** On any drift between a copy under
`vendor/` and its source, the source wins and the copy is regenerated. A copy
here is never an authority over the thing it was copied from, and a reader
resolving a disputed claim goes to the source.

Vendored 2026-07-31. Verify a copy with:

```bash
sha256sum vendor/<name>/SKILL.md
```

## Installing them

**The maintained way to install this skill and all six dependencies at once is
[OpenCnid/dovetail](https://github.com/OpenCnid/dovetail)** — a pack that holds
each of them as a git submodule pinned to a commit, with no copies at all. Every
dependency now has its own public repository, which the pack pins and this
document records.

The commands below remain the fallback for installing from this repository
alone.

**Reading a vendored copy does not open Guardrail 15; installing it does.** The
gate is opened by *invoking* a skill, and a skill can only be invoked once it is
in a skills directory. The vendored bytes are the same bytes — what changes is
whether the harness can load them.

The two hard requirements:

```bash
cp -r vendor/prompt-engineering  ~/.claude/skills/
cp -r vendor/hypershot-protocol  ~/.claude/skills/
```

All six, if you want the skill's claims fully grounded:

```bash
cp -r vendor/*/ ~/.claude/skills/
```

Claude Code picks them up live, with no restart. Use `.claude/skills/` instead of
`~/.claude/skills/` to install them per-project and commit them with a repo.

**These are pinned copies, not upstream.** Where a dependency has a public home —
`subagent-composition` does — prefer installing from there and treat the copy
here as a fallback and a record of what this skill was built against.

## The dependency set

Read off `SKILL.md` by grep, not inherited from a list. Six skills, and nothing
else that a stranger would need to install.

| Skill | Role in this skill | What breaks without it |
|---|---|---|
| `prompt-engineering` | **Guardrail 15, hard.** Opens the gate for authoring player prompt bytes. | Player prompts get authored ungated. The skill's own § *House note* becomes a sentence the repository does not honour. |
| `hypershot-protocol` | **Guardrail 15, hard.** The player prompt frame *is* a hypershot. | The frame reads as a template with odd naming instead of a contamination-control device, and filled-in examples creep back into player prompts — the exact leak the method exists to close. |
| `subagent-composition` | The **spawn gate** (§ *When to use*) and the **isolation ledger** (§ *The players*). | Two load-bearing claims lose their grounding: that a spawn is the expensive path, and that your reasoning does not cross into a player unless you put it there. Without the second, isolation is an assumption rather than a mechanism. |
| `judge-composition` | The clean-context principle this skill generalizes, and the source of the judging seats that score utility. | § *What this is* loses its lineage, and "judges score utility" has no composition method behind it. |
| `harness-traps` | § *A loaded skill is not an applied one*, cited in § *House note*. | The gate degrades into a checklist tick — a `Skill` call that returned gets read as guidance that arrived. |
| `spark-steering` | § *Ask first — the un-tool*, behind the "a held expectation goes to the collaborator" rule. | The ground-block rule names a destination for a held expectation and cannot say what that move is or why it precedes installing configuration. |

## Pins

| Skill | SHA-256 (first 16) | chars | Source path, 2026-07-31 | Canonical authority |
|---|---|---|---|---|
| `prompt-engineering` | `87AD6993F587F094` | 5,907 | `~/.claude/skills/prompt-engineering/SKILL.md` | The Lexideck Prompt Engineering Curriculum (Matthew Murphy). The `SKILL.md` is the deployed artifact, not the curriculum. |
| `hypershot-protocol` | `A97D26B654D3CE2B` | 11,265 | `~/.claude/skills/hypershot-protocol/SKILL.md` | Same Lexideck lineage. |
| `subagent-composition` | `2F1BB2FAB87C08E8` | 17,538 | `~/.claude/skills/subagent-composition/SKILL.md` | `github.com/OpenCnid/subagent-composition` — **see the drift note below.** |
| `judge-composition` | `6271CED312BE7276` | 24,334 | `~/.claude/skills/judge-composition/SKILL.md` | [OpenCnid/judge-composition](https://github.com/OpenCnid/judge-composition), which since 2026-07-31 ships all thirteen records it cites in its own `references/`. Its design record is public in [OpenCnid/trellis](https://github.com/OpenCnid/trellis). **The pin above is behind that**; see the note below. |
| `harness-traps` | `5EEDC09973752A40` | 9,296 | `~/.claude/skills/harness-traps/SKILL.md` | This `SKILL.md`. |
| `spark-steering` | `760C7DA9282AD2B3` | 5,613 | `~/.claude/skills/spark-steering/SKILL.md` | This `SKILL.md` plus its `references/`, backed by a 373-primitive map of Claude Code surfaces kept with its research paper. |

Full hashes are in [`vendor/HASHES.txt`](../vendor/HASHES.txt).

### Drift note — `subagent-composition` — **RESOLVED 2026-07-31**

The published repository carried a 13,548-byte `SKILL.md` at commit `06ad525`,
behind the copy vendored here: it predated the § *The disproving arm* section.

**Upstream is now level.** The content was ported and merged as
[OpenCnid/subagent-composition#1](https://github.com/OpenCnid/subagent-composition/pull/1),
and `main` carries it at `c5ab4fa`. The repair went to the source rather than to
this copy, which is the direction the rule requires.

One residual difference is expected and is not drift: upstream normalises to LF
via `.gitattributes`, so its `SKILL.md` is ~246 bytes smaller than the CRLF copy
pinned here and **the SHA-256 above will not match a fresh clone.** Compare
content, not digests, across that boundary.

### Pin note — `judge-composition` is behind

The copy vendored here predates 2026-07-31, when the skill gained its own
`references/` carrying all thirteen records it cites, byte-for-byte. The vendored
`SKILL.md` is therefore ~150 bytes behind and, more importantly, its citations
resolve to nothing from inside this repository while the upstream skill's resolve
locally.

**This is not regenerated here on purpose.** The maintained install path is
[OpenCnid/dovetail](https://github.com/OpenCnid/dovetail), which pins the current
skill as a submodule; the copies under `vendor/` are the record of what this
skill was built and tested against, and rewriting that record to look current
would defeat its only job.

### What was not vendored

Only `SKILL.md` is vendored for each dependency, because that is the file
`SKILL.md` cites. Three dependencies carry more:

- `harness-traps/references/` — not vendored. The cited section is in the body.
- `spark-steering/references/` — not vendored, including
  `steer-1-levers.md` § *The un-tool*, which the body cites for the
  construction and its provenance. The body carries enough to act on; a reader
  wanting the derivation needs the source.
- `judge-composition/README.md` — not vendored.

## Rules this skill depends on

`SKILL.md` cites house rules that live in the origin repository. Each is restated
portably here so the skill stands alone — and **the origin repository is public**,
at [OpenCnid/trellis](https://github.com/OpenCnid/trellis), pinned for these
citations at
[`07bd744`](https://github.com/OpenCnid/trellis/tree/07bd7441e832aaa582a6d93e374c2e2334729830).
So each restatement below is checkable against its source rather than taken on
trust. An earlier version of this table described that repository as private; it
is not.

| Rule | Where it was cited | Disposition here |
|---|---|---|
| Measurement doctrine — instructions are specifications; a null needs a discriminating control; correctness is the whole score | § *When to use*, discipline 6, § *The cell that matters* | **Restated in full** at [`.claude/skills/self-play/references/measurement-bounds.md`](../.claude/skills/self-play/references/measurement-bounds.md). That file is what the skill cites. |
| Guardrail 15 — the two protocols run before the bytes | § *House note* | **Restated in full** in [`AGENTS.md`](../AGENTS.md) § *Guardrail 15*. It is house-wide, so it binds work in this repository directly. |
| The positive-control duty — *a null result is meaningless until the experiment has demonstrated it can produce a positive one* | discipline 6 | **Restated verbatim in the discipline itself.** The origin citation is dropped; the sentence is load-bearing and now travels with the rule that uses it. |
| Ask-the-collaborator — put an underdetermined call to the collaborator rather than settling it with standing configuration | § *The `## Ground` block* | **Restated in full** in the section, with the reason attached. Also carried in [`AGENTS.md`](../AGENTS.md). |

## Records that stayed behind

Two auto-memory entries were cited by the origin `SKILL.md`:
`project-corrosion-bound-critique` and `feedback-encoding-tracks-presentation`.

These are **unportable by construction and are not carried forward.** Auto-memory
does not cross the sub-agent boundary (see `vendor/subagent-composition/SKILL.md`
§ *The inheritance ledger*), so they were never available to a player in the
first place, and they are not available to anyone outside the machine that holds
them. The findings they recorded are written up with their addresses in
[`FINDINGS.md`](FINDINGS.md).

## A rule the origin skill did not cite

The origin repository's `composed-evaluators.md` (rule 17) appears in this
repository's build hand-off as a rule the skill depends on. Grepping the origin
`SKILL.md` for it returns nothing: the skill never cited it. It is therefore not
a dependency and nothing here restates it.
