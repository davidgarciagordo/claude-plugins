# claude-plugins

> 🇬🇧 [Read this in English](README.md)

Catálogo único (marketplace) de todos los plugins de Claude Code de David García Gordo. Cada plugin vive y se actualiza en su propio repo — esto solo te da un único sitio desde el que explorar e instalar.

## Instalación

```bash
/plugin marketplace add davidgarciagordo/claude-plugins
```

Después instala lo que necesites:

```bash
/plugin install token-economy@davidgarciagordo-plugins
/plugin install design-review@davidgarciagordo-plugins
/plugin install forge-methodology@davidgarciagordo-plugins
/plugin install working-methods@davidgarciagordo-plugins
/plugin install automations@davidgarciagordo-plugins
```

## Plugins

| Plugin | Para qué sirve | Repo fuente |
|---|---|---|
| `token-economy` | Recorta tokens de entrada/orquestación en trabajo multi-agente: context-pack "discover-once" (un escaneo, mapa file:línea), agentes read-only tersos, output-style `frugal` y memoria entre runs. Apila con caveman. | [token-economy](https://github.com/davidgarciagordo/token-economy) |
| `design-review` | Pipeline de auditoría de diseño/rediseño: investigación de referencias → 4 lentes de diseño aplicadas de verdad → veredicto de vitalidad en vivo (alive/templated/flat) con loop hasta pasar el listón. | [design-review](https://github.com/davidgarciagordo/design-review) |
| `forge-methodology` | Metodología humano↔IA para trabajo serio: alinear intención → spec versionado → grill adversarial ×3 → plan global → ejecución → verificación contra la Definition of Done → sign-off del owner. | [forge-methodology](https://github.com/davidgarciagordo/forge-methodology) |
| `working-methods` | Normas de trabajo transversales a proyectos: `/grill` (ataque adversarial ×3 a un spec/plan), `/handoff` (relevo entre sesiones) y `forge-on-claude` (la Forja codificada con gates no saltables). | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |
| `automations` | Skill `optimize-my-setup`: audita y adapta la config `.claude` de un repo (CLAUDE.md, settings, hooks, agentes, output-styles) proponiendo mejoras — tú eliges qué se aplica. | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer) |

Cada plugin también es instalable de forma independiente desde el marketplace de su propio repo — este catálogo solo los agrega.

## Extra: statusline con badges

[`statusline_prompt.md`](statusline_prompt.md) — prompt copy-paste para que Claude Code te monte una statusline con dir · rama git · modelo/estilo · % contexto · límites 5h/7d, y badges `[CAVEMAN]` / `[TOKEN-ECON]` si usas esos plugins.

## Licencia

MIT © David García Gordo
