# Plan — `robert-flo/pj-template#84`

**Issue:** [#84](https://github.com/robert-flo/pj-template/issues/84) — Actualizar documentación, plantillas de hijos y ADRs a la nueva convención `a-*` / `c-*` (sub-issue del spec [#81](https://github.com/robert-flo/pj-template/issues/81), desbloqueado tras [#82](https://github.com/robert-flo/pj-template/issues/82) y [#83](https://github.com/robert-flo/pj-template/issues/83)).
**Base / PR:** `81-redisenar-makefile-hijo` (rama del spec).
**Worktree:** `/z/repos/pj-template/84-actualizar-docs-adrs-a-c` (rama `issue/84-actualizar-docs-adrs-a-c`).

## Qué hay que hacer

1. **Actualizar documentación del template y plantillas de hijos (`plantillas/cursor/pm.mdc`, `docs/pj-template.md`, `AGENTS.md`, `README.md`):**
   - Reemplazar toda referencia vigente a `make agy-pm` / `make agy-wk` / `make agy-rv` / `make agy-os`, `make cursor-pm` / `make cursor-wk` / `make cursor-rv` / `make cursor-os` y `make pm` / `make wk` / `make rv` / `make os` por la nueva convención simétrica:
     - Rápidos (por defecto, `--solo`): `make a-pm`, `make a-wk`, `make a-rv`, `make a-os` (agy) y `make c-pm`, `make c-wk`, `make c-rv`, `make c-os` (Cursor CLI).
     - Con actualización previa (`git pull` + `./pj listo`): `make listo a-pm`, `make listo c-pm` (composición con `make listo <target>`).
   - Revisar también `plantillas/agents/` y `plantillas/cursor/` para confirmar que no quede ninguna referencia obsoleta (por ejemplo `plantillas/cursor/pm.mdc` aún menciona `make c-pm` / `make cursor-pm` o similares).
   - Nota: en `scripts/prueba-agy.sh` y `scripts/prueba-cursor.sh`, las listas `for viejo in pm os agy-pm ...` son tests que verifican que los targets viejos *ya no existen*; esos tests **no** se deben eliminar.

2. **Documentar el reemplazo en ADRs (`docs/adr/`):**
   - Crear `docs/adr/0024-convencion-simetrica-a-c-y-listo.md` (o siguiente número libre en `docs/adr/`) documentando el reemplazo de `make agy-*` / `make cursor-*` / `make pm` por la convención simétrica `a-*` / `c-*` en `--solo` por defecto y la composición `make listo <target>`, enmendando/reemplazando `0020-make-agy-pm.md` y `0022-make-cursor-pm.md` (y actualizando las referencias vigentes en `0021`, `0023` y `docs/adr/README.md`).
   - En `0020`, `0022`, `0023` y `0021`: añadir nota de estado/enmienda apuntando al nuevo ADR y actualizar las menciones a los comandos vigentes para que no induzcan a usar los targets eliminados.

3. **Verificación:**
   - Correr `make check` en `/z/repos/pj-template/84-actualizar-docs-adrs-a-c` y verificar que pase 100% en verde.
   - Verificar con `rg` que no queden referencias vigentes obsoletas a `agy-(pm|wk|rv|os)`, `cursor-(pm|wk|rv|os)` ni `make (pm|wk|rv|os)` fuera de los tests de eliminación en `scripts/prueba-*.sh` y las notas históricas explícitas de reemplazo en los ADRs.

## Criterios de aceptación (del issue #84)
- La documentación del template y las plantillas de documentación para hijos no contienen referencias obsoletas a `make agy-pm`, `make cursor-pm` ni `make pm` como interfaz vigente.
- Queda documentado en ADR el reemplazo de la convención anterior por `a-*` / `c-*` en `--solo` y `make listo <target>`.
- `make check` en `pj-template` pasa en verde.
