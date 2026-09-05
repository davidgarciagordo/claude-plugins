# claude-plugins

> 🇬🇧 [Read this in English](README.md)

Catálogo único (marketplace) de todos los plugins de Claude Code de David García Gordo. Cada plugin vive y se actualiza en su propio repo — este catálogo te da un único sitio desde el que explorar e instalar.

El hilo conductor: cada plugin convierte "el modelo promete" en "un mecanismo lo impone". Scripts deterministas en vez de fe, gates machine-checked en vez de verde auto-declarado, números medidos en vez de claims.

## Instalación

Añade el marketplace una vez:

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
/plugin install swarm@davidgarciagordo-plugins
```

## Plugins

| Plugin | Qué resuelve | Componente estrella | Fuente |
|---|---|---|---|
| `token-economy` | Las sesiones multi-agente queman tokens de entrada redescubriendo el mismo contexto. Medido: 2,6×–7× menos tokens de entrada con cobertura idéntica. | Script de context-pack determinista y testeado ("discover once") + agente-lente read-only acotado por lista de tools + output-style `frugal`. | [token-economy](https://github.com/davidgarciagordo/token-economy) |
| `design-review` | Los skills de diseño de terceros producen output templated y se saltan sus propios pasos de setup. Este pipeline los hace correr de verdad — y lo demuestra. | Pipeline de 8 agentes con gates binarios, veredicto machine-checkable `alive/templated/flat` impuesto por hook, y 7 playbooks verificados y pineados a commit. | [design-review](https://github.com/davidgarciagordo/design-review) |
| `forge-methodology` | "Hecho" declarado contra la idea de done del ejecutor, no contra el objetivo. Aquí la completitud es mecánica, no una sensación. | Referencia enumerada con req-ids → Acceptance Matrix → hook que bloquea `gh pr create` mientras falte evidencia verificada por alguien ≠ ejecutor. 8 domain packs, 8 ejemplos end-to-end. | [forge-methodology](https://github.com/davidgarciagordo/forge-methodology) |
| `working-methods` | Una metodología que el modelo puede saltarse es una sugerencia. Esta es la capa de ENFORCEMENT de la Forja dentro de Claude Code. | `/forge-run`: 12 fases secuenciadas por `forge.js` (máquina de estados sin dependencias, gates machine-checked) + hook PR-gate fail-closed. Además `/grill` (3-4 lentes adversariales read-only, criterio binario de hallazgo) y `/handoff` (relevo de sesión con scheduler durable). | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer/tree/main/plugins/working-methods) |
| `automations` | La config `.claude` de un repo suele ser ad-hoc y estar desactualizada. Esto la arranca de forma determinista — y nunca aplica nada que no hayas marcado. | `/optimize-my-setup`: scan determinista (`scan.mjs`) de las 8 superficies `.claude` + fan-out de agentes read-only + multi-check obligatorio. Incluye 4 hooks template fail-closed (guard-main, secrets-guard, commit-lint, ui-diff), 5 reviewers adversariales generados a medida por repo, y `/release`. | [claude-code-setup-optimizer](https://github.com/davidgarciagordo/claude-code-setup-optimizer/tree/main/plugins/automations) |
| `swarm` | Un objetivo de desarrollo pasa por descubrimiento, análisis, diseño, implementación y entrega, y cada fase la hace un agente distinto — sin que tú tengas que orquestar el traspaso entre ellos. | 36 agentes de responsabilidad única, memoria unificada (un escaneo del repo, compartido), grill ×3 adversarial sobre el plan, TDD en worktree aislado y un verificador independiente que nunca construyó lo que revisa. | [swarm](https://github.com/davidgarciagordo/swarm) |

## Cómo componen entre sí

Cinco de los seis plugins forman una familia, cada uno con su capa:

- **`forge-methodology`** — la metodología: spec → grill adversarial → plan → done verificado.
- **`working-methods`** — el enforcement de esa metodología en Claude Code: `/forge-run` gatea cada fase mecánicamente para que no se pueda saltar pasos.
- **`token-economy`** — la capa de coste: cualquier fase multi-agente (lentes de grill, reviewers, lentes de diseño) corre 2,6×–7× más barata con la misma cobertura.
- **`design-review`** — el pipeline de diseño: una review especializada y gateada para trabajo de UI, enchufable como fase de diseño de un run de Forja.
- **`automations`** — el bootstrap: monta la config `.claude` del repo (hooks, reviewers, settings) sobre la que corre todo lo anterior.

**`swarm` no es una capa más — es el sexto plugin, y encadena varias de las de arriba dentro de un único ciclo:** un objetivo entra, y discovery, análisis, diseño (con su propio grill ×3), implementación TDD y entrega salen encadenados, con memoria compartida entre fases en vez de que cada agente redescubra el repo. Resuelve el mismo problema que `working-methods`/`forge-methodology` (que la metodología no dependa de que el agente se acuerde) pero para el ciclo de desarrollo completo, no solo para el gate de un PR.

**Cada plugin funciona standalone.** La composición es opcional — instala uno y tienes su valor completo; instala varios y encajan entre sí.

## Extra: statusline con badges

[`statusline_prompt.md`](statusline_prompt.md) — prompt copy-paste para que Claude Code te monte una statusline con dir · rama git · modelo/estilo · % contexto · límites 5h/7d, y badges `[CAVEMAN]` / `[TOKEN-ECON]` si usas esos plugins.

## Licencia

MIT © David García Gordo
