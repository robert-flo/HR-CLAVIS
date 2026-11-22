# WK — `robert-flo/pj-template#84`

- **Issue:** [#84](https://github.com/robert-flo/pj-template/issues/84) (sub-issue de [#81](https://github.com/robert-flo/pj-template/issues/81))
- **PR:** [#87](https://github.com/robert-flo/pj-template/pull/87) (`81-redisenar-makefile-hijo` ← `issue/84-actualizar-docs-adrs-a-c`)
- **Estado:** `MERGED`
- **Head SHA:** `dedfee31157ac8454a584b781b17ef06a4c8189d`
- **Merge SHA:** `7bf1bc6947860e05815887142b697a5e1b034c03`

## Cambios realizados
- Actualizados `README.md`, `AGENTS.md`, `docs/pj-template.md` y `plantillas/cursor/pm.mdc` (con regeneración de `.cursor/rules/pm-pj-template.mdc` vía `make generar`) a la convención simétrica `a-*` / `c-*` (`--solo`) y composición `make listo <target>`.
- Creado [docs/adr/0024-convencion-simetrica-a-c-y-listo.md](https://github.com/robert-flo/pj-template/blob/81-redisenar-makefile-hijo/docs/adr/0024-convencion-simetrica-a-c-y-listo.md).
- Actualizados `docs/adr/README.md` y los ADRs `0020`, `0021`, `0022`, `0023` con notas de enmienda y comandos vigentes.

## Pruebas y verificación
- `make check` en `/z/repos/pj-template/84-actualizar-docs-adrs-a-c`: `check: todo bien` (exit 0).
- `rg -n '(agy|cursor)-(pm|wk|rv|os)\b|make\s+(pm|wk|rv|os)\b'`: sin referencias vigentes obsoletas; solo coinciden los tests negativos en `scripts/prueba-{agy,cursor}.sh` y las notas históricas explícitas en `docs/adr/`.
- `code-review` (Standards y Spec): limpio, 0 violaciones duras, 0 requisitos faltantes, 0 scope creep.
- CI (`check/check (pull_request)` en PR #87): `✓` verde (`38s`).
