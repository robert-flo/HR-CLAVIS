# WK — robert-flo/pj-template#83

- **PR:** https://github.com/robert-flo/pj-template/pull/86
- **Base:** `81-redisenar-makefile-hijo`
- **Rama:** `issue/83-migrar-referencias-a-c` (borrada tras squash-merge)
- **Head SHA:** `471a6204d29a11e692a094ae9ef2248bd9963894`
- **Merge SHA:** `aecd1d9fef876150a557b7827fd63b28c394b78f`
- **Estado:** `MERGED` (sub-issue `#83` cerrado y comentario publicado en spec `#81`)

## Archivos modificados
- `plantillas/hijo/pj`
- `scripts/onboard.py`
- `scripts/nuevo-hijo.sh`
- `.agents/reglas/comun.md`
- `.agents/reglas/pm.md`
- `plantillas/cursor/pm.mdc`
- `.cursor/rules/pm-pj-template.mdc` (regenerado con `make generar`)
- `scripts/humo.py`
- `scripts/check.sh`

## Pruebas
- `make check` en `/z/repos/pj-template/83-migrar-referencias-a-c` → `check: todo bien` (incluyendo `prueba-agy.sh`, `prueba-cursor.sh`, `prueba-onboarding.sh`, `humo estático`, `sintaxis bash`, `YAML válido`).
- CI GitHub Actions (`check/check (pull_request)`): `✓` en 46s.
- `code-review` (2 ejes en paralelo):
  - **Standards:** limpio (0 violaciones, 0 smells).
  - **Spec:** sin errores funcionales; observación menor sobre `sin_targets_viejos` en `.agents/agents/` y `.cursor/agents/` descartada porque esas plantillas (`plantillas/agents/*` y `plantillas/cursor/{wk,rv,os}.md`) no referencian targets de `make`.
