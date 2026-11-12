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

The user sets the vectors: a global one, and the current direction. The navigator keeps them in its own buffer and reads every step against them, asking one question:

- Does this go where the user points?

## Why a release is marked

A pull request is what lands together in the code. A release is what the system has become for a reader outside it, read from a different height than a pull request. It is also the place where one project's progress meets the others'.

## The release marker

The navigator places `---RELEASE---` in the roadmap: a line under the last `---PR---` the release collects, with a description of what the reader outside the system gets. At the cut the record gains the release's version, where it went out as the project's instructions name it, and when.

The marker takes the form of the builder's `---PR---`, which `architect-role-builder`'s "Pull requests" gives.

Only the builder writes, so the builder's hand writes the marker line. The navigator tells the builder when its marker comes due, and the builder executes the cut by the project's instructions.

## Judging alone, with the explorer as its hand

The navigator judges alone. The judgment is its own: it asks its hand for facts, never for a verdict. Its hand is the explorer, defined in `src/agents/explorer.md`.
