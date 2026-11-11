# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A **Claude Code plugin marketplace**: `.claude-plugin/marketplace.json` plus one directory per plugin
under `plugins/`. No application code, build step or package manager — Markdown (skills), JSON
(manifests, hooks, evals) and one shell script.

The marketplace's identifier is **`zmarkoni-plugins`** (the `name` field in `marketplace.json`),
independent of the GitHub repo name. Users install with `/plugin install <plugin-name>@zmarkoni-plugins`.

## Commands

```bash
claude plugin validate .                                   # the catalog, and every plugin.json it reaches
claude plugin validate ./plugins/<plugin-name> --strict    # one plugin; unknown fields become errors
claude --plugin-dir ./plugins/<plugin-name> -p "/<plugin-name>:<skill-name> <test prompt>"
```

**A plugin whose files changed carries a new `version` in its `plugin.json`, and its catalog entry in
`marketplace.json` carries the same one.** An installed copy does not update until the version moves.

Neither plugin has a test runner. `plugins/<plugin-name>/evals/evals.json` (where present) holds scripted
scenarios — the spec to check a skill's behaviour against when editing it.

## The no-hallucinations hook/skill pattern

`plugins/no-hallucinations/` injects its rules two ways — a `SessionStart` hook (full rules, once per
session) and a `UserPromptSubmit` hook (one-line reminder every turn, to survive compaction) — plus an
on-demand `skills/be-honest/SKILL.md`. **`skills/be-honest/SKILL.md` is the single source of truth**:
`scripts/inject-rules.sh` strips its frontmatter and prints the body for the `SessionStart` hook. Edit
the skill file only, never the hook's injected text.

## Known gotchas

- **Changing `marketplace.json`'s `name` breaks every existing `@<old-name>` install**; there is no
  migration for a marketplace name.
- **The key in `extraKnownMarketplaces`, the part after `@` in `enabledPlugins`, and `marketplace.json`'s
  `name` must match exactly**, or the plugin silently fails to resolve (see `example-project/`).
- **Don't verify hooks from a sandboxed shell.** A nested run inside the Bash sandbox cannot write under
  `~/.claude/`, so hooks die with `EPERM ... mkdir '.../plugins/data/...'` and the plugin looks unloaded.
  Check `~/.claude/debug/latest` for those lines first.
- **A skill's `model:` frontmatter only pins the model for the invoking turn**, so it cannot hold
  `made-decision`'s multi-turn interview on one model; the README recommends `/model` instead.
