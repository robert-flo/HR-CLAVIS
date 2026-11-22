# Plan — robert-flo/HR-CLAVIS#118

- **Ticket:** `robert-flo/HR-CLAVIS#118` (sub-issue de `#116`, `#117` ya cerrado)
- **Repo destino:** `robert-flo/pj-template`
- **Rama base:** `116-trio-por-spec-y-herdr-resiliente` (`origin/116-trio-por-spec-y-herdr-resiliente`)
- **Rama del PR:** `issue/118-pm-md-trio-por-spec`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/pj-template/118-pm-md-trio-por-spec`

## Qué hay que hacer

1. Actualizar `plantillas/reglas/pm.md` (y reglas/ADRs/docs que referencien `Fronteras DAG` por sub-issue, por ejemplo `docs/adr/0021-flujo-herdr-y-politica-merge-danger.md` y cualquier otra referencia en `plantillas/` o `docs/`) para restaurar el flujo **1 trío por spec**:
   - **PM Orquestador:** ejecuta `ro-restate-goals` → `grill-with-docs` → `to-spec` → `to-tickets`, crea la rama del spec, despacha **1 `pj-trio`** por el spec completo (en Engineering) o `pj-trio --oneshot` (en Trivial), supervisa con `pj-trio watch --ticket <clave>` y reporta el resultado a Roberto.
   - **PM del trío (en `herdr`):** orquesta `/implement-spec` con el WK (sub-issues a la rama del spec con `/code-review`), abre el PR final, lanza al RV en `herdr`, rebota si hay `BLOQUEO`, y ejecuta el merge final tras `APRUEBO` según `Merge Danger`.
   - Eliminar la instrucción de que el PM Orquestador despache un `pj-trio` por cada sub-issue o lance al RV con `invoke_subagent`.
2. Regenerar adaptadores (`make generar` o `./pj`) para que `.agents/reglas/pm.md` y `.cursor/rules/` queden sincronizados.
3. Verificar que `./pj check` (o `make check`) pase limpio sin errores de esquema, idempotencia ni consistencia.
4. Abrir el PR contra `116-trio-por-spec-y-herdr-resiliente` en `robert-flo/pj-template`, esperando CI verde y corriendo `/code-review` antes de mergear a `116-trio-por-spec-y-herdr-resiliente` (`--squash --delete-branch --match-head-commit <sha>`).
