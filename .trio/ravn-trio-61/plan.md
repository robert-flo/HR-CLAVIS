# Plan — Ticket #61 (`robert-flo/HR-CLAVIS#61` → `robert-flo/ravn-trio`)

## Contexto
- **Issue:** `robert-flo/HR-CLAVIS#61` (https://github.com/robert-flo/HR-CLAVIS/issues/61)
- **Spec padre:** `robert-flo/HR-CLAVIS#58` (`pj-trio watch: filtrar eventos por --ticket (v0.1.14) y exigir su uso en pj-template`)
- **Tipo:** Sub-issue de spec (el WK mergea su PR directamente a la rama del spec tras CI verde y `code-review` limpio; no pasa por RV).
- **Repo de trabajo:** `robert-flo/ravn-trio`
- **Worktree:** `/z/repos/ravn-trio/61-watch-ticket-filter`
- **Rama del WK:** `issue/61-watch-ticket-filter`
- **Rama base del PR:** `58-watch-ticket-filter`

## Qué implementar
Agregar soporte nativo de filtrado por ticket (`--ticket`) en `pj-trio watch`, actualizar ayuda y autocompletado, cubrir con tests automatizados en `tests/` y subir la versión a `0.1.14` en `herdr-plugin.toml`.

### Archivos objetivo
- `runtime/scripts/watch`
- `runtime/completion/pj-trio.bash`
- `tests/test-watch.sh` (y otros tests relevantes en `tests/` si aplica)
- `herdr-plugin.toml` (`version = "0.1.14"`)

### Restricciones del repo (`ravn-trio/AGENTS.md`)
- **NO editar** `runtime/lib/{presentation,command_catalog,command_interface,dispatch,lifecycle}.sh`.
- `make lint` (`shellcheck`) y `make test` deben pasar en verde antes del PR.
- Commits sin trailers ni atribución a agentes (`Co-authored-by`, etc.). Identidad git ya configurada en el repo.
- Título del PR debe incluir `· v0.1.14`.
- En el body del PR (redactado con skill `/pr`: `## Summary`, `## Evidence`, `## Merge Danger`), incluir `Closes robert-flo/HR-CLAVIS#61` y `Part of robert-flo/HR-CLAVIS#58`.

## Contrato funcional
1. **CLI:**
   - `--ticket <clave>` repetible (`--ticket a --ticket b`) y con listas separadas por comas (`--ticket a,b`).
   - Validar argumento vacío/faltante devolviendo error (`exit 1`).
   - Sin `--ticket`, conserva el comportamiento actual de flota completa.
2. **Matching:**
   - Un `.trio/<clave>/status.json` coincide si:
     - `<clave>` es igual al filtro, o
     - el campo `.ticket` del JSON coincide, o
     - el filtro es numérico (`^[0-9]+$`) y `<clave>` termina en `-<filtro>`.
3. **Filtrado temprano:**
   - `collect_snapshot()` debe omitir desde el inicio cualquier carpeta que no coincida con el filtro (sin parsear JSON ni invocar `herdr agent get` de tickets ajenos), teniendo en cuenta que si se necesita verificar `.ticket` cuando `<clave>` no matcheó directamente ni por sufijo numérico, se evite cualquier llamada innecesaria a `herdr agent get` en tickets ajenos.
4. **Race condition en arranque sin `--snapshot`:**
   - Cuando hay al menos un `--ticket` activo y NO se pasó `--snapshot`, si en el snapshot inicial alguno de los tickets filtrados ya está en estado accionable (`hecho`, `needs-roberto`, o `en-curso` con `pm_herdr_status` en `blocked`/`unknown`), `watch` debe emitir el evento (respetando texto / `--json`) y salir con `exit 0` de inmediato.
   - Sin `--ticket`, conserva el comportamiento actual (no despierta por histórico viejo).
5. **Ayuda y completion:**
   - Documentar `--ticket` en `show_watch_help` (`runtime/scripts/watch`).
   - Añadir `--ticket` en `runtime/completion/pj-trio.bash`.
6. **Bump de versión:**
   - Subir `version = "0.1.14"` en `herdr-plugin.toml`.

## Criterios de aceptación y verificación
- [ ] `pj-trio watch <proyecto> --ticket <clave>` ignora cambios en otros tickets y solo despierta ante eventos accionables del/los ticket(s) filtrados (test en `tests/` donde cambia otro ticket → `124` por timeout corto, y luego cambia el filtrado → `0` emitiendo solo ese evento en texto y `--json`).
- [ ] `--ticket` funciona repetido (`--ticket a --ticket b`), separado por comas (`--ticket a,b`), matchea por clave exacta (`rf-dotfiles-55`) y por número (`55`), y rechaza argumento vacío/faltante con `exit 1` (casos en `tests/` pasando en `make test`).
- [ ] Con `--ticket` y sin `--snapshot`, un ticket filtrado ya en `hecho` o `needs-roberto` al arrancar despierta de inmediato (`exit 0`), mientras que sin `--ticket` un ticket ya en `hecho` NO despierta (casos en `tests/` pasando en `make test`).
- [ ] `make lint` (`shellcheck`) y `make test` pasan completos en verde, con salida real en `## Evidence` del PR.
- [ ] `herdr-plugin.toml` actualizado a `version = "0.1.14"` y `runtime/completion/pj-trio.bash` incluye `--ticket` (verificado con diff y `grep` en `## Evidence`).
- [ ] PR abierto contra `58-watch-ticket-filter` en `robert-flo/ravn-trio` redactado con `/pr`, sin trailers de agente en commits, `code-review` limpio y CI verde.
- [ ] Como es sub-issue: con CI verde y `code-review` limpio, el WK mergea el PR a `58-watch-ticket-filter` con `gh pr merge <pr> -R robert-flo/ravn-trio --squash --delete-branch --match-head-commit <sha-verde>` y anota PR, head SHA, pruebas y merge SHA en `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-61/wk.md`.
