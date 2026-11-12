---
name: architect-runtime-codex
description: >-
  Codex native mechanics for the architect↔editor paired loop: how a head reads
  its own thread id, the locator it writes, how skills and resources load, how the
  persistent hand is spawned and followed up, and how a Codex head is
  reached by a peer.
argument-hint: "[none — loaded by agent-architect or a hand]"
user-invocable: false
disable-model-invocation: false
allowed-tools: Read Bash
---

# Architect Runtime — Codex

Load this skill after `architect-editor-engine` is resident, when your known harness is Codex — the common architect (`src/skills/agent-architect`) and a hand's definition (`src/agents/<name>.md` — the editor or the explorer) are its callers, and the engine's § "Resolving installed resources" is the rule that loads it. This is an engine; its callers depend on its exact behavior, and the reverse graph resolves via `` grep -l "architect-runtime-codex" src/skills/*/SKILL.md src/commands/*.md src/agents/*.md ``. A caller in another harness never loads it.

## Own identity

Read `CODEX_THREAD_ID` from your own execution environment, for example `printenv CODEX_THREAD_ID`. One non-empty value is the session id. Unset or empty is no unique result. A shell that cannot run is an unavailable operation, not a missing result: report it as such, as the common rule in `agent-architect` has it. The read is the head's own; a child hand's `CODEX_THREAD_ID` never substitutes for the parent's.

## Locator

Publish only your own folder's `address.json` under `architect-editor-engine`'s “Locator selection and transition” contract. Build a fresh object from your own nonempty observed `CODEX_THREAD_ID`: `{"session-id": <CODEX_THREAD_ID>, "runtime": "codex"}`. There is no Claude destination field. Do not merge old fields. With no uniquely observed id, report the identity observation and preserve the known locator, or leave it unwritten for a new head.

Serialize the complete fresh object with a JSON serializer, such as Python's `json.dump`, into a uniquely named temporary file in your own folder, such as `tempfile.NamedTemporaryFile(mode="w", encoding="utf-8", dir=<own folder>, delete=False)`. Close the completed file before atomically replacing `address.json` with it using `os.replace`. Only after that replacement succeeds, remove that same folder's legacy `address.md` if present. If preparation, serialization or replacement fails, report it, clean up only your temporary file, and preserve the previous published locator and legacy file. If legacy removal fails, report that failure with the valid JSON remaining authoritative. No in-place truncation of the published locator is part of this procedure.

## Loading skills and resources

Use native skill invocation where the harness exposes it; otherwise fall back as `architect-editor-engine` § "Resolving installed resources" states. The hand's definition is raised by having the child read, as its operating definition, the canonical definition the common caller gives, or `src/agents/editor.md` where the caller gives none, through the pinned spawn below. The question facility for a genuinely marginal fork is to ask the user directly in the reply.

## The persistent hand

**First creation.** Use `collaboration.spawn_agent`, always with `fork_turns: "none"` — the schema default is `all`. Pass `model: "gpt-6.1-sol"` and `reasoning_effort: "low"` explicitly, because without them the hand inherits its head's model and effort. This holds for whichever hand is raised — the editor or the explorer — on the first spawn and on every replacement alike: the hand is born from the definition, the buffer and the channel-message alone, never from the head's history, because inheriting it would give the hand the head's own reading and turn every relay's agreement into an echo.

Pass the required `task_name` (lowercase letters, digits and underscores; a fresh one per hand: the hand's definition name — the `<name>` of the canonical definition path `agents/<name>.md` the common caller gives, the file stem without `.md` — then that name with `_2` on replacement; where the caller gives no other hand, that is `editor`, then `editor_2`) and `message`, laid out exactly so: the text opens literally with the first channel-message, its format token (`REPORT-ONLY` or `APPLY-EDIT`) as the very first characters. Only after the channel-message come, as spawn metadata outside the payload, the instruction to read and follow the canonical definition path of the hand the common caller supplies as the operating definition before acting on the message, and the buffer's path — both pointers, no reading. Nothing precedes the token: the hand keys its mode off the token that literally opens the message and falls back to `REPORT-ONLY` otherwise, which would silently downgrade a first `APPLY-EDIT`.

Record in the buffer the agent's canonical task name as spawn reports it — the value `collaboration.followup_task` accepts as `target`; where spawn reports an agent id instead, that serves equally. That is the handle. The hand's behavioral body is not duplicated here.

**Every later round** goes through `collaboration.followup_task` with `target` set to that handle and `message` the channel-message.

**Liveness.** The next follow-up is itself the probe. A failure is a dead hand, replaced by a fresh `collaboration.spawn_agent` as above; when and why to replace stays with the common caller.

## How a head of this runtime is reached

A sender reaching a Codex head reads this section alone.

Dispatch with the native argv `codex queue --thread <session-id> --message <text>`, without `--resume`, using the recipient's fresh `session-id` and the text as one uninterpolated argument. A letter too long for one argument travels as its path, as `agent-architect` § "Working with another architect" states.

A successful queue is evidence of enqueue, not of the receiver's read or reply. An unavailable or failed dispatch is reported, never rerouted.
