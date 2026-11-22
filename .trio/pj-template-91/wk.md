# WK — robert-flo/HR-CLAVIS#91

- **PR:** https://github.com/robert-flo/pj-template/pull/89
- **Head SHA:** `467479659fd437290b46514064e4d39f317bf1cb`
- **Rama:** `issue/91-cursor-pm-trio` → `main`
- **Worktree:** `/z/repos/pj-template/91-cursor-pm-trio`

## Cambios aplicados
- `plantillas/cursor/pm.mdc`: dedicado exclusivamente al PM Orquestador en Cursor (`pj-trio --kind cursor`, `--oneshot --kind cursor`, `pj-trio watch`, sin subagentes in-chat).
- `plantillas/cursor/pm-trio.mdc`: nueva plantilla generada en `.cursor/rules/pm-trio-<slug>.mdc` para el PM efímero en `herdr` (`kickoff.md`, `herdr agent prompt`, `herdr agent wait --timeout 600000`, `wk.md`/`rv.md`, `Merge Danger`).
- `plantillas/cursor/{wk,rv,os}.md`, `scripts/generar.py`, `plantillas/hijo/pj`: dejan de generar `.cursor/agents/{wk,rv,os}-<slug>.md` (limpiados vía `.agents/generados.txt`) y actualizan las rutas de autochequeo a `.cursor/rules/`.
- `.agents/reglas/{comun,pm}.md`: actualizados al despacho en dos niveles vía `herdr` (`--kind cursor`).
- `scripts/humo.py` y `scripts/prueba-cursor.sh`: validan `.cursor/rules/{pm,pm-trio,wk,rv,os}-<slug>.mdc`, rechazan subagentes en `.cursor/agents/` y conservan las pruebas de `make c-pm`, `make a-pm` y `make listo`.

## Pruebas verificadas
- `make check` completo en `/z/repos/pj-template/91-cursor-pm-trio` sobre `467479659fd437290b46514064e4d39f317bf1cb`: `check: todo bien` (incluyendo `humo.py`, `prueba-cursor.sh`, `prueba-agy.sh`, `prueba-onboarding.sh`, `prueba-clonar-dry-run.sh`, idempotencia y modo dependencia).
