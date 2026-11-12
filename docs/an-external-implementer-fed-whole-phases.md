# An external implementer fed whole phases — one way of working beside the orchestrator

A project does not have to execute its roadmap through the [orchestrator](sakshi-harness/skill-cycle.md#исполнение--orchestrator). It can hand the work to an implementer that lives outside the family: an agent on another host, with sessions of its own, reached through an API. The architect pair — a head and its hand — then works with it directly, and this document describes the loop that settles. It is one way of working among the family's possibilities: it stands beside the orchestrator, replaces nothing, and is no rule for the skills. No skill depends on it. What follows is what the practice settles into, including the corrections the user made on the way.

## What differs from the orchestrator

The orchestrator is roles that verify one another: a planner, a plan reviewer, an implementer, a code reviewer, with PASS signals and a sidecar for every task. The external implementer is one agent that plans and builds a whole unit alone and answers with a report. Nothing around it verifies it. All verification is the pair's, and it rests on one distinction: the implementer's report is a claim, and the pushed branch and its diff are the evidence. Completion is judged by the branch, the diff, the head's own test run and the head's own probes, never by the report.

The implementer's sessions are the other difference. Each API call is a session of its own, and the sessions know nothing of one another beyond whatever notes the implementer keeps for itself. The user named this the weakness of such an implementer and, in the same breath, perhaps its strength: no message can lean on a previous one, so each carries what it needs.

## Phases whole, and tasks when the owner splits one

The work is cut into small phases, each with one reason to land, and each is fed whole to the implementer, one at a time, until the owner splits one into tasks. A phase fed whole has no task specs of its own. Its note is self-sufficient: what is true now, read from the code with exact values; what must be true after; what breaks on contact; and the user's own words where they bear on it, in the original and with an English rendering. It is the same three parts a [task spec holds](what-a-task-carries.md#what-a-spec-holds), carried by the phase, and its "now" is the code as the phases above it leave it, so the note is rebuilt from the main branch each time a phase lands.

Phases go whole by default because of the user's correction. The head had cut a phase into three tasks with specs, as the planning chain does for the orchestrator. The user asked whether the planning chain had to be followed that far: he had meant the implementer to be fed phases and to sort the rest out itself. The tasks were rolled back, and a phase that is too large is cut into smaller phases. The split into tasks is the owner's word and not the head's: later, on one large phase, the user asked to lay it out as small tasks, the way the head builds them for the implementer, with the pair's own tasks beside them in the same roadmap. Each task line then names its owner, the implementer, the owner or the pair, and each task has a note of its own in the same sections as a phase's.

## The stop sits directly under the phase in work

The roadmap's stop line sits directly under the phase the implementer holds. The user's words for it: the stop goes under the phase that was handed over, and when the implementer finishes, the head checks and sends it fixes until the quality is satisfactory. The line shows where the seam is: everything above it is in the implementer's hands or landed, everything below it waits. When the phase lands, the stop moves down by one phase. Where a phase is split into tasks, the stop stands directly under the task handed over and moves down by one task, and no line owned by the owner or the pair stands above it: a runner that reads the roadmap would take such a line for work to execute, so those lines wait below. In this way of working the stop marks what the implementer holds, which is why it stands beside the [markers paragraph](philosophy/context-tree.md#две-карты-входа--время-и-пространство) without contradicting it.

The first placement was lower, after a group of phases, and the user corrected it to the phase in work.

## The message

A phase goes out in one message. A short header in English comes first:

- who sends it, and that the sender is an architect whose word carries the user's where the user has ruled so;
- that this is a phase, or a task of a phase the owner split: the implementer plans it itself and builds it whole;
- the branch to push, named for the phase and cut from the current main branch;
- that it neither merges nor deploys;
- that it keeps the governing spec current in the same branch, without renumbering what exists, and puts no plan reference into code;
- that it runs the tests;
- what it reports back: per section of the note's "after", and what it could not check.

The phase note follows, verbatim. Every message opens with its sender and ends with the report it expects.

The same shape serves the questions the head sends between phases, with one more rule: the question is the narrowest one that decides the next step. Several questions asked at once to choose a single mechanism cost the implementer a long stretch of its time, and a question that sends it measuring more than the decision needs is a question too wide. A measurement the implementer is asked to make is passive reads and probes in a scratch directory, never an operation on a live thing: refreshing a copy of a live credential can rotate its token and sign the original out.

## The review loop

When the branch is pushed, two independent readings start. The head reviews the diff, runs the tests in a throwaway checkout, and probes the risky paths itself: crafted inputs, an isolated home directory, the rendering a user would see. Its hand reads the same diff as a `REPORT-ONLY` round, given the target and no findings, so the two readings are produced independently, as [the split of the paired loop](paired-loop.md#where-the-split-falls) asks of a judgment. The two are then reconciled. The independent reading finds defects the head has missed; the head's probes find what only a run shows, such as a value a server answers with being written unescaped into a file a shell reads.

The result goes back to the implementer as one fix-round message per review, listing each defect with the scenario in which it fails and the "after" it must meet. The review repeats until the quality holds. Where the work goes whole by phase, the user calls these fix rounds tasks, and they are messages to the implementer, never entries of the roadmap. Where the roadmap holds tasks, a fix round goes back as a new task line under the task it repairs, with a short fix note listing the findings in the note's own sections, the stop moved under it, to the same implementer session.

A review also catches what the head itself pinned: a destination a note names for a setting can be a place the target tool does not read, and then the change does nothing. What a note pins as a destination is checked against what the target tool actually reads.

## Landing, and a separate call to deploy

When the quality holds, the head lands the branch on the main branch. Where a project lands by fast-forward without a pull request, that is the user's ruling; a project that merges through pull requests keeps the user's merge. Deployment is a separate call to the implementer, which asks for evidence: the deployed commit, the health of the service and a probe of the behaviour the phase changed. Where the network allows, the head runs one probe itself, such as checking that a removed route now answers as absent.

## Docs between phases

While no implementer branch is open, the pair writes the behavioural governing spec on the main branch and commits it, and the next phase's note points at the spec's rule ids instead of restating them. The direction of change holds as ever, docs then roadmap then code: the pair writes the docs, the implementer writes the code and only the spec lines its code forces. A spec's sections and rule ids are never renumbered; a new one takes the next free number. The project's README describes what runs, and changes with the code.

The user's corrections bound what the pair writes in the product repository. The tree is clean before the implementer is loaded: a draft left uncommitted there collides with its work, so it goes back to the head's own folder as a diff. Beyond docs, writing into the project is the implementer's job, and the pair does not spend it.

## A second hand beside the editor

The implementer is a second hand of the architect, beside its editor, and the two hands divide the work by what they write: the editor writes the docs and the planning artifacts, the implementer writes the code. The head arranges the pipeline that suits that pair. The split is the one [docs between phases](#docs-between-phases) already draws, seen from the head's side. On a large project the razor and the navigator join the pair as they join any cycle, reading the same steps at their own heights.

## Fed task by task, inside the architect's own session

The sections above say how a phase is fed and when the owner splits one into tasks. The implementer can be fed task by task, one at a time, from inside the architect's own session. Then nothing separates what is planned from what lands: the task is ordered and its code arrives in the same session that ordered it, with no delay between the plan and the tree. The structure's change shows step by step, each landing read against the plan as it stands.

## The architect who ordered the work reviews and repairs it

In place of the orchestrator's review and rescue cycles, the architect who ordered the work reviews the landing and repairs it, because everything is at hand: the plan it wrote, the diff, the tests and the evidence the branch gives. This holds only while the review is two hands plus real runs: the head's own pass, with real probes on the real system and a real click where a client is touched, and its hand's independent pass, as [the review loop](#the-review-loop) describes. The defects surface in the hand's independent pass or the head's real probes, never in the implementer's report; with one hand the review is no review. The repair is a fix round that goes back to the implementer, as [the review loop](#the-review-loop) says, and the head never fixes the branch itself.

## The channel

The implementer is reached by one synchronous call per message, run in the background. The real signal is the pushed branch, which the head watches by asking the remote for the branch's head. A call cut on the head's side does not stop the implementer's work. Two calls never run on one branch at once; the next item waits in the head's own ledger. A credential that opens the implementer's host never leaves that host.

An implementer reached this way needs nothing of the family but a message, and what it receives is a note that stands alone. That made the door into Sakshi more visible: an outside agent enters the work through a plain API and a self-sufficient note, without learning the family's artifacts. The user's reading of the whole is that, as an experience, it is interesting, and that it is for now kept as one of the ways to work and not brought into the skills.
