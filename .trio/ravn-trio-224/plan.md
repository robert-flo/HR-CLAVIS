# Plan del Spec #224 — ravn-trio: estado verdadero de agentes en fleet y trio-status (Spec 1)

- **Spec:** `robert-flo/HR-CLAVIS#224`
- **Repo destino:** `robert-flo/ravn-trio`
- **Rama del spec:** `224-fleet-estado-verdadero` (ya creada en `origin` desde `main`)
- **Rama base del PR final:** `main`
- **Bare repo:** `/z-home-repos/HR-CLAVIS/.bares/ravn-trio.git`
- **Worktrees:** `/z-home-repos/HR-CLAVIS/repos/ravn-trio/<n>-<slug>`

## Orden DAG de sub-issues

1. **#225** — `ravn-trio: extender trio-status y trio.conf con campos de observabilidad` (`ready-for-agent`, sin bloqueos)
   - Extender `trio-status` para aceptar `--owner`, `--branch`, `--last-commit`, `--note`, `--heartbeat`, `--worktree` y persistirlos en `.trio/<clave>/status.json` conservando claves previas (`estado`, `paso`, etc.).
   - Agregar `TRIO_STALE_MINUTES=15` y patrones de cuota por defecto en `trio.conf.example` y `trio_config`.
   - Criterios:
     - `tests/test-status.sh` verifica nuevos campos y preservación de claves previas.
     - `tests/test-config-capas.sh` verifica `TRIO_STALE_MINUTES` (default `15`) y patrones de cuota.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa en verde.

2. **#226** — `ravn-trio: cruce herdr y worktree git en pj-trio fleet` (`ready-for-agent`, bloqueado por `#225`)
   - Hacer que `pj-trio fleet` (tabla y `--json`) cruce `herdr` y el worktree `git` contra `status.json` (solo como pista):
     - Inspeccionar en `herdr` el agente correspondiente al `paso` activo (`wk`, `rv`, `pm`, `os`), estado del proceso/panel y últimas líneas para detectar errores de cuota/rate-limit (`sin-cuota`).
     - Inspeccionar el worktree `git` (desde `worktree` en `status.json` o resolviendo ruta canónica): rama, commits locales y antigüedad del último commit.
     - Calcular `estado_real` (`vivo`, `sin-cuota`, `colgado` tras `TRIO_STALE_MINUTES`, `muerto`, `trabado`) y el `step` local inferido (`worktree`, `commits`), exponiendo `estado_real`, `step`, `owner`, `heartbeat_at` y `note` en `--json` y en la tabla.
   - Criterios:
     - `tests/test-fleet.sh` con stubs de `herdr` y worktree `git` temporal verifica `estado_real` para `vivo`, `sin-cuota`, `colgado`, `muerto` y `trabado`.
     - Tabla legible de `pj-trio fleet` muestra estado real y señal/paso sin romper `--activos`.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa en verde.

3. **#227** — `ravn-trio: cruce GitHub (gh) y flag --no-gh en pj-trio fleet` (`ready-for-agent`, bloqueado por `#226`)
   - Completar la jerarquía de verdad en `pj-trio fleet` agregando consulta a GitHub (`gh`):
     - Para tickets no confirmados como `hecho` (y sin `--no-gh`), consultar `gh` para verificar PR abierto/mergeado, estado de review/checks y estado del issue.
     - Confirmar `estado_real = terminado` cuando el cierre cuadra en GitHub y completar la inferencia de `step` (`pr-abierto`, `en-review`, `mergeado`).
     - Soportar `--no-gh` en `pj-trio fleet` para saltar consultas de red en modo rápido/offline.
   - Criterios:
     - `tests/test-fleet.sh` con stubs de `gh` confirma `terminado` y `step` (`pr-abierto`, `en-review`, `mergeado`), y omite `gh` en tickets ya cerrados o con `--no-gh`.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa completo en verde.

## Nota de versionado y PR final
- Revisar `AGENTS.md` de `ravn-trio` (`make lint`, `make test`, `herdr-plugin.toml` si aplica nueva versión, o si el release se hace al final del spec).
- Cada sub-issue se implementa en su propio worktree salido de `origin/224-fleet-estado-verdadero` y se mergea por sub-PR a `224-fleet-estado-verdadero`.
- Al terminar todos los sub-issues, abrir el PR final de `224-fleet-estado-verdadero` hacia `main` (`gh pr create -R robert-flo/ravn-trio --base main`) con body `/pr` referenciando `robert-flo/HR-CLAVIS#224`.
