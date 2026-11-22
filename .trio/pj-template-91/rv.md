# RV — robert-flo/HR-CLAVIS#91

- **PR:** https://github.com/robert-flo/pj-template/pull/89
- **Head SHA:** `467479659fd437290b46514064e4d39f317bf1cb`
- **Base:** `main`
- **Veredicto:** `APRUEBO 467479659fd437290b46514064e4d39f317bf1cb`

## Revisión contra ticket y plan
- `plantillas/cursor/pm.mdc` y `.cursor/rules/pm-pj-template.mdc`: dedicados exclusivamente al PM Orquestador; despachan con `pj-trio --kind cursor` / `pj-trio --oneshot --kind cursor`, supervisan con `pj-trio watch` y prohíben subagentes in-chat.
- `plantillas/cursor/pm-trio.mdc` y `.cursor/rules/pm-trio-pj-template.mdc`: nueva regla (`alwaysApply: false`, sin `description`) para el PM efímero en `herdr` con `kickoff.md`, `herdr agent prompt`, `herdr agent wait --timeout 600000`, `wk.md`/`rv.md` y `Merge Danger`.
- `scripts/generar.py`, `plantillas/cursor/{wk,rv,os}.md` y `plantillas/hijo/pj`: dejan de generar `.cursor/agents/{wk,rv,os}-<slug>.md` (limpiables vía `.agents/generados.txt` y `pj`), actualizan autochequeos a `.cursor/rules/{wk,rv,os}-<slug>.mdc`, y agregan `pm-trio` a `lanzar_cursor`.
- `.agents/reglas/{comun,pm}.md`: actualizados al despacho en dos niveles vía `herdr --kind cursor`.
- `scripts/humo.py` y `scripts/prueba-cursor.sh`: validan las 5 reglas en `.cursor/rules/`, rechazan archivos en `.cursor/agents/` y mantienen intactas las pruebas de `make c-pm`, `make a-pm` y `make listo`.
- Commit `467479659fd437290b46514064e4d39f317bf1cb` con identidad `Roberto Flores <25asab015@ujmd.edu.sv>`, sin trailers de agente, sin secretos ni imágenes commiteadas.

## Pruebas ejecutadas
- CI (`check/check` en `pull_request`): verde (47s).
- `bash /z/repos/pj-template/rv-89/scripts/prueba-cursor.sh`: verde.
- `cd /z/repos/pj-template/rv-89 && make check`: `check: todo bien` (exit 0).
