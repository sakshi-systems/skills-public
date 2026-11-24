# A public surface the closed system releases into

The system develops closed and belongs to the user alone. What it runs on can be useful to others. This document states how the two meet: a public repository of its own for each opened product, which the closed system releases into and never merges with. It is the governing spec of that release process.

## The system develops closed

The system's memory, its roadmap, its heads and the team form one whole that writes itself. A buffer names the phase it works on, a snapshot names the peers it spoke with, a roadmap line quotes the user, and a rescue report carries the run it repaired. None of these parts can be cut into an open half and a closed half without breaking what holds it together, so none of them is open, in any repository: no head's buffer, snapshot, chronicle, roadmap, note, review or diagram opens. Even the roadmap is not public: it is the system's plan for itself, written in the words of its own work.

## What opens

What opens is what the system runs on, and only that: the skills, the orchestrator's engine, the testlab stand, the family root's entry point, and the long-running service that watches the family, once it exists ([a monthly analysis sprint, and the service that holds the environment](futures/a-monthly-analysis-sprint-and-the-service-that-holds-the-environment.md)). Each is a product another person can install and use without the memory that produced it.

The testlab stand opens whole, as a product: the parsing algorithm the stand produced, without its memory and never the channel's data. Only someone already in the channel can use the algorithm, so the data stays where it is.

The family root opens as its entry point: a README and the layout of its sibling repositories.

The role seeds open as they are, each its own repository in `sakshi-systems` (`razor`, `navigator`, `architect`, `builder`, `musafir`), public itself. A seed is the skills' distillate: one instruction file an agent works from as one role, with no memory behind it to keep closed, so a public copy beside it would only be a second home of the same text. A seed has no counterpart and no release cut, and its history is public with it.

## The public surface is a repository of its own

Each opened repository but the seeds has a public counterpart, a separate repository with a history of its own, and it holds only the product. The closed repositories keep their names; the public one is `<name>-public`, in the GitHub organization `sakshi-systems`, where the closed ones live as well: `skills-public`, `orchestrator-public`, `testlab-public`, and `sakshi-public` for the family root. The license is MIT.

A public repository is not a mirror of the closed one and does not share its commits. The first release of each is a snapshot of its product; every later release is the difference since the previous one. A release is one commit carrying the release's version, and it enters the public repository as a pull request from a `release/vX.Y.Z` branch into `main`. A release comes at the end of a [sprint](sakshi-harness/skill-cycle.md#спринт), not at every merge: the roadmap marks it ahead with a `---RELEASE---` marker, whose place [the roadmap's markers](philosophy/context-tree.md#две-карты-входа--время-и-пространство) state. The pull requests of the closed repository merge into its `dev` branch in roadmap order, and the head cuts the release from `dev` at the release point, the commit where the last pull request the marker collects merged; a pull request merged after that point is not in the release and waits for the release whose marker stands below it in the roadmap. At the same moment the release enters the public repository, the closed repository's `dev` goes into its main branch, up to the release point. The closed history never leaves: the public repository shows what each version holds, never how it came to hold it.

## How a release is cut

A release is cut when the navigator says it is due, and the text of its marker, which the navigator places, becomes the description of the public pull request. Every group the marker collects is already in the closed repository's `dev`, merged by rebase and fast-forward and never by a merge commit, so the history reads as what happened.

The release branch is brought to the product at the release point. The orchestrator's is taken by its include list: the code and its tests whole, and the listed docs whole, since a doc may run ahead of the code and trimming it for a release would contradict what it is; the public README is kept as it stands, and the version is set where the project carries it. The skills' is the difference since the last cut, applied onto a clone of the public repository's `main`. In either cut a link from the product to a document outside the public set becomes its plain text, and the paths another public repository links by absolute address keep their place across releases: `docs/architect-runtimes.md`, the two `architect-runtime-*` skills, and `src/skills/roadmap-engine/scripts/user-slug.sh`.

A product with a test suite runs it in the release tree, and the sweep below runs, and passes, before the branch is pushed. The release commit is authored and committed as the organization's public identity, `sakshi-systems` with its GitHub no-reply address, never as the user, and carries no attribution line. The branch is pushed and the pull request opens into `main`.

## Each release is swept

Each release is swept. The closed repository is the system's memory, and it names what it works with freely: other teams' projects, hosts, people. Its docs are not kept clean by a writing rule, and nothing relies on writers having kept names out. The cut is defined by what it includes, not by what it excludes: only the product that [What opens](#what-opens) names, with the docs chosen for publication. Buffers, roadmaps, notes, the chronicle and diagrams are never in it, so what they name does not matter.

What is checked is the release's difference, which is small; for the first release, a snapshot, it is the whole product against nothing. Three steps run on it, in order. A search of the difference covers what is always private and needs no list to keep: absolute home paths, email addresses and host addresses; a hit stops the release, naming the file and the line. An agent then reads the difference for what needs judgment, another team's project or a person named in the product; both run, and pass, before the branch is pushed. Then comes the user's word, given on the public pull request after an external reviewer installed on the public repositories has reviewed the difference: the head reads that review and brings it to the user, and the user's merge, or a merge the user has authorized, is the release. The external reviewer reads the difference without the task's intent, as an audit. While releases are cut by hand the search needs no dedicated script.

## Review and merge

The external reviewer, CodeRabbit, reviews the pull request, and the head answers every thread: it fixes the point, or says why it does not. Critical points are closed before the release, in two rounds at most, and what remains then goes to the user. A fix after the review is a commit of its own on the release branch, never an amend, so the pull request carries the release commit and the review's work after it. The same fix reaches the closed `dev` by a pull request of its own, and the closed release point is not rewritten. A clean pass posts no review and only edits the reviewer's summary comment, which is what a head waiting on it watches.

On the merge, the public `main` is fast-forwarded to the release branch's tip, never by a merge commit, and the release is tagged `vX.Y.Z` with an unsigned tag made by `sakshi-systems`. The closed repository's main branch is fast-forwarded to the release point, not to the tip of `dev`, and the release point is tagged `vX.Y.Z` there too, so the closed history shows which commit became which release.

## The roadmap closes last

The roadmap records the release last. Its `---RELEASE---` marker keeps the version, `vX.Y.Z`, alone, and the `---PR---` markers of the groups the release collected leave; the change is refused while a group whose marker would leave still holds an open task. It is committed into `dev` with the release's closure.

## Why the line runs here

A repository that is public carries in its history everything its memory ever held, and a head records what it reads and what it was told, so public memory carries other teams' internals. Another team moves its plan layer out of its repositories into a shared store, so that its code repository holds code alone. This system does not need to: it develops inside itself, and its team is the system, so its plan layer has no outside readers to be kept from. The line runs between the closed whole and the product it runs on, not through the middle of either.

## Who takes it up

Each repository's head cuts its own release: the skills' to the head of the skills, the engine's to the head of the orchestrator, the testlab stand's to the head of testlab, and the watching service's to the head that holds it. The family root has no heads of its own: the head of the skills cuts `skills-public` and keeps the root's `sakshi-public`, its entry README and the layout of the siblings.

The herald takes the process over later, on a local LLM. The process written here is what it automates.
