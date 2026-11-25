---
name: committing
description: >-
  How a head commits: what a commit takes, how a stretch of work becomes one,
  and when it is pushed. A head that commits loads it first.
user-invocable: false
disable-model-invocation: false
allowed-tools: Read
---

# Committing

This is an engine: its callers depend on its exact behavior — edits here must honor their expectations as part of its contract; the reverse graph resolves via `` grep -l "committing" src/skills/*/SKILL.md src/commands/*.md ``.

Whether a head may commit stays with `agent-architect`'s **Default** and **Advance coverage** under [The user rules the forks and owns the commits](../agent-architect/SKILL.md#the-user-rules-the-forks-and-owns-the-commits) and the global CLAUDE.md's [Git](../../global/CLAUDE.md#git). This engine holds how an authorized commit is made.

## What a commit takes

Every change in the repository the head works in, every head's memory there included, and, where the project's `.ai-factory` is a link into another repository, the folder it points to, committed in that repository. Nothing in another project's repository.

## One commit is a stretch of work

Changes are gathered and amended into one unpublished commit until the stretch has come together, never a commit per step or per round; this holds for every commit a head makes, its memory included. The amended hash is the receipt a peer checks. Separate work is a separate commit. While amending, gather the meaning briefly in the message without overloading it.

## When it is pushed

A pushed commit can no longer be amended, so a stretch is pushed when someone outside the working tree needs it: before a run, before a pull request. Do not rewrite a published commit.

## Before a run

Every place containing the run's work is clean before launch: the product repository and the repository holding the project's artifacts through a symlink alike. The orchestrator's whole-tree staging can silently sweep leftovers into a task commit; checking only the product repository misses separately held work.

## During a run

The queue owns its task commits and pushes; the head makes no commit or amend in the queued tree, so its Git operations do not compete with the queue. The head's own writes, memory included, remain free: the intended flow lets the orchestrator collect those writes.

## Closure

Commit the resulting preparation and closure work before another launch.
