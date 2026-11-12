# A public surface the closed system releases into

The system develops closed and belongs to the user alone. What it runs on can be useful to others.
This document states how the two meet: a public repository of its own for each opened product,
which the closed system releases into and never merges with. It is the governing spec of that
release process.

## The system develops closed

The system's memory, its roadmap, its heads and the team form one whole that writes itself. A
buffer names the phase it works on, a snapshot names the peers it spoke with, a roadmap line
quotes the user, and a rescue report carries the run it repaired. None of these parts can be cut
into an open half and a closed half without breaking what holds it together, so none of them is
open, in any repository: no head's buffer, snapshot, chronicle, roadmap, note, review or diagram
opens. Even the roadmap is not public: it is the system's plan for itself, written in the words of
its own work.

## What opens

What opens is what the system runs on, and only that: the skills, the orchestrator's engine, the
testlab stand, the family root's entry point, and the long-running service that watches the family,
once it exists
([a monthly analysis sprint, and the service that holds the environment](futures/a-monthly-analysis-sprint-and-the-service-that-holds-the-environment.md)).
Each is a product another person can install and use without the memory that produced it.

The testlab stand opens whole, as a product: the parsing algorithm the stand produced, without its
memory and never the channel's data. Only someone already in the channel can use the algorithm, so
the data stays where it is.

The family root opens as its entry point: a README and the layout of its sibling repositories.

## The public surface is a repository of its own

Each opened repository has a public counterpart, a separate repository with a history of its own,
and it holds only the product. The closed repositories keep their names; the public one is
`<name>-public`, in the GitHub organization `sakshi-systems`, where the closed ones live as well:
`skills-public`, `orchestrator-public`, `testlab-public`, and `sakshi-public` for the family root.
The license is MIT.

A public repository is not a mirror of the closed one and does not share its commits. The first
release of each is a snapshot of its product; every later release is the difference since the
previous one. A release is one commit carrying the release's version, and it enters the public
repository as a pull request from a `release/vX.Y.Z` branch into `main`. A release comes at the end
of a [sprint](sakshi-harness/skill-cycle.md#спринт), not at every merge: the roadmap marks it
ahead with a `---RELEASE---` marker, whose place
[the roadmap's markers](philosophy/context-tree.md#две-карты-входа--время-и-пространство) state. The
pull requests of the closed repository merge into its `dev` branch, and the head cuts the release
from that merged `dev` once the pull requests the marker collects are in it, the release point. At
the same moment the release enters the public repository, the closed repository's `dev` goes into
its main branch. The closed history never leaves: the public repository shows what each version holds, never how it
came to hold it.

## Each release is swept

Each release is swept. The closed repository is the system's memory, and it names what it works with
freely: other teams' projects, hosts, people. Its docs are not kept clean by a writing rule, and
nothing relies on writers having kept names out. The cut is defined by what it includes, not by what
it excludes: only the product that [What opens](#what-opens) names, with the docs chosen for
publication. Buffers, roadmaps, notes, the chronicle and diagrams are never in it, so what they
name does not matter.

What is checked is the release's difference, which is small; for the first release, a snapshot, it
is the whole product against nothing. Three steps run on it, in order. A search of the difference
covers what is always private and needs no list to keep: absolute home paths, email addresses and
host addresses; a hit stops the release, naming the file and the line. An agent then reads the
difference for what needs judgment, another team's project or a person named in the product. Then
comes the user's word, given on the public pull request after an external reviewer installed on the
public repositories has reviewed the difference: the head reads that review and brings it to the
user, and the user's merge is the release. The external reviewer reads the difference without the
task's intent, as an audit. While releases are cut by hand the search needs no dedicated script.

## Why the line runs here

A repository that is public carries in its history everything its memory ever held, and a head
records what it reads and what it was told, so public memory carries other teams' internals. Another
team moves its plan layer out of its repositories into a shared store, so that its code repository
holds code alone. This system does not need to: it develops inside itself, and its team is the
system, so its plan layer has no outside readers to be kept from. The line runs between the closed
whole and the product it runs on, not through the middle of either.

## Who takes it up

Each repository's head cuts its own release: the skills' to the head of the skills, the engine's to
the head of the orchestrator, the testlab stand's to the head of testlab, and the watching
service's to the head that holds it. The family root has no heads of its own: the head of the
skills cuts `skills-public` and keeps the root's `sakshi-public`, its entry README and the layout of
the siblings.

The herald takes the process over later, on a local LLM. The process written here is what it
automates.
