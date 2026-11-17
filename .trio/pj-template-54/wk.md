# Entrega WK — robert-flo/pj-template#54

- **PR:** https://github.com/robert-flo/pj-template/pull/57 (`#57`)
- **Base:** `52-os-y-flujo-herdr`
- **Head SHA:** `9caab2e68454c49c3689d478bbcde4f0e3aece22`
- **Merge SHA:** `685a00814808f8670133f9c8f5ea449090845e36`
- **Estado:** Mergeado por el WK a `52-os-y-flujo-herdr` (`--squash --delete-branch --match-head-commit`).

## Cambios implementados

1. **Despacho visible en `herdr` (`comun.md` y `pm.md`):**
   - Engineering vía `pj-trio <proyecto> <ticket> [--repo <miembro>] [--base <rama>]`.
   - Trivial vía `pj-trio --oneshot <proyecto> <ticket> [--repo <miembro>]` (o `make agy-os` para side-tasks directos).
   - Directriz de evitar `invoke_subagent` para tareas largas de desarrollo/programación.
2. **Supervisión de flota con 0 tokens (`comun.md` y `pm.md`):**
   - Prohibición de polling en bucle gastando tokens.
   - `pj-trio watch <PROYECTO>` para espera pasiva de eventos accionables (`hecho`, `needs-roberto`, `blocked`, `unknown`).
   - `pj-trio fleet <PROYECTO> [--activos]` para consultas de estado consolidado bajo demanda.
3. **Política de `Merge Danger` (`comun.md` y `pm.md`):**
   - Auto-merge condicionado a `Door: two-way` + `Blast Radius: Low` (junto con CI verde, `code-review` limpio y pruebas reales).
   - Merge humano obligatorio (`ready-to-merge` + `needs-roberto`) ante `Door: one-way` o `Blast Radius` distinto de `Low`.
4. **Fronteras DAG en `implement-spec` (`pm.md`):**
   - Despacho de sub-issues desbloqueados en paralelo con `pj-trio --base <rama-del-spec>`.
5. **ADR 0021:**
   - Registrado en `docs/adr/0021-flujo-herdr-y-politica-merge-danger.md` y enlazado en `docs/adr/README.md`.
6. **Robustez de check (`scripts/check.sh`):**
   - Fijado `LC_ALL=C comm` para prevenir discrepancias de ordenamiento por locale en entornos UTF-8.

## Pruebas verificadas

- `make check`: `check: todo bien` en `pj-template`.
- CI (`check/check (pull_request)` en PR #57): verde en 29s.
- `code-review`: limpio en estándares y especificación.
- Squash & merge ejecutado sobre `52-os-y-flujo-herdr` en commit `685a00814808f8670133f9c8f5ea449090845e36`.
