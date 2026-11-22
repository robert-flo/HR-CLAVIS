# Plan — robert-flo/HR-CLAVIS#91 (en robert-flo/pj-template)

- **Ticket:** `robert-flo/HR-CLAVIS#91` (Parent: `robert-flo/HR-CLAVIS#85`)
- **Repo objetivo:** `robert-flo/pj-template`
- **Worktree:** `/z/repos/pj-template/91-cursor-pm-trio`
- **Rama:** `issue/91-cursor-pm-trio`
- **Base:** `origin/HEAD` (`main`) — el PR va contra `main`.

## Qué construir
Actualizar los generadores, plantillas, reglas y pruebas de `pj-template` para que Cursor CLI opere sobre la arquitectura de dos niveles vía `herdr` (`--kind cursor`):

1. **`.cursor/rules/pm-<slug>.mdc`** (PM Orquestador):
   - Dedicado exclusivamente al PM Orquestador (saludo inicial, no codifica).
   - Despacha `pj-trio --kind cursor` (y `pj-trio --oneshot --kind cursor` en triviales).
   - Supervisa con `pj-trio watch <PROYECTO> --ticket <clave>`.
2. **`.cursor/rules/pm-trio-<slug>.mdc`** (PM efímero en `herdr`):
   - Nueva regla generada para el PM efímero en `herdr`.
   - Sigue `kickoff.md`, coordina `wk` y `rv` con `herdr agent prompt` y `herdr agent wait --timeout 600000`, verifica `wk.md` / `rv.md` y aplica Merge Gate (`## Merge Danger`).
3. **Eliminar `.cursor/agents/{wk,rv,os}-<slug>.md`**:
   - Dejar de generar archivos en `.cursor/agents/` (sobrantes limpiados vía `.agents/generados.txt` al correr `./pj`).
   - Mantener `.agents/agents/*` intactos para `agy --agent`.
4. **Reglas comunes y suite `make check`**:
   - Actualizar `comun.md` y `pm.md` (despacho de Cursor vía `pj-trio --kind cursor` / `--oneshot --kind cursor`, sin subagentes in-chat en `.cursor/agents/`).
   - Actualizar `humo.py`, `prueba-cursor.sh` (y `check.sh` si aplica) para exigir `pm-<slug>.mdc` y `pm-trio-<slug>.mdc` y rechazar subagentes in-chat en `.cursor/agents/`.
   - Mantener intactas las aserciones de `make c-pm`, `make a-pm` y `make listo <target>`.

## Criterios de aceptación
- [ ] El generador produce `pm-<slug>.mdc` y `pm-trio-<slug>.mdc` y no genera archivos en `.cursor/agents/`.
- [ ] `pm-<slug>.mdc` despacha `pj-trio --kind cursor` (y `--oneshot --kind cursor` en triviales) y supervisa con `pj-trio watch`.
- [ ] `humo.py`, `prueba-cursor.sh` y `make check` pasan en verde.
- [ ] El PR contra `main` de `robert-flo/pj-template` incluye `## Summary`, `## Evidence` y `## Merge Danger` (skill `pr`), sin trailers de agente en commits.
