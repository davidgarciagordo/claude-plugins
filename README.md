# claude-plugins

> 🇪🇸 [Léelo en castellano](README.es.md)

Single marketplace catalog for all of David García Gordo's Claude Code plugins. Each plugin lives and updates in its own repo — this catalog gives you one place to browse and install from.

The common thread: every plugin turns "the model promises" into "a mechanism enforces". Deterministic scripts instead of vibes, machine-checked gates instead of self-declared green, measured numbers instead of claims.

## Install

Add the marketplace once:

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

| Plugin | What it solves | Star component | Source |
|---|---|---|---|
| `token-economy` | Multi-agent sessions burn input tokens rediscovering the same context. Measured 2.6×–7× fewer input tokens at identical coverage. | Deterministic, tested context-pack script ("discover once") + read-only lens agent scoped by tool list + `frugal` output-style. | [token-economy](https://github.com/davidgarciagordo/token-economy) |
| `design-review` | Third-party design skills produce templated output and skip their own setup steps. This pipeline makes them actually run — and proves it. | 8-agent pipeline with binary gates, a machine-checkable `alive/templated/flat` verdict enforced by hook, and 7 verified playbooks pinned to commit. | [design-review](https://github.com/davidgarciagordo/design-review) |
| `forge-methodology` | "Done" declared against the executor's own idea of done, not against the goal. Here completeness is mechanical, not felt. | Enumerated reference with req-ids → Acceptance Matrix → hook that blocks `gh pr create` until every row has evidence verified by someone ≠ executor. 8 domain packs, 8 end-to-end examples. | [forge-methodology](https://github.com/davidgarciagordo/forge-methodology) |
| `working-methods` | A methodology the model can skip is a suggestion. This is the Forge ENFORCEMENT layer inside Claude Code. | `/forge-run`: 12 phases sequenced by `forge.js` (zero-dependency state machine, machine-checked gates) + fail-closed PR-gate hook. Plus `/grill` (3-4 read-only adversarial lenses, binary finding criterion) and `/handoff` (session relay on a durable scheduler). | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer/tree/main/plugins/working-methods) |
| `automations` | Repo `.claude` config is usually ad-hoc and stale. This bootstraps it deterministically — and never applies anything you didn't tick. | `/optimize-my-setup`: deterministic scan (`scan.mjs`) of all 8 `.claude` surfaces + read-only agent fan-out + mandatory multi-check. Ships 4 fail-closed template hooks (guard-main, secrets-guard, commit-lint, ui-diff), 5 adversarial reviewers generated per-repo, and `/release`. | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer/tree/main/plugins/automations) |

## How they compose

The five plugins form one family, each owning a distinct layer:

- **`forge-methodology`** — the methodology: spec → adversarial grill → plan → verified done.
- **`working-methods`** — the enforcement of that methodology in Claude Code: `/forge-run` gates each phase mechanically so steps can't be skipped.
- **`token-economy`** — the cost layer: any multi-agent phase (grill lenses, reviewers, design lenses) runs 2.6×–7× cheaper with the same coverage.
- **`design-review`** — the design pipeline: a specialized, gated review for UI work, pluggable as the design phase of a Forge run.
- **`automations`** — the bootstrap: sets up the repo's `.claude` config (hooks, reviewers, settings) that everything above runs on.

**Every plugin works standalone.** Composition is optional — install one, get its full value; install several, they snap together.

## Extra: statusline with badges

[`statusline_prompt.md`](statusline_prompt.md) — copy-paste prompt (Spanish) that has Claude Code set up a statusline showing dir · git branch · model/style · context % · 5h/7d limits, plus `[CAVEMAN]` / `[TOKEN-ECON]` badges if you use those plugins.

## License

MIT © David García Gordo
