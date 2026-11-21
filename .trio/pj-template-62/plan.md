# Plan — robert-flo/HR-CLAVIS#62 (repo `robert-flo/pj-template`)

## Datos clave
- **Issue:** `robert-flo/HR-CLAVIS#62` (Parent: `robert-flo/HR-CLAVIS#58`)
- **Repo de trabajo:** `robert-flo/pj-template`
- **Worktree:** `/z/repos/pj-template/62-watch-ticket-rules`
- **Rama:** `issue/62-watch-ticket-rules`
- **Rama base del PR:** `58-watch-ticket-filter` (rama del spec; es un sub-issue)
- **Handoff del WK:** `/z-home-repos/HR-CLAVIS/.trio/pj-template-62/wk.md`

## Objetivo
Actualizar las reglas canónicas de `pj-template` y el ADR 0021 para que toda sesión de PM (`make pm`) supervise pasivamente únicamente los tickets despachados en su propia sesión usando `pj-trio watch <PROYECTO> --ticket <clave1> [--ticket <clave2> ...]`, evitando falsos despertares cuando hay varios PMs en paralelo sobre el mismo Home Repo o proyecto.

## Archivos a actualizar (en `/z/repos/pj-template/62-watch-ticket-rules`)
1. `.agents/reglas/pm.md` (§Tu flujo 7 — *Supervisión con 0 tokens*):
   - Exigir `pj-trio watch <PROYECTO> --ticket <clave1> [--ticket <clave2> ...]` con las claves exactas de los sub-issues despachados por esa sesión en la frontera DAG (nunca sin `--ticket` cuando pueda haber sesiones en paralelo).
   - Documentar que el filtro evita falsos despertares y despierta de inmediato si un ticket ya cerró antes de lanzar `watch`.
2. `.agents/reglas/comun.md` (§El trío — *Supervisión de flota con 0 tokens*):
   - Actualizar la mención de `pj-trio watch` para incluir `[--ticket <clave> ...]` y aclarar su uso por sesión.
3. `docs/adr/0021-flujo-herdr-y-politica-merge-danger.md`:
   - Actualizar la mención de `pj-trio watch` para reflejar el filtrado por `--ticket`.

## Criterios de aceptación y pruebas obligatorias
1. `.agents/reglas/pm.md`, `.agents/reglas/comun.md` y `docs/adr/0021-flujo-herdr-y-politica-merge-danger.md` documentan de forma consistente `pj-trio watch <PROYECTO> --ticket <clave>...`
   - **Prueba:** pegar la salida de `grep -rn "pj-trio watch" .agents/reglas/ docs/adr/` en `## Evidence` del PR.
2. `make check` en `pj-template` pasa en verde sin errores ni deriva.
   - **Prueba:** pegar la salida real de `make check` en `## Evidence` del PR.
3. PR abierto en `robert-flo/pj-template` contra `58-watch-ticket-filter`, redactado con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `Closes robert-flo/HR-CLAVIS#62` / `Part of robert-flo/HR-CLAVIS#58`, sin trailers de agente ni `Co-authored-by` en commits, con `code-review` limpio y CI verde.

## Cierre del WK (sub-issue)
- Al ser un sub-issue sobre la rama del spec `58-watch-ticket-filter`, una vez que el CI esté verde y el `code-review` esté limpio, el WK mergea su propio PR:
  `gh pr merge <pr> -R robert-flo/pj-template --squash --delete-branch --match-head-commit <sha-verde>`
- Escribir `/z-home-repos/HR-CLAVIS/.trio/pj-template-62/wk.md` con:
  - URL del PR
  - Head SHA revisado/mergeado
  - Merge SHA
  - Resumen de pruebas ejecutadas (`grep` y `make check`) y resultado de CI / `code-review`
