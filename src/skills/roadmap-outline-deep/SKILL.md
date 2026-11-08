---
name: roadmap-outline-deep
description: >-
  Second pass over phases `roadmap-outline` has already drafted: works its
  question in the phase note the outline wrote — what diverges now between the
  docs and the code, and whether the code has the shape the phase needs to
  land as one change — modifying that note in place and never writing a second
  one. Every drafted phase qualifies — there is no per-phase gate. Use when
  deepening a drafted phase before decomposition. Trigger: "deepen phase",
  "phase note".
argument-hint: "[phase]"
disable-model-invocation: false
allowed-tools: Read Write Edit Glob Grep AskUserQuestion Skill
loads: roadmap-engine
---

# Roadmap Outline Deep — Working the Question in the Phase Note

A **second** pass over phases `roadmap-outline` has already drafted — one tier up from
`roadmap-decompose-skeleton`: it deepens the phase tier, it does not draft phases.
**Every** drafted phase qualifies — there is no per-phase gate, because a phase exists
precisely where the docs and the code diverge.

## Targeting

Optional arg — a phase (matching `argument-hint`). Default: infer the target
phase set from conversation context. Resolve the roadmap in play per
`roadmap-engine`'s named-roadmap resolution order (explicit argument → "my roadmap" →
default `.ai-factory/ROADMAP.md`).

## Workflow

### Step 0: Project context

**Read `.ai-factory/ARCHITECTURE.md`** if it exists. **Read the target roadmap**: its
phase headings and phase intros, of the whole roadmap, once before the pass works on
its phase, so that what other phases build bears on this one. The contract lines are
not part of this read; a phase's own contract lines are read when that phase is
worked. Then, for each target phase, read its phase note at the path the phase's
`Phase note:` pointer names. Then read the
docs the header, the intro and the note link — a `Governing spec:` line is one such
link and nothing more — and the code the phase and its note name, the modules, skills
or files, down to the leaf. What is not yet as those docs say is the phase; it is
visible only there, never from a doc's own prose about itself.

With the docs and the code read down to the leaf, the pass asks of every phase,
with no gate: "What shape must the code have for this phase to land as one change,
and does it have it now?" The answer is worked into the phase's note; the pass names
the need and does not make the phase.

### Step 1: Work the question in the phase note

The pass works in the note at the path the pointer names. It modifies the note by what
it finds and corrects what the docs or the code have outrun where it stands. It writes
with `Write` or `Edit`, never through `note`. It never mints a second note and never
adds a second `Phase note:` token to one intro. The pointer stays byte-identical.

The note holds what is not yet as the docs say — this and this — with links to those
docs, never a copy of the doc. Beside that, it holds the answer to Step 0's question:
the shape the phase needs to land as one change, and where the code lacks it; and,
where it lacks it, the preparation — a change to existing code with behaviour
unchanged — and whether its after can be known in advance.

- A claim that rests on a file names the file and the heading, symbol or quoted
  fragment that holds it, never a line number.
- Where no doc says how it must be, the note says so in one line.
- The pass never writes under `docs/` and never invents a link.
- The note names the preparation; it does not draft it as a phase or a task.
- The note is never a transcript of the conversation.

## Critical Rules / What NOT to do

1. Do not draft, renumber, reorder, or delete phases — this pass only deepens
   existing ones.
2. Do not write under `docs/` and never invent a `docs/` link — where no doc says
   how it must be, the note says so in one line.
3. Do not mint a second note for a phase — work in the note at the path the `Phase note:`
   pointer names, through `Write`/`Edit`, never through `note`, and leave the pointer
   byte-identical. Never a second `Phase note:` token in one intro.
4. Do not touch task lines — that is `roadmap-decompose`'s tier.
