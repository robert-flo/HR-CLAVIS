# Plan — robert-flo/HR-CLAVIS#119

- **Issue:** `robert-flo/HR-CLAVIS#119` (sub-issue de `robert-flo/HR-CLAVIS#116`)
- **Repo destino:** `robert-flo/ravn-trio`
- **Base del PR:** `116-trio-por-spec-y-herdr-resiliente`
- **Worktree:** `/z/repos/ravn-trio/119-pm-name-herdr`
- **Rama:** `issue/119-pm-name-herdr`

## Qué hay que construir

Corregir el bug por el cual el PM lanzado con `agy -i` queda sin campo `name` en `herdr agent list` tras el timeout de `herdr agent start`, provocando que `pj-trio watch` reporte `pm=unknown` y salga al instante:

1. Arrancar el PM en `runtime/lib/trio_herdr.sh` (y revisar `runtime/scripts/start` si aplica) sin `-i` (esperando `interactive_ready: true` para que `herdr` registre `name: pm-<proyecto>-<ticket>`) y luego enviar el kickoff de una línea vía `herdr agent prompt <agent_name>` (manteniendo fallback si `herdr` no confirma).
2. Complementariamente, en `runtime/scripts/watch` y `runtime/scripts/fleet`, si el agente no aparece por `name`, verificar también por `pane_id` / `workspace_id` del trío antes de reportar `pm=unknown`.

## Criterios de aceptación

- [ ] `shellcheck` limpio en `runtime/` (`make lint`).
- [ ] `make test` en verde: tests automatizados con mock de `herdr` verifican que el PM se registra con su `name` antes de recibir el kickoff vía `herdr agent prompt` y que `pj-trio watch` no sale prematuramente con `pm=unknown`.
- [ ] `code-review` limpio y PR abierto contra `116-trio-por-spec-y-herdr-resiliente` en `robert-flo/ravn-trio`, referenciando `robert-flo/HR-CLAVIS#119` en el body (no `Closes #119`).
- [ ] Al tratarse de un sub-issue (`--base`), tras CI verde y `code-review` limpio, el WK mergea el PR a `116-trio-por-spec-y-herdr-resiliente` con `gh pr merge <pr> -R robert-flo/ravn-trio --squash --delete-branch --match-head-commit <sha-verde>` y anota PR, head sha, pruebas y merge sha en `wk.md`.

## Notas del repo

- Leer `/z-home-repos/HR-CLAVIS/ravn-trio/AGENTS.md`.
- No editar `runtime/lib/{presentation,command_catalog,command_interface,dispatch,lifecycle}.sh`.
- Commits sin trailers `Co-authored-by` ni atribución a agentes.
