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
| `token-economy` | Cuts input/orchestration tokens for multi-agent work: "discover-once" context-pack (a single scan, file:line map), terse read-only agents, `frugal` output-style, and cross-run memory. Stacks with caveman. | [token-economy](https://github.com/davidgarciagordo/token-economy) |
| `design-review` | Design/redesign audit pipeline: reference research → 4 design lenses actually applied → live vitality verdict (alive/templated/flat) with a loop until it clears the bar. | [design-review](https://github.com/davidgarciagordo/design-review) |
| `forge-methodology` | Human↔AI methodology for serious work: align intent → versioned spec → adversarial grill ×3 → global plan → execution → verify against the Definition of Done → owner sign-off. | [forge-methodology](https://github.com/davidgarciagordo/forge-methodology) |
| `working-methods` | Cross-project working norms: `/grill` (adversarial ×3 attack on a spec/plan), `/handoff` (session relay), and `forge-on-claude` (the Forge encoded with non-skippable gates). | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |
| `automations` | `optimize-my-setup` skill: audits and tailors a repo's `.claude` config (CLAUDE.md, settings, hooks, agents, output-styles) and proposes improvements — you pick what gets applied. | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |

Each plugin is also independently installable from its own repo's marketplace — this catalog just aggregates them.

## Extra: statusline with badges

[`statusline_prompt.md`](statusline_prompt.md) — copy-paste prompt (Spanish) that has Claude Code set up a statusline showing dir · git branch · model/style · context % · 5h/7d limits, plus `[CAVEMAN]` / `[TOKEN-ECON]` badges if you use those plugins.

## License

MIT © David García Gordo
