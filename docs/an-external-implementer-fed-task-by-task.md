# An external implementer fed task by task — the other way a roadmap is executed

A roadmap is executed in one of two ways. The [orchestrator](sakshi-harness/skill-cycle.md#исполнение--orchestrator) runs it with its own protocol: a sidecar per task, plans, reviews and PASS signals. Or an implementer that lives outside the family — an agent on another host, with sessions of its own, reached through an API — is fed its tasks one at a time, and it speaks none of that protocol. The architect pair, a head and its hand, then works with it directly. Both ways share the roadmap, its tasks and their specs, and the closing of a phase; what differs is who builds and who verifies. This document describes the second way.

## What differs from the orchestrator

The orchestrator is roles that verify one another: a planner, a plan reviewer, an implementer, a code reviewer, with PASS signals and a sidecar for every task. The external implementer is one agent that plans and builds a task alone and answers with a report. Nothing around it verifies it. All verification is the pair's, and it rests on one distinction: the implementer's report is a claim, and the pushed branch and its diff are the evidence. Completion is judged by the branch, the diff, the head's own test run and the head's own probes, never by the report.

The implementer's sessions are the other difference. Each API call is a session of its own, and the sessions know nothing of one another beyond whatever notes the implementer keeps for itself. The user named this the weakness of such an implementer and, in the same breath, perhaps its strength: no message can lean on a previous one, so each carries what it needs.

Fed from inside the architect's own session, one task at a time, nothing separates what is planned from what lands: the task is ordered and its code arrives in the session that ordered it, and each landing is read against the plan as it stands.

## Every phase is decomposed

Every phase is decomposed into tasks, exactly as for the orchestrator, and a simple phase is one task. The user's reason: «Меньше условий а с размером фазы - предсказать не возможно» — fewer conditions, and a phase's size cannot be predicted. Each task has its [task spec](what-a-task-carries.md#what-a-spec-holds): what is true now, read from the code with exact values; what must be true after; what breaks on contact; and the user's own words where they bear on it. Its "now" is the code as the tasks above it leave it, so it is re-read against the main branch each time a task lands. A task line may name its owner — the implementer, the owner or the pair — and lines the implementer does not hold wait below the stop.

## The stop sits directly under the task in work

The roadmap's stop line sits directly under the task the implementer holds. The user's words for it: the stop goes under what was handed over, and when the implementer finishes, the head checks and sends it fixes until the quality is satisfactory. Everything above the line is in the implementer's hands or landed; everything below it waits. When the task lands, the stop moves down by one line. No line owned by the owner or the pair stands above it: a runner that reads the roadmap would take such a line for work to execute. In this way of working the stop marks what the implementer holds, which is why it stands beside the [markers paragraph](philosophy/context-tree.md#две-карты-входа--время-и-пространство) without contradicting it.

## The message

A task goes out in one message. A short header in English comes first:

- who sends it, and that the sender is an architect whose word carries the user's where the user has ruled so;
- that this is one task: the implementer plans it itself and builds it whole;
- the branch to push, named for the phase and cut from the current main branch;
- that it neither merges nor deploys;
- that it keeps the governing spec current in the same branch, without renumbering what exists, and puts no plan reference into code;
- which tests it runs, sized to the task;
- what it reports back: per section of the spec's "after", and what it could not check.

The task's roadmap line and its spec follow, verbatim. A house rule stated once in the header holds; restated per message it drifts.

The same shape serves the questions the head sends between tasks, with one more rule: the question is the narrowest one that decides the next step. Several questions asked at once to choose a single mechanism cost the implementer a long stretch of its time, and a question that sends it measuring more than the decision needs is a question too wide. A measurement the implementer is asked to make is passive reads and probes in a scratch directory, never an operation on a live thing: refreshing a copy of a live credential can rotate its token and sign the original out.

## The review loop

When the branch is pushed, two independent readings start. The head reviews the diff, runs the tests in a throwaway checkout, and probes the risky paths itself: crafted inputs, an isolated home directory, the rendering a user would see. Its hand reads the same diff as a `REPORT-ONLY` round, given the target and no findings, so the two readings are produced independently, as [the split of the paired loop](paired-loop.md#where-the-split-falls) asks of a judgment. The two are then reconciled. The independent reading finds defects the head has missed; the head's probes find what only a run shows, such as a value a server answers with being written unescaped into a file a shell reads.

Every finding then goes one of two ways, and the question that tells them apart is whether the implementer did what the spec said.

- **It did not: the finding is in the code alone.** The repair is a fix round: a new line directly under the task, `N.Ma` (then `N.Mb`), a contract line like any other with a spec of its own, its "now" the code as the landed commit left it. The stop moves under it, and it goes to the implementer as a task, on the same branch. The task it repairs stays landed.
- **It did, and the result is still wrong: the task itself is wrong.** The spec said something false of the code, missed a place the change reaches, or asked for the wrong thing. That task is rescued first, by [`task-rescue`](../src/skills/task-rescue/SKILL.md): a report that tells what the spec said, what landed and what the finding showed, and the spec repaired where it was wrong. For an implementer with no sidecar the rescue's rollback is the fix round itself, the same `N.Ma` line.

A fix round is always a line of the roadmap. A repair that lives only in a message to the implementer is a repair the roadmap does not know, and the head ends up holding it in mind.

A review also catches what the head itself pinned: a destination a spec names for a setting can be a place the target tool does not read, and then the change does nothing. What a spec pins as a destination is checked against what the target tool actually reads.

## The architect who ordered the work reviews and repairs it

In place of the orchestrator's review cycle, the architect who ordered the work reviews the landing, because everything is at hand: the plan it wrote, the diff, the tests and the evidence the branch gives. This holds only while the review is two hands plus real runs: the head's own pass, with real probes on the real system and a real click where a client is touched, and its hand's independent pass, as [the review loop](#the-review-loop) describes. The defects surface in the hand's independent pass or the head's real probes, never in the implementer's report; with one hand the review is no review. The repair goes back to the implementer as a fix round, and the head never fixes the branch itself.

## Landing, and a separate call to deploy

When the quality holds, the head lands the branch on the main branch. Where a project lands by fast-forward without a pull request, that is the user's ruling; a project that merges through pull requests keeps the user's merge. The phase's pull request lists its fix rounds beside its tasks, and an external review's remarks on it go the same two ways as a finding: the code alone to a fix round, a task shown wrong to a rescue and then a fix round, and a remark that blocks the merge repaired before it. Deployment is a separate call to the implementer, which asks for evidence: the deployed commit, the health of the service and a probe of the behaviour the task changed. Where the network allows, the head runs one probe itself, such as checking that a removed route now answers as absent.

## Docs between landings

While no implementer branch is open, the pair writes the behavioural governing spec on the main branch and commits it, and the next task's spec points at the spec's rule ids instead of restating them. The direction of change holds as ever, docs then roadmap then code: the pair writes the docs, the implementer writes the code and only the spec lines its code forces. A spec's sections and rule ids are never renumbered; a new one takes the next free number. The project's README describes what runs, and changes with the code.

The user's corrections bound what the pair writes in the product repository. The tree is clean before the implementer is loaded: a draft left uncommitted there collides with its work, so it goes back to the head's own folder as a diff. Beyond docs, writing into the project is the implementer's job, and the pair does not spend it.

## A second hand beside the editor

The implementer is a second hand of the architect, beside its editor, and the two hands divide the work by what they write: the editor writes the docs and the planning artifacts, the implementer writes the code. The head arranges the pipeline that suits that pair. The split is the one [docs between landings](#docs-between-landings) already draws, seen from the head's side. On a large project the razor and the navigator join the pair as they join any cycle, reading the same steps at their own heights.

## The channel

The implementer is reached by one synchronous call per message, run in the background. The real signal is the pushed branch, which the head watches by asking the remote for the branch's head. A call cut on the head's side does not stop the implementer's work. Two calls never run on one branch at once, and a second task is never sent while one is open; the next item waits in the head's own ledger. A credential that opens the implementer's host never leaves that host.

An implementer reached this way needs nothing of the family but a message, and what it receives is a spec that stands alone. That made the door into Sakshi more visible: an outside agent enters the work through a plain API and a self-sufficient spec, without learning the family's artifacts.
