# Revisión RV — Spec #231 (`robert-flo/HR-CLAVIS#231`)

- **PR:** https://github.com/robert-flo/ravn-trio/pull/82 (`231-watch-estado-real-notify` → `main`)
- **Head revisado:** `b59d65a98a52f3f774c9f1fa739b054d4154f4ab`
- **Veredicto:** `APRUEBO b59d65a98a52f3f774c9f1fa739b054d4154f4ab`

## Cumplimiento de Spec y Plan (`plan.md`)

1. **Integración de `estado_real` y `.session` (#232):**
   - `runtime/scripts/watch` (`collect_snapshot`) reutiliza `runtime/scripts/fleet --json --no-gh` (con `--ticket`).
   - `runtime/lib/trio_project.sh` agrega `trio_herdr_cmd` y soporte de `session_name` en `trio_resolver_agente_herdr`; `runtime/scripts/fleet` cachea consultas por sesión y respeta `.session` de `status.json`.
   - `confirm_events_with_gh` consulta `gh` únicamente cuando `estado != "hecho"` y `estado_real` es `muerto` o `colgado` para confirmar si ya pasó a `terminado`.
   - `diff_snapshots` detecta transiciones de `estado_real` hacia `sin-cuota`, `colgado`, `muerto`, `terminado` y `trabado`, exponiendo `estado_real` y `prev_estado_real` en `--json` y en la salida legible.

2. **Alertas, `--no-notify` y `--loop` (#233):**
   - `notify_events` valida transición previa y envía notificaciones vía `herdr notification show` (`--sound request` en críticos, `--sound done` en `terminado`/`hecho`) con fallback a `notify-send` (`-u critical` / `-u normal`).
   - `--no-notify` silencia las alertas y `--loop` actualiza `baseline` para continuar vigilando hasta `--timeout` (exit `124`) o señal, preservando la salida al primer evento por defecto.
   - `runtime/commands.tsv` y `runtime/completion/pj-trio.bash` (vía `commands.tsv`) reflejan `--ticket` en `fleet` y `--loop`/`--no-notify` en `watch`.

3. **Reglas de `ravn-trio` y proyecto:**
   - Rama base correcta (`main`), PR mergeable, sin force-push.
   - `herdr-plugin.toml` actualizado a `0.1.23` y título del PR con `· v0.1.23`.
   - Commits con identidad `Roberto Flores <25ASAB015@ujmd.edu.sv>`, sin `Co-authored-by` ni trailers de agente.
   - Sin cambios en librerías compartidas de `git-setup`, sin imágenes en el repo ni secretos.

## Pruebas verificadas en worktree efímero `rv-82` (`b59d65a98a52f3f774c9f1fa739b054d4154f4ab`)

- `make lint` (`shellcheck -x ...`): limpio.
- `make test` (`bash tests/run.sh`, incluyendo `test-fleet.sh` y `test-watch.sh` casos `#12` y `#13`): `All tests passed successfully!`.
- `make install DESTDIR=/tmp/rv82-install-check`: limpio.
- CI en GitHub (`ShellCheck Lint`, `Test Installation Target`, `Test Suite`): 3/3 en verde.
