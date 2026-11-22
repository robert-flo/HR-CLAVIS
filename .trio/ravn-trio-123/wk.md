# Entrega WK — Spec `robert-flo/HR-CLAVIS#123` (`ravn-trio`)

- **PR final:** https://github.com/robert-flo/ravn-trio/pull/67 (`123-ravn-trio-sesiones-herdr` → `123-sesiones-herdr-por-spec`)
- **Head SHA:** `bd19b422ca84c271f6acf1fdf9bae42957cf5119`

## Sub-issues cerrados

1. **`robert-flo/HR-CLAVIS#125`** — `ravn-trio: crear y usar 1 sesión herdr persistente por spec`
   - Sub-PR: https://github.com/robert-flo/ravn-trio/pull/65 (mergeado en `123-ravn-trio-sesiones-herdr`)
   - Implementación:
     - `runtime/lib/trio_project.sh`: `trio_nombre_sesion_spec` deriva y sanea el nombre compatible con `herdr --session` (`[A-Za-z0-9._-]+`, ej. `123-herdr-1-sesion-persistente-por-spec`).
     - `runtime/lib/trio_herdr.sh` y `runtime/lib/trio_agy.sh`: `trio_asegurar_sesion_herdr` y `trio_herdr` para crear/reusar la sesión persistente en modo spec.
     - `runtime/scripts/start` y `runtime/scripts/status`: registran y conservan `.session` en `.trio/<clave>/status.json` en modo spec; `--oneshot` no crea ni adjunta sesión persistente.
     - `tests/test-sesion-spec.sh`: pruebas CLI con mocks de `herdr`, `gh` y `agy`.

2. **`robert-flo/HR-CLAVIS#127`** — `ravn-trio: comando de cierre de sesión controlado por el Orquestador tras verificar merge`
   - Sub-PR: https://github.com/robert-flo/ravn-trio/pull/66 (mergeado en `123-ravn-trio-sesiones-herdr`)
   - Implementación:
     - `runtime/scripts/close` y `runtime/commands.tsv`: nuevo subcomando `pj-trio close` que valida `status.json` (`estado != needs-roberto`, `.session`, `.pr`) y consulta `gh pr view` (`state == MERGED`) antes de ejecutar `herdr session stop` y `herdr session delete`.
     - `trio-pm-prompt.md`: aclara que el PM del trío ejecuta únicamente `herdr workspace close <workspace_id>` sin detener ni eliminar la sesión del spec.
     - `tests/test-close-sesion.sh`: pruebas CLI con mocks de `herdr` y `gh` (cierre tras merge, rechazo en `needs-roberto`, `OPEN` y `CLOSED` sin merge, y verificación de `trio-pm-prompt.md`).

## Pruebas verificadas

- `make lint` (`shellcheck -x pj-trio trio-status runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/plugin/* runtime/completion/*.bash tests/*.sh`): **0 errores**.
- `make test` (`bash tests/run.sh`): **21/21 suites en verde** (`All tests passed successfully!`), incluyendo `test-sesion-spec.sh` y `test-close-sesion.sh`.
- `/code-review` (ejes Standards y Spec): limpio.
