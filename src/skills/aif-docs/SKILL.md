---
name: aif-docs
description: Write or change the project's governing spec — the docs step of the cycle — as a present-tense account of behavior, protocols, data flows, and connections, split by topic in the project's docs directory. Use when user says "create docs", "write documentation", "update docs", "document project", "напиши ТЗ", or "техническое задание".
allowed-tools: Read Write Edit Glob Grep
disable-model-invocation: false
---

# Documentation Generator

Write and maintain the docs directory as the project's **governing spec**: present-tense behavior, invariants, protocols, data flows, and connections, whether the code behind it exists yet or not. README and its onboarding relatives (CHANGELOG.md, CONTRIBUTING.md, LICENSE) are exempt from that genre.

This skill stands on the global CLAUDE.md § "Documentation style" and § "Grounding claims" for how a doc is worded, linked and kept to one home per fact, and restates none of it.

## What a doc is about

The subject is behavior, invariants, protocols, data flows, and connections. The target behavior comes from the stated user intent, the task spec, or the ROADMAP phase where one is discoverable, and is written in present tense as if it already ships. The code is the baseline the target changes from, and the referent where nothing changes it.

## Where it is written

- **Docs directory:** `docs/` by default, or the project's existing docs directory where one is in use. Details are written there.
- **One topic, one page.** Each doc is self-contained: a reader of one file gets the whole of its topic.

## The CLAUDE.md Documentation Index

Create or update the `## Documentation` section in the project's CLAUDE.md — the single home for the documentation index:

```markdown
## Documentation

| Doc | What it covers |
|-----|-----------------|
| [Getting Started](<repo-root-to-docs-dir>/getting-started.md) | Installation, setup, first steps |
| [Architecture](<repo-root-to-docs-dir>/architecture.md) | Project structure and patterns |
```

- One row per doc page in the docs directory — README is **not** listed
- Descriptions under ~12 words
- Rows follow the docs directory's logical reading order (getting started → workflow → details)
- If CLAUDE.md doesn't exist, create it with only this section plus a one-line header

## What is written

- **Never delete content** without moving it to its home.
- **Project facts are read from the project** — never invented.

## Ownership

This skill owns the docs directory and the `## Documentation` section of the project's CLAUDE.md, scoped to that section only. `.ai-factory/ARCHITECTURE.md` and the roadmap are read-only context.
