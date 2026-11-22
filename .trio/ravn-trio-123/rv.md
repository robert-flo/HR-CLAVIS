# Veredicto RV — Spec `robert-flo/HR-CLAVIS#123` (`ravn-trio`)

- **PR revisado:** https://github.com/robert-flo/ravn-trio/pull/67 (`123-ravn-trio-sesiones-herdr` → `123-sesiones-herdr-por-spec`)
- **Head SHA:** `bd19b422ca84c271f6acf1fdf9bae42957cf5119`
- **Veredicto:** `APRUEBO bd19b422ca84c271f6acf1fdf9bae42957cf5119`

## Revisión contra spec `#123` y `plan.md`

1. **Sesión `herdr` persistente por spec (`#125`):**
   - `runtime/lib/trio_project.sh` (`trio_nombre_sesion_spec`) deriva y sanea el nombre compatible con `herdr --session` (`[A-Za-z0-9._-]+`, ej. `123-herdr-1-sesion-persistente-por-spec`).
   - `runtime/lib/trio_herdr.sh` (`trio_asegurar_sesion_herdr`, `trio_herdr`) y `runtime/lib/trio_agy.sh` crean/reusan la sesión dedicada en modo spec y enrutan `herdr --session`.
   - `runtime/scripts/start` y `runtime/scripts/status` registran y conservan `.session` en `.trio/<clave>/status.json`.
   - `--oneshot` no crea ni adjunta sesión persistente propia.

2. **Cierre controlado por el Orquestador (`#127`):**
   - `runtime/scripts/close` y `runtime/commands.tsv` agregan `pj-trio close`, que valida `status.json` (`estado != needs-roberto`, `.session`, `.pr`) y consulta `gh pr view` exigiendo `state == "MERGED"` y `mergedAt` válido antes de ejecutar `herdr session stop` y `herdr session delete`.
   - Ante `needs-roberto`, `OPEN` o `CLOSED` sin merge, rechaza el cierre y conserva viva la sesión.
   - `trio-pm-prompt.md` instruye al PM del trío a ejecutar únicamente `herdr workspace close <workspace_id>` sin detener ni eliminar la sesión del spec.

## Reglas y estándares (`ravn-trio/AGENTS.md` + `HR-CLAVIS`)

- Diseño modular estilo `git-setup` respetado; `runtime/lib/{presentation,command_catalog,command_interface,dispatch,lifecycle}.sh` sin modificar; mensajes con `print_*`.
- Base correcta (`123-sesiones-herdr-por-spec`), historial limpio sin `Co-authored-by` ni trailers de agente, sin secretos ni imágenes commiteadas.

## Pruebas verificadas en worktree efímero `rv-67` (`bd19b422ca84c271f6acf1fdf9bae42957cf5119`)

- `make lint` (`shellcheck -x ...`): **0 errores**.
- `make test` (`bash tests/run.sh`): **21/21 suites en verde** (`All tests passed successfully!`), incluyendo `test-sesion-spec.sh` y `test-close-sesion.sh`.
- `pj-trio close --help`: verificado en CLI.
