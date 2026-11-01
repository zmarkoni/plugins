# zmarkoni/plugins

A Claude Code plugin marketplace with two small behaviour plugins.

- **[no-hallucinations](plugins/no-hallucinations/README.md)** — truthfulness rules for every response, through two hooks and a skill.
- **[made-decision](plugins/made-decision/README.md)** — a six-step decision interview (Outcomes, Options, Consequences, Evaluate, Mitigate, Resolve).

## Install

```bash
/plugin marketplace add zmarkoni/plugins
/plugin install no-hallucinations@zmarkoni-plugins
/plugin install made-decision@zmarkoni-plugins
```

To enable a plugin for one project only, commit a `.claude/settings.json` — see [`example-project/`](example-project/).

## Moving from `@agenticdraft`

Both plugins used to ship in the `agenticdraft` marketplace (`agenticDraft/plugins`). If you installed them from there:

```bash
claude plugin uninstall no-hallucinations@agenticdraft
claude plugin uninstall made-decision@agenticdraft
claude plugin marketplace add zmarkoni/plugins
claude plugin install no-hallucinations@zmarkoni-plugins
claude plugin install made-decision@zmarkoni-plugins
```

A committed `enabledPlugins` entry such as `no-hallucinations@agenticdraft` changes to `no-hallucinations@zmarkoni-plugins`, together with its `extraKnownMarketplaces` key.

## Layout

```text
.claude-plugin/marketplace.json      # the catalog, name "zmarkoni-plugins"
plugins/
├── no-hallucinations/               # always-on truthfulness rules (hooks + skill)
└── made-decision/                   # on-demand OOCEMR decision interview (skill + evals)
example-project/                     # project-scoped install via committed .claude/settings.json
```

## Validate

```bash
claude plugin validate .
claude plugin validate ./plugins/no-hallucinations --strict
claude plugin validate ./plugins/made-decision --strict
```

## License

MIT — see [LICENSE](LICENSE).
