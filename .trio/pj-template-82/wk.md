# WK: robert-flo/pj-template#82

- **Ticket:** https://github.com/robert-flo/pj-template/issues/82 (sub-issue de #81)
- **PR:** https://github.com/robert-flo/pj-template/pull/85 (`MERGED` en `81-redisenar-makefile-hijo`)
- **Head SHA:** `cdfb8617ff7835ab612711504500bd1392f429f2`
- **Merge SHA:** `1702a79a2c300b1c61637fe3cda719f980b713d3`

## Archivos modificados
- `plantillas/hijo/Makefile`: targets simétricos `a-{pm,wk,rv,os}` y `c-{pm,wk,rv,os}` con `--solo $(ARGS)` por defecto, eliminación de `pm`, `os`, `agy-*` y `cursor-*`, y `make` (`help`) agrupado en 4 secciones (`Agentes — agy`, `Agentes — Cursor`, `Home Repo`, `Flota (pj-trio)`) con 15 targets operativos y descripciones autocontenidas.
- `scripts/prueba-agy.sh`: verificación de `make -n a-*`, eliminación de targets viejos, ejecución rápida sin `pull`/`listo`, `make listo a-pm` con actualización y argumentos.
- `scripts/prueba-cursor.sh`: verificación de `make == make help`, 4 secciones y 15 targets en orden, descripciones sin referencias cruzadas, `make -n c-*`, `fleet`/`watch` intactos, `make c-*` sin `pull`/`listo` y `make listo c-pm`.

## Pruebas
- `bash scripts/prueba-agy.sh` (local): exit 0, todos los checks `ok`.
- `bash scripts/prueba-cursor.sh` (local): exit 0, todos los checks `ok`.
- `make check` (local): exit 0 (`check: todo bien`, incluyendo `humo.py`, `prueba-onboarding.sh`, `prueba-agy.sh`, `prueba-cursor.sh`, `prueba-clonar-dry-run.sh`, sintaxis bash y YAML).
- CI (`check/check (pull_request)` en PR #85): verde (54s).
