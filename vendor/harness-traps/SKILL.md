---
name: harness-traps
description: Load before trusting what a permission rule, sandbox setting, bypass flag, managed MCP config, sub-agent delegation, or rewind or clear command actually does in Claude Code. Fires when editing settings.json or permissions.allow, permissions.deny, sandbox.filesystem, or dangerouslyDisableSandbox; reaching for --dangerously-skip-permissions; wiring managed-mcp.json, deniedMcpServers, policyHelper, or hooks; spawning, delegating to, or isolating a sub-agent; running /clear, /rewind, or /resume; or reasoning about settings precedence, tool scope, or what survives compaction. Corrects harness beliefs that are confidently wrong and fail silently instead of erroring.
---

# Harness Traps

A control's name, precedence, or presence in your tool list is not proof of what it does. Four shapes below cover where this harness's real behavior (CLI 2.1.215) diverges from what the surface implies.

**Match the shape first.** An unlisted control fitting one of these four earns the same suspicion as any named here — the shapes generalize, the individual facts do not.

## The four shapes

1. **Reach stops short of the name.** A word like sandbox, wrapper, guard, dangerous, or remote promises a boundary; the mechanism draws it narrower, and something adjacent keeps operating outside it.
2. **Merge and override run backwards.** The five-layer hierarchy (managed > CLI > local > project > user) is the working model, but several categories are carved out — some merge across every scope instead of the top winning, one replaces every peer instead of layering in.
3. **Recovery reaches less, or more, than it claims.** A checkpoint, compaction reload, sub-agent return, or clear gets trusted to be as complete or as destructive as its name suggests. The real coverage has a gap, or the real destruction has a leak, surfacing only when the guarantee is needed.
4. **A visible identifier is not the enforcement point.** A display name, a tool in your namespace, a schema field, a command — reads like the control, but something narrower and less obvious does the actual checking.

## Four that must not be looked up

These carry data-loss, privacy, or security consequences. They stay resident because needing to fetch them is already too late.

- **`/rewind` cannot undo Bash changes — data loss.** Checkpoints cover changes made through direct file-editing tools only. Anything via Bash — `rm`, `mv`, `cp` — is excluded entirely and is unrecoverable through rewind, even though Bash is the tool most likely to destroy something outright.
- **`/clear` does not put a conversation out of reach — privacy.** The prior conversation is not discarded. Once `/clear` has run in this process, the rewind menu grows a `/resume <session-id> (previous session)` entry reopening the "cleared" content for the rest of the process's life. It neither hides sensitive content nor forces a genuine reset.
- **The bypass-permissions root guard lapses inside a sandbox — security.** The refusal to start `--dangerously-skip-permissions` as root/sudo is skipped once Claude Code detects it is already inside a recognized sandbox. Root plus full bypass becomes available exactly where the refusal was the last line of defense.
- **MCP `serverName` is not a security control — security.** It is documented as such: a user-assigned label any server can claim. Only `serverUrl` and `serverCommand` matches are enforced, so a blocked server reappearing under the same name via a different URL or command passes straight through.

## Pull the specific fact when a shape matches

Eighteen further traps sit in the reference, each with the wrong belief, the actual behavior, the concrete cost, its source primitive, and a confidence tag. When a control matches a shape above, extract its entry rather than guessing:

```
Grep "{setting_flag_or_command_name}" references/steer-2-traps.md -B 2 -A 6
```

To review everything under one shape, grep the pattern heading instead. Read only matching blocks — loading the file whole spends context on eighteen facts to answer one question.

Confidence tags in the reference: `(doc)` stated in official docs or schema text; `(obs)` observed in behavior or a tool's own schema, not independently corroborated; untagged means supplied as established, not corpus-mined. **Never upgrade your own unverified belief to either tier.**

## Before you act

When a setting, flag, tool, or command is about to govern something that matters — data, security, or whose config wins — confirm its actual scope, precedence, coverage, or enforcement point against the four shapes before relying on its name.

## A loaded skill is not an applied one

The opening rule extends to skills themselves, and it bites twice — once on whether the frame is there at all, once on whether you are using it.

**First: a `Skill` call that returns is not proof the body arrived.** The tool reports `Launching skill: {name}`, and the body lands as a separate message — so a call can return, read as success, and deliver nothing. This is shape 4 at the skill boundary: the acknowledgement string is the visible identifier, and the arriving body is the enforcement point. What follows a silent miss is a session citing the skill from memory of what it usually says, and **a citation to a skill whose body never arrived is the same defect class as a quotation never retrieved** — both assert a source the session did not read. *Check, before the first citation and before any artifact commits one:* name a section heading you can actually see in this session's context. If none is there, invoke it again — a re-invocation has recovered a body the first call dropped `(obs)`.

**Second: a frame's presence in your context is not proof you are operating under it.** Loading a skill injects its text; it does not install the discipline of using that text. Unchecked, two quiet substitutions take the place of applying it — shown with `prompt-engineering` freshly loaded:

1. **You restate the frame instead of applying it.** You paraphrase the skill back — into a plan, a preamble, the top of an artifact — and it reads as compliance. It is not: **a paraphrased frame is drift, not an implementation.** Applying `prompt-engineering` means authoring *structured bytes shaped by it*; reciting "structural clarity, semantic tags, hierarchical markers" is the restatement it exists to replace. *Tell:* your output **describes** the frame where it should **be shaped by** it.
2. **You set experimental targets the frame already synthesized.** You propose to test, measure, or A/B what the frame settled — "does structure really beat magic words" — reopening a closed result as an open question. A frame's core is invariant: you **compose from** it, you do not re-derive it, and proving a synthesized claim moves behavior is anti-useful work. The only thing worth an experiment is a flank the frame *names* as unproven — never its established core. *Tell:* an experiment whose result the frame already states.

Both fail silently because both feel like rigor: restating feels like understanding, re-testing feels like diligence — while each spends the turn reopening what was closed. It is the **instance-promoted-to-frame** error run backwards: a settled frame quietly demoted to a hypothesis.

**Before you rely on a loaded skill:** cite it and let it shape the bytes — if you are paraphrasing it, you are drifting — and separate the synthesized from the open, composing from what the frame established and reserving experiments for the flank it names as unproven.

*Provenance: the frames-are-invariant, compose-don't-re-derive discipline is the composition-from-primitives thesis (collaborator M. Murphy); this section is its harness-facing corollary.*

## Which copy answered

When one name exists at two scopes, the body you receive may not be the file you edited. **On CLI 2.1.214 a user-level `~/.claude/skills/{name}/` shadows a project-level `.claude/skills/{name}/` of the same name** `(obs)` — the reverse of agent precedence, where project overrides user. Both were verified from a cold `claude -p` started inside the project directory, so this is precedence and not a stale in-session registration.

This is shape 2, and it bites hardest where a repository deliberately vendors a house skill so a clone depends on nothing outside it: the vendored copy is correct, committed, and inert on any machine that also has the house copy installed. The divergence then never executes and never announces itself.

*Check:* the delivered body prints `Base directory for this skill:` as its first line. **Read that path before believing an edit took effect.** Where a vendored copy must diverge from its house original, treat the divergence as unreachable locally and put it somewhere a reader will meet it — a `## Lineage` note in the file, not an instruction in the body.

---

Diagnosing which capability axis is short before adding one is a separate concern — see the `spark-steering` skill. It sets `disable-model-invocation: true`, so the `Skill` tool refuses it by name: read its `SKILL.md` from the skills directory it is installed in, or ask the collaborator to invoke it. Four house skills cite its § *Ask first — the un-tool* as further reading, and that is a read, never a call.
