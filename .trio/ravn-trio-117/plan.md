# Plan — robert-flo/HR-CLAVIS#117

- **Ticket:** `robert-flo/HR-CLAVIS#117` (sub-issue de `robert-flo/HR-CLAVIS#116`)
- **Repo destino:** `robert-flo/ravn-trio`
- **Base del PR:** `116-trio-por-spec-y-herdr-resiliente`
- **Rama de trabajo:** `issue/117-modo-spec-y-blindaje-herdr`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/ravn-trio/117-modo-spec-y-blindaje-herdr`
- **Carpeta de handoff:** `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-117`

## Qué hay que construir

En `ravn-trio`:

1. **Modo `spec` (1 trío por spec):**
   - Detectar automáticamente si el issue tiene etiqueta `spec` (vía `gh issue view`) o aceptar flag explícito `--spec`.
   - En `renderizar_kickoff` (`kickoff.md`), cuando sea modo `spec`, instruir al PM del trío a:
     - Leer el spec y sus sub-issues vinculados, y ordenar al WK ejecutar `/implement-spec` sobre el spec (creando worktrees para cada sub-issue sobre la rama del spec `<N>-<slug>`, haciendo `/code-review` y mergeando cada sub-PR a la rama del spec, hasta abrir el PR final de la rama del spec hacia la rama por defecto `main`/`master`).
     - Al recibir el PR final abierto, pasarlo al RV en `herdr` (rebotando al WK si hay `BLOQUEO`), y tras `APRUEBO <sha>` evaluar `Merge Danger` (auto-merge si `two-way` + `Low`, o `needs-roberto` en otro caso) y cerrar el trío.
   - Mantener compatibilidad con tickets unitarios y `--base <rama>` si se pasa explícito.

2. **Blindaje de `herdr`:**
   - En `trio_herdr.sh`, dar margen tras crear el pane y reintentar 1 vez `herdr agent start` si devuelve `no longer owns the target terminal` (capturando la última salida del pane en el error si vuelve a fallar).
   - En `kickoff.md`, indicar que si `herdr agent wait` devuelve `idle` sin que exista `wk.md`/`rv.md` (por subagentes en segundo plano como `Spec Code Reviewer`), el PM del trío verifique actividad (`herdr agent read` / `gh pr list`) y vuelva a esperar antes de contarlo como fallo y escalar a `needs-roberto`.

## Criterios de aceptación

- `shellcheck` (`make lint`) pasa limpio en `runtime/` y scripts modificados.
- `make test` pasa limpio: tests automatizados con mocks de `herdr` y `gh` verifican:
  - generación de `kickoff.md` en modo `spec`,
  - reintento en `trio_herdr.sh` ante `no longer owns the target terminal`,
  - texto/lógica de manejo de `idle` sin archivo de entrega.
- PR abierto contra `116-trio-por-spec-y-herdr-resiliente` en `robert-flo/ravn-trio` (referenciando `robert-flo/HR-CLAVIS#117` en el body, sin `Closes #117`).
- Al ser sub-issue con CI verde y `code-review` limpio, el WK mergea el PR a `116-trio-por-spec-y-herdr-resiliente` con `--squash --delete-branch --match-head-commit <sha-verde>` y anota el SHA del merge en `wk.md`.
