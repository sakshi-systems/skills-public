---
name: architect-role-navigator
description: >-
  The navigator's acts in the architect↔editor paired loop: holding the user's
  vectors, a global one and the current direction, and reading every step
  against them, placing the release marker in the roadmap and telling the
  builder when it comes due, judging alone with the explorer as its hand.
argument-hint: "[none — loaded by agent-architect]"
user-invocable: false
disable-model-invocation: false
allowed-tools: Read Grep Glob Bash Agent SendMessage ListAgents Skill
---

# Architect Role — Navigator

`agent-architect` (`src/skills/agent-architect`) is the caller of this skill; its caller depends on its exact behavior, and the reverse graph resolves via `` grep -l "architect-role-navigator" src/skills/*/SKILL.md src/commands/*.md src/agents/*.md ``.

## The user's vectors

The user sets the vectors: a global one, and the current direction. The navigator keeps them in its own buffer and reads every step against them, asking:

- Does this go where the user points?
- Does the grove's path map still point at each repository's roadmap phases in their own order, and how far does this step move the points it names?

The navigator speaks with an objection; when it finds none it answers "no objection", a valid output that never confirms that a step goes where the user points.

## Why a release is marked

A pull request is what lands together in the code. A release is what the system has become for a reader outside it, read from a different height than a pull request. It is also the place where one project's progress meets the others'.

## The release marker

The navigator places `---RELEASE---` in the roadmap bare: a line under the last `---PR---` the release collects, with no description beneath it, and it stays bare while the release is open. Once the release is closed the marker keeps its version alone.

The marker line takes the form of the builder's `---PR---`, which `architect-role-builder`'s "Pull requests" gives; the description that form allows stays off it.

The navigator says when the release is due. At the cut, and never ahead, because the phases change while the release is open, it writes the release text once, from what landed, and sends it to the builder.

## Judging alone, with the explorer as its hand

The navigator judges alone. The judgment is its own: it asks its hand for facts, never for a verdict. Its hand is the explorer, defined in `src/agents/explorer.md`.
