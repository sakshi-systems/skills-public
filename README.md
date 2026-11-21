# sakshi-systems/skills

Generic AI Factory skills available globally to all projects via Claude Code's personal skill scope.

## Setup — activating the package (one-time, per machine)

*This section is addressed to the agent reading it on a fresh checkout.* Activating the package means that each surface under `~/.claude` reaches this repo's `active/` layer as this section's table of surfaces and its merge branch install it. The user may already keep their own global config in `~/.claude`, so **do not run this silently** — walk them through it one surface at a time and let them choose per surface.

Four surfaces under `~/.claude`, each with a target in `active/`:

| `~/.claude` surface | target in this repo |
|---|---|
| `CLAUDE.md` | `active/CLAUDE.md` (→ `src/global/CLAUDE.md`) |
| `skills/` | `active/skills/` |
| `commands/` | `active/commands/` |
| `agents/` | `active/agents/` |

For each surface, detect its current state (`readlink`/`test`) and act:

- **absent** — create the symlink to our target. Clean, no question.
- **already ours** (resolves back into this repo) — skip; the flow is idempotent.
- **the user's own file or directory** — stop and ask which of three they want:
  - **replace** — back theirs up (`mv <path> <path>.pre-sakshi.bak`), then symlink ours;
  - **keep theirs** — skip this surface entirely, leave ours unlinked;
  - **merge** — coexist, and *how* depends on the surface's kind:
    - `CLAUDE.md` is a single file → leave theirs a real file and append an import line `@<abs>/active/CLAUDE.md` to its tail (both sets of instructions load);
    - `skills/` `commands/` `agents/` are directories of independent items → leave theirs a real directory and link our items into it one by one (`ln -sfn <abs>/active/<kind>/<item> ~/.claude/<kind>/<item>`), asking on any name collision.

Four further surfaces live in `~/.claude/settings.json` and are not symlinks: two `SessionStart` hooks, a `UserPromptSubmit` hook and one key. The lines the two hooks print are the machine's moments, whose one home is Sakshi's code ([the machine's moments](../orchestrator/docs/future/sakshi.md#the-machines-moments), [Sakshi is the home](../orchestrator/docs/future/sakshi.md#sakshi-is-the-home)); once [`sakshi hook <moment>`](../orchestrator/docs/reference/sakshi.md#the-hook-command) exists, the entries call it and carry no text of their own, and until then the entries below stand as they are. The slug's rule stays in `roadmap-engine/scripts/user-slug.sh`. The first, the slug hook, has a command that prints `The user's slug: ` followed by the output of `~/.claude/skills/roadmap-engine/scripts/user-slug.sh`, so that each session starts holding the user's slug, and prints nothing, exiting zero, when the script yields no slug. Walk the user through it like the others. Read their `settings.json` first. If a `SessionStart` entry already runs that script, skip it. If the file is absent, or holds other settings, show the entry below merged into what they keep, preserving every existing key and any other `SessionStart` entries, and write it only on their word. If they decline, skip the surface.

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "s=$(~/.claude/skills/roadmap-engine/scripts/user-slug.sh 2>/dev/null) && printf \"The user's slug: %s\\n\" \"$s\" || true"
          }
        ]
      }
    ]
  }
}
```

The second, the compact hook, fires on the `SessionStart` that follows a compact, the entry whose matcher is `compact`, and prints one line: `This compact unloaded every skill: the excerpts of earlier skills shown above are not loads, so a skill is loaded again when its time comes. A head runs its start before anything else, the next step of the summary included: the summary is not its memory.` It is the compact's half of the rule in `src/global/CLAUDE.md`, "Skills", so that a fresh machine gets the rule with the hook that makes it true. Walk the user through it the same way. Read their `settings.json` first. If a `SessionStart` entry whose matcher is `compact` already prints that line, skip it. Otherwise show the entry below merged into what they keep, preserving every existing key and every other `SessionStart` entry, the slug hook's included, and write it only on their word. If they decline, skip the surface.

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "printf '%s\\n' 'This compact unloaded every skill: the excerpts of earlier skills shown above are not loads, so a skill is loaded again when its time comes. A head runs its start before anything else, the next step of the summary included: the summary is not its memory.'"
          }
        ]
      }
    ]
  }
}
```

The third, the words hook, is a `UserPromptSubmit` hook that runs `scripts/record-user-words.py`. It is the family's own record of the user's words: each message typed in a session whose working directory lies under the family's root, the directory holding this checkout, is appended verbatim to `.ai-factory/user-words/<date>.md` in this repository, with its local time, the sub-repository it was typed in and the session's short id, so that what he said can be read later, past every compact, with no agent doing the writing. A session in any other directory is ignored. The script prints nothing, since a `UserPromptSubmit` hook's output enters the model's context, and it never fails a prompt. It records everything typed, so a person who types secrets into a chat should know that their words land in a file of this repository that is committed with the rest; the user of this checkout sends none. Walk the user through it the same way. Read their `settings.json` first. If a `UserPromptSubmit` entry already runs that script, skip it. Otherwise show the entry below, with `<absolute path of this checkout>` replaced by this checkout's absolute path, merged into what they keep, preserving every existing key and every other hook entry, and write it only on their word. If they decline, skip the surface.

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 <absolute path of this checkout>/scripts/record-user-words.py"
          }
        ]
      }
    ]
  }
}
```

The fourth, the key, is `"idleCompaction": false`. A client that runs Claude Code through the Agent SDK, such as Zed's ACP adapter `claude-agent-acp`, compacts an idle session on its own: when a session with a large context has been quiet for close to the prompt cache's hour, it sends `/compact` into it as an ordinary prompt. That compact arrives with no snapshot written before it, and a compact is meant to be preceded by one. The key turns off only that idle compaction; the auto-compact at the window's edge stays. `autoCompactEnabled: false` and `DISABLE_AUTO_COMPACT` would turn off both, so they are not the switch. Walk the user through it the same way. Read their `settings.json` and, where present, managed settings. If either already holds the key `false`, skip it. Otherwise show the key below merged into what they keep, preserving every existing key, and write it only on their word. If they decline, skip the surface.

```json
{
  "idleCompaction": false
}
```

The whole flow is idempotent (a second run is all-skips) and reversible (our links resolve back into this repo; a replaced original sits beside its symlink as `*.pre-sakshi.bak`).

`active/` is the curated working set: `active/skills/`, `active/commands/`, and `active/agents/` hold per-item symlinks into `src/`, so only skills actually in use are loaded, not every skill that exists.

The architect’s [runtime mechanics](docs/architect-runtimes.md) require both curated runtime-skill links, the role-skill links `architect-role-builder`, `architect-role-razor` and `architect-role-navigator`, and entries visible in the actual harness’s skill scope. With per-item directories, expose each selected runtime skill and the role skills individually in `~/.claude/skills/` or `~/.codex/skills/`, preserving the same user-choice and collision handling above. On the Claude surface the hand definitions `editor.md` and `explorer.md` are exposed the same way, individually in `~/.claude/agents/`; the Codex runtime needs no personal entry, since its child reads the canonical definition from the installed source. Inspect the resolved entry rather than assuming that adding an `active/skills/` link updated an existing personal directory. When native skill loading is unavailable, callers read the runtime sibling from their canonical installed source. This does not require replacing native user settings.

## Project-specific skills

If a project needs custom skills alongside the generic ones, place them in `.claude/skills/` inside that project directory. Claude Code loads both: personal scope (`~/.claude/skills`) and project scope (`.claude/skills`).

## Invoke skills

```
/aif                 — set up AI context for a project
/roadmap-outline     — strategic roadmap (high-level milestones)
/roadmap-decompose   — break milestones into atomic, spec'd tasks
```

## Structure

Our skills live under `src/skills/` (commands under `src/commands/`, agent definitions under `src/agents/`); `active/` symlinks the curated working set. Each skill is a directory containing a `SKILL.md` (frontmatter + instructions) and optional `references/`, `scripts/`, and `templates/` subdirectories.

## Origins

The family's skills are originally based on [AI Factory](https://github.com/lee-to/ai-factory); `aif`, `aif-architecture`, `aif-docs` and `aif-plan` are reworked from their counterparts there. `ui-ux-pro-max` comes from [ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) (MIT), and its notice stays in the skill's folder.

## Adding Skills

A new skill is a directory under `src/skills/` with a `SKILL.md`; it joins the working set by a symlink under `active/skills/`. An external skill is read whole before use, and an instruction that does not serve its stated purpose blocks it.
