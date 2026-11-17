# Plan — robert-flo/pj-template#54

- **Ticket:** `robert-flo/pj-template#54` — `docs(reglas): flujo herdr (pj-trio, fleet, watch, --oneshot) y Merge Danger en pm.md/comun.md`
- **Parent:** Spec #52 (rama base: `52-os-y-flujo-herdr`)
- **Tipo:** Sub-issue (no pasa por RV; el WK mergea a `52-os-y-flujo-herdr` tras CI verde y `code-review` limpio)
- **Worktree:** `/z/repos/pj-template/54-reglas-herdr-merge-danger`
- **Rama:** `issue/54-reglas-herdr-merge-danger`
- **Base / Target PR:** `52-os-y-flujo-herdr`

## Qué construir

Actualizar `.agents/reglas/pm.md` y `.agents/reglas/comun.md` para incorporar el flujo de orquestación en `herdr` validado en `HR-CLAVIS`:

1. **Despacho visible en `herdr`:**
   - Engineering vía `pj-trio`.
   - Trivial vía `pj-trio --oneshot` (o `make agy-os` para side-tasks directos).
   - Evitar `invoke_subagent` para trabajo largo.

2. **Supervisión de flota con 0 tokens:**
   - `pj-trio watch <PROYECTO>` (solo eventos accionables: `hecho`, `needs-roberto`, `blocked`, `unknown`).
   - Reportes bajo demanda con `pj-trio fleet <PROYECTO>`.

3. **Política de `Merge Danger`:**
   - Auto-merge en `Door: two-way` + `Blast Radius: Low`.
   - Merge humano (`ready-to-merge` + `needs-roberto`) en cualquier otro caso.

4. **Fronteras DAG en `implement-spec`:**
   - Despacho de sub-issues desbloqueados en paralelo con `pj-trio --base <rama-del-spec>`.

## Acceptance Criteria

- [ ] `make check` pasa en verde en `pj-template`.
- [ ] PR abierto contra `52-os-y-flujo-herdr` y mergeado por el WK tras CI verde y `code-review` limpio.
