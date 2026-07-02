# claude-plugins

> 🇪🇸 [Léelo en castellano](README.es.md)

Single marketplace catalog for all of David García Gordo's Claude Code plugins. Each plugin still lives and updates in its own repo — this just gives you one place to browse and install from.

## Install

```bash
/plugin marketplace add davidgarciagordo/claude-plugins
```

Then install whatever you need:

```bash
/plugin install token-economy@davidgarciagordo-plugins
/plugin install design-review@davidgarciagordo-plugins
/plugin install forge-methodology@davidgarciagordo-plugins
/plugin install working-methods@davidgarciagordo-plugins
/plugin install automations@davidgarciagordo-plugins
```

## Plugins

| Plugin | What it does | Source repo |
|---|---|---|
| `token-economy` | Cuts input/orchestration tokens for multi-agent work | [token-economy](https://github.com/davidgarciagordo/token-economy) |
| `design-review` | Design/redesign audit pipeline (reference research → lenses → vitality verdict) | [design-review](https://github.com/davidgarciagordo/design-review) |
| `forge-methodology` | Human↔AI spec → grill → plan → execute → verify methodology | [forge-methodology](https://github.com/davidgarciagordo/forge-methodology) |
| `working-methods` | `/grill`, `/handoff`, `forge-on-claude` | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |
| `automations` | `optimize-my-setup` — tailors a repo's `.claude` config | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |

Each plugin is also independently installable from its own repo's marketplace — this catalog just aggregates them.

## Extra: statusline with badges

[`statusline_prompt.md`](statusline_prompt.md) — copy-paste prompt (Spanish) that has Claude Code set up a statusline showing dir · git branch · model/style · context % · 5h/7d limits, plus `[CAVEMAN]` / `[TOKEN-ECON]` badges if you use those plugins.

## License

MIT © David García Gordo
