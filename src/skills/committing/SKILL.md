---
name: committing
description: >-
  How a head commits when it commits: the scope is the project, including every
  change of that project in each owning repository. Before a run, every place
  the run's work lives in is clean, the product repository and the repository
  holding the project's artifacts through a symlink alike. Through preparation,
  from outline to decomposition, the head amends its own unpublished preparation
  commit or makes a separate one when the change is large, is documentation, or
  a separate commit makes sense.
user-invocable: false
disable-model-invocation: false
allowed-tools: Read
---

# Committing

This is an engine: its callers depend on its exact behavior — edits here
must honor their expectations as part of its contract; the reverse graph resolves
via `` grep -l "committing" src/skills/*/SKILL.md src/commands/*.md ``.

Whether a head may commit stays with `agent-architect`'s **Default** and
**Advance coverage** under [The user rules the forks and owns the commits](../agent-architect/SKILL.md#the-user-rules-the-forks-and-owns-the-commits)
and the global CLAUDE.md's [Git](../../global/CLAUDE.md#git). This engine holds how
an authorized commit is made.

## Scope by project

Take every change of the project in each owning repository, including other
heads' memory of that same project: buffers, locators, snapshots and chronicle
lines. In a repository holding
several projects' artifacts, take only this project's folder. The user's boundary
is the project: shared ownership includes the project's changes, without
extending to other projects or the whole shared root.

## Owning repository

Commit each change in the Git repository that owns its files, including artifacts
reached through a symlink. The product and its artifacts can have separate
histories; Git in the product repository does not record changes beyond the
symlink, so a product commit alone can leave the artifacts unrecorded.

## Preparation

From outline through decomposition, avoid a commit per step. Amend only the
head's own unpublished preparation commit, or make a separate commit when the
change is large, is documentation, or a separate commit makes sense. This avoids
meaningless preparation commits while preserving meaningful separate changes.
While amending, gather the meaning briefly in the message without overloading it.
Do not rewrite a published commit, because published history is already shared with
its consumers.

## Before a run

Every place containing the run's work is clean before launch: the product
repository and the repository holding the project's artifacts through a symlink
alike. The orchestrator's whole-tree staging can silently sweep leftovers into a
task commit; checking only the product repository misses separately held work.

## During a run

The queue owns its task commits and pushes; the head makes no commit or amend in
the queued tree, so its Git operations do not compete with the queue. The head's
own writes, memory included, remain free: the intended flow lets the orchestrator
collect those writes.

## Closure

Commit the resulting preparation and closure work before another launch,
including the run's generated task marks, plans, reviews and sidecars where they
are versioned separately. The product-repository task commit leaves separately
versioned run records outside its history; closure records them in their owning
repository before the next run begins.
