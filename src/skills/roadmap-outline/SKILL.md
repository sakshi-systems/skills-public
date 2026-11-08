---
name: roadmap-outline
description: Create or update a project roadmap with major phases. Generates .ai-factory/ROADMAP.md — a strategic checklist of high-level goals. Use when user says "roadmap", "project plan", "milestones", or "what to build next".
argument-hint: "[project vision or requirements]"
allowed-tools: Read Write Edit Glob Grep Bash(git *) Bash(rm *) AskUserQuestion Skill
loads: roadmap-engine note
disable-model-invocation: false
---

# Roadmap - Strategic Project Planning

Create and maintain a high-level project roadmap where every direction breaks into
**phases** — a header plus a short intro closed by the `Phase note:` pointer, per
`roadmap-engine`'s format.
`/roadmap-decompose` writes and cuts tasks.

Work under `roadmap-engine` and run its **Roadmap maintenance flow** with the hooks
below.

## Hooks for `roadmap-engine`'s flow

### (a) Granularity

Each entry is a **phase**: a `### Phase N — <Title>` header, a short intro closed by
the `Phase note:` pointer in the form `roadmap-engine`'s format gives, and a phase
note. The note holds what the intro does not: the gate ("blocked on X"), the problem
today, the key contracts and pinned decisions the phase rests on, and the user's
words where they are the reason — whole, never compressed. A phase is **never a
checkbox bullet, no contract line, no `Spec:` tag**. Phases are what
`/roadmap-decompose` fills with tasks. Order by logical sequence (dependencies
first).

After the person confirms, write each new phase's note under `note`, supplying its
three hooks: the destination is the roadmap's own spec directory per
`roadmap-engine`'s "Spec destination"; the template is a free-form body holding that
content; the verbosity is the content whole. Then close that phase's intro with the
pointer to the exact path `note` returns, before `$TARGET_FILE` is saved.

Numbering: allocate each new phase one above the larger of
`> Highest pruned phase: N` and the file's highest `### Phase N` header, using the
header maximum alone when the line is absent and starting at 1 when neither
exists. Preserve every surviving phase and task number, leaving gaps after
removal. When creating the first phases of a new direction section, also emit the
`##` direction header and its preamble prose.

One parity carry-over the engine's create flow does not itself hold:
- Create-mode gather-input question (fills the engine's
  `AskUserQuestion: <caller phrases this…>` placeholder): *"What are the major goals
  for this project?"*

Links to task specs are allowed as plain markdown links inside the
intro prose — no formal `Spec:` tag, no invented task specs.

### (b) Per-entry gate

None. Outline supplies no per-entry gate hook. Do not improvise an atomicity-style
gate here; that is decompose's gate.

### (c) Target-file routing

Resolve `$TARGET_FILE` per `roadmap-engine`'s named-roadmap resolution order:
explicit argument wins, then "my roadmap", then the default `.ai-factory/ROADMAP.md`
— see the engine's "Named roadmaps" section for the slug/owner mechanics. Strategic
tier still adds **no keyword branching** on top of that resolution (unlike
decompose's test-keyword branch).

### (d) Extra update-mode actions

None beyond the engine's built-in menu (review progress / add / reprioritize /
rewrite). Outline registers no added action. Apply “Reconcile the roadmap against
the direction” in every Create and Update pass; “Confirm and apply the complete
proposal” supplies the update confirmation extension.

## Reconcile the roadmap against the direction

Apply this section in every Create and Update pass, before showing the draft.
Read `$TARGET_FILE` when it exists: compare its open phases and open `[ ]` tasks
with the direction being laid, including work that direction will later remove.

Propose deleting a contradicted phase or rewriting its intro. Propose deleting
contradicted open tasks. A phase and its note are a pair: when rewriting an intro,
preserve its `Governing spec:` line, any `**Touches:**` line and the existing
`Phase note:` pointer, with that pointer still closing the intro, and keep the intro
short. Rewrite the content that changed in place in the note at the path the pointer
names, with `Write`/`Edit` and not through `note` — never a second note, never a
second pointer.
Where work remains wanted in a different shape, rewrite the phase intro and
leave the new task cut to the next `/roadmap-decompose` pass.

Preserve every `[x]` line. A phase containing completed tasks retains its header
and those tasks; reconcile its open tasks and intro without changing completed
history. If the proposal removes a direction's last phase, include removal of that
direction's `##` heading and preamble in the same draft.

Before removing a phase from the draft, raise `> Highest pruned phase: N` to that
phase's number when higher. If the line is absent, initialize it from the highest
number removed. Keep the line in the draft even when no phase remains; new phases
in this pass allocate from that updated draft. Save the line and the confirmed
removals together.

### Capture the removal files and affected observations

Before dropping an entry from the draft, capture its associated paths from the
original roadmap, using [roadmap-prune's token selection and root joining](../roadmap-prune/SKILL.md#step-5--sweep-completed-artifacts-and-specs):

- For a deleted open task, capture only the path its literal `Spec:` tag names.
- For a deleted phase, capture only the note path its intro's literal
  `Phase note:` token names and the `Spec:` paths of its deleted open tasks.
- A missing token contributes no path. A retained phase keeps its note; completed
  tasks keep their specs.

Derive the target project root as the parent of the `.ai-factory/` directory
holding `$TARGET_FILE`. Captured paths already begin with `.ai-factory/`; join
them onto that root, not onto `.ai-factory/`. Only these explicitly captured spec
and note files accompany the proposed removals.

Read entry lines inside `## Deferred observations` sections in Markdown files
under the target project's `.ai-factory/plan-reviews/` and `.ai-factory/reviews/`,
at any depth and across roadmap segments, per
[orchestrator-artifacts, “5. `## Deferred observations` section”](../orchestrator-artifacts/SKILL.md#5--deferred-observations-section)
and
[orchestrator-artifacts, “6. Status-marker grammar”](../orchestrator-artifacts/SKILL.md#6-status-marker-grammar).
Match a complete bracketed marker in an entry's trailing marker suffix:
`[routed → <removed spec path>]` or
`[routed → <selected roadmap repo-root-relative path> § Phase N]` for a removed
phase. Compare the whole marker, closing bracket included, with the exact path
and phase number; `Phase 12` and `Phase 127` are distinct targets. Quoted marker
text outside deferred entries or their trailing marker suffixes contributes no
match. Name each matching review file, entry and target in the draft so
confirmation covers its re-routing or dismissal.

## Confirm and apply the complete proposal

For Update's Add, Reprioritize and Review progress actions, and arguments carrying
requested changes, show the draft's removals (including emptied directions), the
intro and note content of each new or rewritten phase, captured file
removals and affected pins with their proposed re-routing or dismissal. Obtain
confirmation before any save or deletion. This extends the engine's update behavior
where it applies requested changes directly. In Create mode the engine's confirm
cycle carries the same intros and note content.

After confirmation, in Create and Update alike, write the new phases' notes through
`note` (per Hook (a)) and the rewritten phases' note content in place, then save the
agreed `$TARGET_FILE` and remove only the captured spec and note files belonging to
confirmed deletions. A removed phase's note leaves with it.

## Check mode

Outline registers no check mode. With no checkboxes at the phase tier there is
nothing to scan or mark — phase progress is a derivative of its tasks (`N.M`
checkboxes written and cut by `/roadmap-decompose`, completed history removed by
`/roadmap-prune`). Create and update modes, and the engine's confirm cycle, work
as described above.

## Critical Rules

1. **Phases are high-level** — `/roadmap-decompose` writes and cuts tasks.
2. **Phases render as a header + a short intro closed by the `Phase note:` pointer,
   per the engine's format** — never a checkbox, no contract line, never a `Spec:`
   tag.
3. **NO implementation** — this skill only plans; the orchestrator implements in a
   separate run.
