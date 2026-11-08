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

Two further surfaces are not symlinks, and each is a `SessionStart` hook in `~/.claude/settings.json`. The first, the slug hook, has a command that prints `The user's slug: ` followed by the output of `~/.claude/skills/roadmap-engine/scripts/user-slug.sh`, so that each session starts holding the user's slug, and prints nothing, exiting zero, when the script yields no slug. Walk the user through it like the others. Read their `settings.json` first. If a `SessionStart` entry already runs that script, skip it. If the file is absent, or holds other settings, show the entry below merged into what they keep, preserving every existing key and any other `SessionStart` entries, and write it only on their word. If they decline, skip the surface.

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

The second, the compact hook, fires on the `SessionStart` that follows a compact, the entry whose matcher is `compact`, and prints one line: `This compact unloaded every skill: the excerpts of earlier skills shown above are not loads, so a skill is loaded again when its time comes.` It is the compact's half of the rule in `src/global/CLAUDE.md`, "Skills", so that a fresh machine gets the rule with the hook that makes it true. Walk the user through it the same way. Read their `settings.json` first. If a `SessionStart` entry whose matcher is `compact` already prints that line, skip it. Otherwise show the entry below merged into what they keep, preserving every existing key and every other `SessionStart` entry, the slug hook's included, and write it only on their word. If they decline, skip the surface.

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "printf '%s\\n' 'This compact unloaded every skill: the excerpts of earlier skills shown above are not loads, so a skill is loaded again when its time comes.'"
          }
        ]
      }
    ]
  }
}
```

The whole flow is idempotent (a second run is all-skips) and reversible (our links resolve back into this repo; a replaced original sits beside its symlink as `*.pre-sakshi.bak`).

`active/` is the curated working set: `active/skills/`, `active/commands/`, and `active/agents/` hold per-item symlinks into `src/`, so only skills actually in use are loaded, not every skill that exists.

The architect’s [runtime mechanics](docs/architect-runtimes.md) require both curated runtime-skill links and entries visible in the actual harness’s skill scope. With per-item directories, expose each selected runtime skill individually in `~/.claude/skills/` or `~/.codex/skills/`, preserving the same user-choice and collision handling above. Inspect the resolved entry rather than assuming that adding an `active/skills/` link updated an existing personal directory. When native skill loading is unavailable, callers read the runtime sibling from their canonical installed source. This does not require replacing native user settings.

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
