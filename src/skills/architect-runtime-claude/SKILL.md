---
name: architect-runtime-claude
description: >-
  Claude Code native mechanics for the architect↔editor paired loop: how a head
  reads its own session id and name, the locator it writes, how skills and
  resources load, how the persistent hand is created and messaged, and how
  a Claude head is reached by a peer.
argument-hint: "[none — loaded by agent-architect or a hand]"
user-invocable: false
disable-model-invocation: false
allowed-tools: Read Bash Agent SendMessage ListAgents Skill AskUserQuestion
---

# Architect Runtime — Claude Code

Load this skill after `architect-editor-engine` is resident, when your known harness is Claude Code — the common architect (`src/skills/agent-architect`) and a hand's definition (`src/agents/<name>.md` — the editor or the explorer) are its callers, and the engine's § "Resolving installed resources" is the rule that loads it. This is an engine; its callers depend on its exact behavior, and the reverse graph resolves via `` grep -l "architect-runtime-claude" src/skills/*/SKILL.md src/commands/*.md src/agents/*.md ``. A caller in another harness never loads it.

## Own identity

You read your own session id and session name; the read is the head's own, and a hand's id never substitutes for it.

**Session id.** Two commands. The first prints a fresh random nonce and returns — its output reaches the transcript only when the command returns. The next searches the project's transcripts — the `.jsonl` files directly under `~/.claude/projects/<project-key>/`, `<project-key>` being the working directory's path with every character that is not a letter or a digit replaced by a hyphen — for the printed nonce. Exactly one file matches, and its name without `.jsonl` is the session id. Any other count is no unique result.

**Session name.** The first line `ListAgents` returns opens `This session is <name> [<ref>] —`. Keep the bare name: the token after `This session is`, up to any bracketed ref that follows it. `SendMessage` takes the bare name as the address, and a ref that was not just read from a listing does not resolve.

When `ListAgents` or the shell is not available, that is an unavailable operation, not a missing result: report which one, as the common rule in `agent-architect` has it.

## Locator

Publish only your own folder's `address.json` under `architect-editor-engine`'s “Locator selection and transition” contract. The id decides the write. With no uniquely observed id, report the identity observation and preserve the known locator, or leave it unwritten for a new head. With a known id, build a fresh object from your native observations: `{"session-id": <observed id>, "runtime": "claude"}`, adding `"session-name": <current bare name>` when available. If the name cannot be read — `ListAgents` is unavailable, or its first line cannot be parsed — report the missing name and omit it. Current name and mode are independent observations: a missing name does not suppress a recognized mode, and a missing mode does not suppress the name or publication. Do not merge old locator fields or retain an old name: own recovery keeps matching the id while peer delivery diagnoses the missing destination.

Only at startup and rehydration, read mode from your own transcript already uniquely identified in “Own identity”. Select the latest entry in transcript order whose `type` is `"user"` and whose top-level `permissionMode` key is present, then validate that selected value. Entries without the key do not participate; key presence does not depend on the value's truthiness or validity. Recognize it by the exact-string rule and canonical values in “How a head of this runtime is reached”; add `"permission-mode": <observed mode>` to the fresh object only when recognized. If no qualifying observation-bearing user entry exists, report an absent observation and omit the key. If the transcript cannot be read or parsed reliably, report an unreadable observation and omit the key. Report a selected null, empty, non-string or unsupported value and omit the key as well. Never replace an invalid selected observation with an older valid one or copy a mode from the old locator. Display labels, `ListAgents` output, sender configuration and another head's records do not supply this observation. Add no per-send observation, polling or mid-session refresh; freshness after a mid-session mode switch remains unverified.

Serialize the complete fresh object with a JSON serializer, such as Python's `json.dump`, into a uniquely named temporary file in your own folder, such as `tempfile.NamedTemporaryFile(mode="w", encoding="utf-8", dir=<own folder>, delete=False)`. Close the completed file before atomically replacing `address.json` with it using `os.replace`. Only after that replacement succeeds, remove that same folder's legacy `address.md` if present. If preparation, serialization or replacement fails, report it, clean up only your temporary file, and preserve the previous published locator and legacy file. If legacy removal fails, report that failure with the valid JSON remaining authoritative. No in-place truncation of the published locator is part of this procedure.

## Loading skills and resources

The native `Skill` tool loads a skill by name; an agent definition is raised through `Agent`. A skill the native manifest does not list, or that the `Skill` tool refuses, falls back as `architect-editor-engine` § "Resolving installed resources" states, including its exception for `disable-model-invocation` — never from an assumed `~/.claude` path. The question facility for a genuinely marginal fork is `AskUserQuestion`.

## The persistent hand

**First creation.** Raise the hand with `Agent`, with `subagent_type` set to the hand's definition name — derived from the canonical definition path `agents/<name>.md` the common caller gives, the file stem without `.md`, which the caller does not pass as a separate thing — or `editor` where the caller gives none, and the first channel-message as the prompt, its format token literally opening it. After the channel-message, as spawn metadata outside the payload, come the buffer's path and that hand's canonical definition path — both supplied by the common caller, both pointers only. Record the returned handle in the buffer. Where the running build exposes `Agent`'s `name:` parameter, pass it too, as an addressing convenience layered on the recorded handle, never the carrier and never a required step: the parameter is absent from some builds and nothing contracts the name's behavior beyond the run.

**Every later round** goes to the same handle via `SendMessage`.

**Liveness.** The next `SendMessage` is itself the probe, and its outcome is the signal. `ListAgents` is never that signal: its subagent rows come from the live task registry of what is actively running right now, a completed round drops out of it on the order of a minute, and a compact never touches the hand at all — it only erases the address from your own context — so absence from that listing is never evidence of death. Recover no handle from a listing, for the same reason.

A failed send is a dead hand. The replacement is a fresh `Agent` creation as above; when and why to replace stays with the common caller.

## How a head of this runtime is reached

A sender reaching a Claude head reads this section alone.

Read `permission-mode` from the recipient's freshly selected public locator. The recognized canonical strings are `acceptEdits`, `auto`, `bypassPermissions`, `manual`, `dontAsk`, `plan`; require an exact string match, without aliases, case conversion or coercion. An absent key leaves the one-shot courier's configured default in effect. A present null, empty, non-string or unsupported value is a locator fault: diagnose it with the repository, owner, folder, faulty file and field instead of silently defaulting or dispatching with it. This consumes only the public locator, without reading the recipient's transcript or an extra `ListAgents` lookup.

Use the native `SendMessage` to the recipient's fresh `session-name` where the sender's harness exposes it; this remains the native call with no mode argument. Otherwise dispatch one-shot with `claude -p <prompt> --tools SendMessage --allowedTools SendMessage --model haiku --output-format stream-json --verbose --strict-mcp-config`, supplying no `--mcp-config`, so only `SendMessage` is exposed and no MCP server is loaded. Pass the prompt as a single argv element without shell interpolation, asking for exactly one `SendMessage` call with the given `to` set to the recipient's fresh `session-name` and a `message` containing the given text exactly, nothing added, removed or reworded, and nothing else; a letter long enough to tempt rewording travels as its file path, written before the courier runs, so the courier copies one short line, as `agent-architect` § "Working with another architect" states. The courier is a model that can re-compose a letter, and a successful enqueue does not reveal paraphrasing. When `permission-mode` is present and recognized, append `--permission-mode` followed by its exact value as one uninterpolated argv element; when absent, add no mode override. This configures the courier process, not the recipient's session.

A dispatch that is held, refused or unavailable is surfaced, never rerouted. A successful tool result is evidence of enqueue, not of the receiver's read or reply. No live validation of the unknown-mode behavior is claimed.
