---
name: architect-editor-engine
description: >-
  Shared contract for the architect↔editor paired loop, loaded at birth by
  the architect (`src/skills/agent-architect`) and a hand's definition
  (`src/agents/<name>.md`). Holds the two channel-message formats — REPORT-ONLY
  and APPLY-EDIT — with the rule that a receiver keys its mode strictly off the
  token that literally opens each message, and the definition of the architect's
  folder the pair shares — its buffer, its snapshots and its address file, with
  their path and numbering — the rule that the head alone writes the buffer and
  the hand reads it in full and writes only `editor/`, the hand's re-read of
  the memory on change, and the drain rule — a project or skill ruling leaves the
  buffer once it reaches the artifact that should hold it, the pair's base
  behaviour goes to the seed and stays as a standing entry, and what was said
  or failed twice is a debt to drain — and the rule that no architect reads
  another's buffer and a hand reads only its own architect's. When-to-use
  policy stays with the caller.
user-invocable: false
disable-model-invocation: false
allowed-tools: Read
---

# Architect-Editor Engine — the Paired-Loop Contract: Channel-Messages and the Shared Buffer

Load this skill at birth — the architect per the instruction in its own body, the hand, as the first action of its definition on spawn — so the contract is resident before any channel-message arrives and the buffer's definition below is held by both halves from birth. The runtime skills (`architect-runtime-claude`, `architect-runtime-codex`) are loaded after this engine, through § "Resolving installed resources". This is an engine; its callers depend on its exact behavior, and the reverse graph resolves via `` grep -l "architect-editor-engine" src/skills/*/SKILL.md src/commands/*.md src/agents/*.md ``.

## The two channel-message formats

Every channel-message the architect sends opens literally with its format token — `REPORT-ONLY` or `APPLY-EDIT` — held byte-exact, like `PLAN_REVIEW_PASS`, never paraphrased.

- **`REPORT-ONLY`** — an analysis/report request: the receiver reads or runs the target, reasons independently, reports by fact, writes no files.
- **`APPLY-EDIT`** — a pinned apply work-order: the receiver makes exactly the edits specified, self-verifies, and does not commit.

## The mode rule

A receiver keys its mode strictly off the token that opens the message — never off content, never off a referenced skill's own default. Ambiguity resolves to `REPORT-ONLY`. That fallback never reaches what keeps the hand current: as § "The architect's buffer" states, what rides alongside a channel-message opens no message of its own, so there is no opening for this rule to classify — it travels inside a message that already carries its own token. The full authoring and execution mechanics of each format stay with the callers; this engine holds the two formats, the rule for telling them apart, and the buffer's definition below — when-to-use policy stays with the caller.

## The architect's buffer

The pair shares one working memory, and it lives in the architect's own folder, `.ai-factory/architects/<user-slug>/<NN>/`, inside its user's own folder: `<user-slug>` is the user's slug, the one the session holds in the line `The user's slug: <user-slug>` or otherwise takes from running `roadmap-engine`'s `scripts/user-slug.sh`, and the folder's number, counted within that user's folder, is the head's identity there. The folder holds `buffer.md`, the buffer itself; `address.json`, the architect's current locator; the head's own snapshots, each a `<NN>-<slug>.md` file with a lowercase-hyphenated slug, numbered inside the folder, the highest number being the latest; and `editor/`, the snapshots of every hand, whatever definition it runs from, named and numbered the same way and on their own. Each hand snapshot opens with the line ``Writer: `<handle>`, relieved <date>.``; `editor/` is the history of hands, and nothing else lists them — neither the locator nor the buffer.

**Locator fields.** `address.json` is one JSON object, parsed by field name: `session-id` and `runtime` are required nonempty strings, and the supported runtimes are `claude` and `codex`. Claude's `session-name`, when supplied, is a nonempty string containing the current bare native name. The id is that of the session the head runs in, which holds across a compact and a reopened chat; the name is the one by which a peer reaches a Claude session, which changes when the chat is reopened. Own recovery needs the exact verified native id; an absent Claude name does not prevent recovery, but delivery to Claude requires that destination and diagnoses its absence. The selected runtime skill owns the exact native read and refresh; this engine owns the meaning.

Claude's optional `permission-mode` is the latest observed native permission mode supplied by the selected runtime. Its absence does not prevent exact-id recovery and leaves the one-shot courier's configured default in effect; a present malformed or unsupported value is a locator fault under the contextual diagnostics below. Recognition and consumption live in [architect-runtime-claude](../architect-runtime-claude/SKILL.md), under “How a head of this runtime is reached”.

**Locator selection and transition.** Own recovery and peer lookup select `address.json` whenever it is present, reading the existing legacy text `address.md` only when JSON is absent. Legacy fields are read by name, never by line order, with the same field meanings and diagnostics. A present locator that cannot be read, invalid JSON, a non-object JSON value, missing required data, malformed field values or an unsupported runtime is diagnosed; defective JSON never permits a legacy fallback. An absent `runtime` is incomplete in either form and is never inferred from an id, name or the reader's harness. Diagnostics name the repository, owner, folder and faulty file or field, distinguishing missing required data, malformed or unsupported present data, read failures and unavailable native operations. No fault licenses guessed identity, peer-buffer reads or a sender's repair of a peer locator. A peer reads the selected public locator and never the buffer.

Only the owner publishes newly observed fields as one atomic JSON replacement, then removes only that folder's legacy `address.md` after publication succeeds. A failed preparation or replacement preserves the published locator and legacy file; a failed legacy removal is reported, with the published JSON authoritative. Failed native identity observation preserves a known locator and leaves unavailable identity unwritten at founding. A known Claude id with an unavailable current name is published with its runtime and without retaining an old name. The selected runtime's “Locator” section supplies the native publication procedure.

A new head's folder takes the number one above the highest folder under its user's folder, `.ai-factory/architects/<user-slug>/`, and a new snapshot the number one above the highest snapshot in its folder; both start at `01` and are at least two digits wide, so several architects coexist without colliding. A project's chronicle is one file, `.ai-factory/chronicle/<project>.md`, in the skills repository, whichever repository the head works in: the skills repository's root is the repository of this engine's canonical installed source, `git -C "$(dirname "$(realpath <this SKILL.md as loaded>)")" rev-parse --show-toplevel`, and `<project>` is the project repository's directory name, verbatim — the last segment of `git rev-parse --show-toplevel` run in the project's own working directory — both resolved when the line is written. This engine is the home of that path and that numbering, of where the chronicle lies, and of the rule that governs who may write to the buffer and who may only read; the architect's own skill points here rather than restating them.

A snapshot continues the memory of one half of the pair, and only whoever held a context can report on it. It carries the volatile residue — where the work stands, what its writer knows, what will slip first, what must not be resolved by inference — and the reasoning that shaped it: why one option was taken over another, which premise proved false and how, what the user's own correction was and in what words; a snapshot that drops the reasoning is an inventory, not a history. It also carries how the knowledge was got: the method of any measurement or analysis, or where that method is kept; the shape of what was studied, when its source lies outside the reader's reach — where the source is at hand, a pointer is enough; and pointers to where the detail lives. A conclusion kept without its method cannot be re-checked or redone. The measure is where a thing lies: the snapshot keeps everything that lies in no file — the user's words, the reasoning, the method, the shape of what was studied — and whatever does lie in a file is replaced by a pointer, since everything durable already lives outside the conversation and restating it is never the point. It is thin only when asked for, or when what is plainly meant is a pointer list for raising context rather than a stretch's history. A head's snapshot also adds to the project's chronicle, located above, a line for each catch or vector turn since the last — who caught what, where the user turned the vector, a rescue by a link to its report. Each new snapshot supersedes the last by name — the numerically higher-numbered one is the current one — so a reader never follows a stale next action; what goes stale is the next action alone, and the record beside it stays.

The head is the buffer's only writer, as it is of its own snapshots; `editor/` is the hand's to write. The hand reads the buffer in full — that is what makes the memory the link between the two halves — and never writes to it. The buffer holds the working discipline, the rulings the user has made about how the pair works, the method learned from repeated failure, and the register of where the skills still lag the practice: the hand reads it as its own working context from the head it is the hand of, not as an outsider glancing at private notes, which is what lets an order name meaning instead of restating the discipline every time.

The hand re-reads the memory when it changes, not once at birth — and the same rule covers being named a different buffer's path outright by its architect: either way the memory has moved, and the hand adopts what it is now told, replacing what it held, holding one memory at a time.

A channel-message the architect is already sending may carry, alongside its payload, what keeps the hand current: a fact about where the shared memory sits — that the memory has moved, or the buffer's path itself — or what the hand needs in order to work at all. This opens no message of its own and takes no form of its own; it rides inside a message that already opens with its own token. It is never a reading of the payload: no finding, no conclusion, no verdict about what the message carries — what crosses for an independent reading still reaches the hand undisturbed, and a fact about where the memory sits carries no reading of it.

A ruling recorded in the buffer is a debt against the skill, not a record of one, and it drains to one of two places. A ruling about the project or its skills leaves the buffer when it reaches the artifact that should hold it; without that drain the buffer accumulates decisions everyone follows and no artifact states. The base behaviour of the pair drains to the seed the buffer is founded from and stays in the buffer as its standing entry, which both halves re-read all session. Something the user had to say twice, or that failed twice, is a debt to drain; said once, it is not.

Two architects are two heads, and a head that reads another's memory is no longer holding its own. Several architects coexist under the numbering above, and each keeps its own buffer: no architect reads another architect's buffer. A hand reads only its own architect's buffer — the memory of the head it is the hand of — never another architect's.

## Resolving installed resources

An installed skill path is a symlink chain, so resolve it through the links before using any relative resource: every relative resource path resolves against the directory of the resolved file, `base="$(dirname "$(realpath <file>)")"` — never appended to the file path itself, since `<file>/../x` is not a directory path.

- From a canonical skill `SKILL.md`, a sibling skill is `"$base/../<skill>/SKILL.md"` and a hand's definition is `"$base/../../agents/<name>.md"`, `<name>` being the hand's definition name (`editor`, for example).
- From a hand's canonical definition, a skill is `"$base/../skills/<skill>/SKILL.md"`.

Loading a skill uses the native facility only when it can load that skill by name. When the facility is absent, the name is not in the native manifest, or the native load fails, read the canonical resolved `SKILL.md` with the runtime's own facilities instead. That is the normal path before a personal per-item entry exists, not an error. One exception keys on the skill's own `disable-model-invocation`, however the failure shows, absent from the manifest or refused. In a harness whose skill facility honours the field (Claude Code), the fallback reads the skill's frontmatter first, and neither the head nor its hand reads a skill whose frontmatter carries `disable-model-invocation: true` past the frontmatter's closing `---`: such a skill is the user's to invoke, and the head asks the user to invoke it through the runtime's question facility. In a harness whose facility does not honour the field (Codex), the fallback reads the file as stated. Say in passing which path was used, since a native load and a source fallback are distinct witnesses. Never assume `~/.claude`.
