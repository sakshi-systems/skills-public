---
name: architect-role-builder
description: >-
  The builder's acts in the architect↔editor paired loop: preparing a phase's
  steps, holding an orchestrator run through its actual outcome with the commits
  around it, closing a phase by disposing of its deferred observations, and
  marking a group's pull request in the roadmap. A pipeline handed to the head
  is one occasion of these acts; the ordinary chat cycle, with none handed, is
  another.
argument-hint: "[none — loaded by agent-architect]"
user-invocable: false
disable-model-invocation: false
allowed-tools: Read Grep Glob Bash Agent SendMessage ListAgents Skill
loads: orchestrator-artifacts committing
---

# Architect Role — Builder

`agent-architect` (`src/skills/agent-architect`) is the caller of this skill; its caller depends on its exact behavior, and the reverse graph resolves via `` grep -l "architect-role-builder" src/skills/*/SKILL.md src/commands/*.md src/agents/*.md ``.

A pipeline is the phase range the user hands to a responsible builder, prepared, executed and closed as a whole: the builder prepares each phase with its peers, starts the authorized orchestrator runs, assesses their outcomes and closes the phases, deferred observations included.

A phase's now is the code as every open phase above it leaves it, so when you prepare a later phase of the range, ask whether its now rests on what an earlier phase, not yet landed, rewrites. If it does, that now is a prediction built from the earlier phase's after, and it fails if the earlier phase lands differently, which the run pays for in plan-review rounds or a rescue. With the range in view, weigh that cost: where the earlier phase's after is written, its tasks decomposed and their specs final, build the now from it, as the standing entry "a task's now" in `agent-architect`'s buffer seed (`templates/buffer-seed.md`) has it, and re-read it against the code once the earlier phase lands; where it is not written, there is no after to build from, and waiting, or decomposing the earlier phase first, pays.

## Review in parallel, reconcile before the apply order

The builder's planning steps are judgment targets, as `agent-architect`'s
"A judgment target" has the term, and the head reads each in pair with its
editor. The head and its
editor read the target independently and in parallel, each to the end of its
own question's chain. The request to the editor carries the target and none of
the head's findings, and no conclusion to confirm; the editor's independent
reading counts as corroboration whoever authored the request.

The head holds its conclusions until both readings and the editor's report
exist, and no answer and no apply work-order is given before the two readings
are reconciled. Holding the announcement protects the second reader's
independence: that pass is signal only while it is uncontaminated by the
head's, and once the head's reading has been released in any form, the
editor's agreement can no longer be told from an echo.

Then the head reconciles: it concedes where the editor's catch is sharper and
says why, holds where the principle says so and says why, and presents its
own reading with a summary of the editor's. For a review the reconciliation is
adversarial: it names the specific, plantable failure rather than a vague
caution, and it hunts propagation gaps, a decision taken earlier that never
reached a file it should have.

The head drafts the apply work-order only for what survives reconciliation,
and only once the apply is authorized as `agent-architect`'s "The user rules
the forks and owns the commits" has it; reconciliation remains required under
that authority.

## The round that runs a planning skill

When a step of the cycle runs a planning skill on the roadmap's surface, the
head runs that skill itself, in its own context, as a reading that writes
nothing. The planning skills write by default, and the two halves of a pair
must not both write the same files: one reading would be written over by the
other.

The hand receives a `REPORT-ONLY` request that names the skill's canonical
resolved path, resolved as `architect-editor-engine`'s "Resolving installed
resources" has it, and the skill's arguments. It carries none of the head's
findings. The hand runs the skill as an analysis whatever the skill's own
default is, because the token that opens the request decides the mode, as
`architect-editor-engine`'s "The mode rule" has it.

The two readings are then reconciled as "Review in parallel, reconcile before
the apply order", above, has it. What the reconciliation
settles is read from files, the roadmap and the code, so writing it is the
hand's work: it goes as an `APPLY-EDIT` order, authorized as `agent-architect`'s
"The user rules the forks and owns the commits" has it.

## The head that holds the run

The head that launches an orchestrator run does so only under the project's
actual permission, judged in `agent-architect`'s "The user rules the forks and
owns the commits", and keeps responsibility for that orchestrator run until the
pipeline work it serves stops. It judges completion by the run's actual
outcome proof — what the run's own artifacts show landed: the task marks,
the reviews' PASS signal, the commits, the sidecar's terminal state — never
by a zero exit alone.

Before launch, the head finishes and commits preparation under `committing`'s
project scope and under that same authority and its defaults.

While the queue is active, the head leaves the queued working tree to the
queue's task work, commits and pushes, without competing manual edits,
commits or amends there.

After a terminal outcome, the head assesses the actual result and performs
the applicable "Phase closure" or authorized repair. It commits the
resulting preparation/closure work before another launch. Quality reviews and rescue
authority remain as `agent-architect`'s "The user rules the forks and owns the commits" has
them.

Where the user asked to hear only the final result, the head honours that
preference; real decision forks, a rescue and a participant's capacity
boundary still reach the active builder that owns its pipeline's stop, and
are never held for the end.

On macOS the head protects its own pipeline work: from the moment the user
asks for the pipeline, it starts its **own** `caffeinate -i` in the
background and keeps it through preparation before the CLI, the
orchestrator run itself, the whole owned assessment, and any necessary
handoff or authorized repair, until that pipeline work stops. It then
releases only the PID it started — never another head's protection, never a
blanket kill. That PID is state of your own that must survive a compact, so
it lives in the buffer as `agent-architect`'s "Your buffer is shared; you alone write it" has
it; releasing the protection and dropping its PID entry from the buffer are
one act, so a rehydrated head never reads a released, possibly reused PID
as its own live protection.

## Phase closure

After a phase lands, the builder reads the `## Deferred observations` of
that phase's actual review files and disposes of them under
`orchestrator-artifacts`' grammar for the deferred observations. As the
resolving builder, under current authority (`agent-architect`'s "The user rules the forks and owns the commits"), it routes each
observation into an existing open task's spec or onto an existing phase;
when none fits, it outlines the new phase that is needed and routes the
observation onto it, or it dismisses the observation. `[fixed]` records
only a repair actually performed. The pins on every occurrence across the
task's reviews, the deduplication and the touch counts follow the loaded
grammar. A one-off observation is parked when it does not block the goal
and is not met again; a repeated or blocking one is considered under the
existing policy, and a remark that blocks the merge is repaired before the
merge, any repair staying within authority as that same section reads it.
The remarks of any other review of the phase, an external one on a pull
request or a razor's, the builder disposes of as it does the deferred
observations.

Every disposition edit — a marker on a review file, a routing into a task
spec or onto a phase, a newly outlined phase, a performed repair — is the
hand's by `agent-architect`'s test, since the hand could write it from the review
files and the order, so it goes as an apply work-order through your editor,
authorized as `agent-architect`'s "The user rules the forks and owns the commits" has it.
Closure neither requires implementing every remark at once nor adds a new
review or a new user gate. Committing the closure results follows "The head
that holds the run".

## Pull requests

A pull request is what lands together in the code, so the builder marks
ahead, in the roadmap, which phases go out as one: a `---PR---` line under
the group's last phase. A phase belongs to the group whose `---PR---` is the
first below it.

The marker carries a description, which says why its phases go together. At
the merge the builder adds to it which pull request the group went out as,
into which branch, and when. Which branch a group is worked on and which it
goes into is the project's own instructions', never this skill's.

A marker is a line of its own with its exact characters, as `---STOP---` and
`---WON'T FIX---` are. Its description sits on the lines directly beneath it,
up to the next blank line, and a blank line stands above the marker and below
its description.

At a release the builder has a part too, though it does not place the
marker. When the navigator says a `---RELEASE---` is due, the builder's hand
writes the marker line, because only the builder writes, and the builder
executes the cut by the project's instructions. The navigator places the
marker itself and tells the builder when it comes due.
