# RV: robert-flo/HR-CLAVIS#89 — Soportar `--kind <agy|cursor>` en `pj-trio`

- **PR:** https://github.com/robert-flo/ravn-trio/pull/59
- **Head SHA:** `c7332b2ad2b3e7746f5fd941b827c48f6302c861`
- **Base:** `main` (`robert-flo/ravn-trio`)
- **Spec padre:** https://github.com/robert-flo/HR-CLAVIS/issues/85
- **Veredicto:** `APRUEBO c7332b2ad2b3e7746f5fd941b827c48f6302c861`

## Verificación de criterios y reglas
1. **`--kind <agy|cursor>` y compatibilidad `agy`:**
   - `runtime/scripts/start` y `runtime/commands.tsv` agregan `--kind` (`agy` por defecto, sin `PJ_TRIO_KIND`) y validan valores permitidos.
   - `runtime/lib/trio_herdr.sh` valida `cursor-agent` (en `PATH` o vía `mise x cursor-agent`) antes de crear panes/workspaces.
   - `trio_construir_args_agente` comparte la construcción de argumentos entre `--dry-run` e `iniciar_agente_rol`: `--yolo --trust --workspace <repo> --add-dir <home-repo>`, con mención inline `@<home-repo>/.cursor/rules/{pm-trio,os}-<slug>.mdc` en `pm`/`os` y sin prompt posicional en `wk`/`rv`.
2. **`trio-pm-prompt.md` y placeholders:**
   - `runtime/lib/trio_config.sh` introduce `trio_cursor_rule_ref` y sustituye `{{WK_RULE_PREFIX}}` / `{{RV_RULE_PREFIX}}` (vacíos en `agy`, `@<home-repo>/.cursor/rules/{wk,rv}-<slug>.mdc ` en `cursor`), manteniendo `--timeout 600000` y verificación de `wk.md`/`rv.md`.
3. **Reglas del repo (`ravn-trio/AGENTS.md` y `comun.md`):**
   - `herdr-plugin.toml` sube a `0.1.16` y el título del PR incluye `· v0.1.16` para `release.yml`.
   - No se editaron los archivos copiados de `git-setup` (`presentation.sh`, `command_catalog.sh`, `command_interface.sh`, `dispatch.sh`, `lifecycle.sh`).
   - Commit único con identidad `Roberto Flores <25asab015@ujmd.edu.sv>`, sin `Co-authored-by` ni trailers de agente, sin secretos ni imágenes.
   - CI de GitHub (`ShellCheck Lint`, `Test Suite`, `Test Installation Target`) en verde.

## Pruebas ejecutadas por el RV (worktree `/z/repos/ravn-trio/rv-59` sobre `c7332b2`)
- `make lint` (`shellcheck -x ...`): **OK** (0 errores).
- `make test` (`tests/run.sh`, 16 suites incluyendo `test-kind-cursor.sh` y `test-pm-prompt-merge.sh`): **All tests passed successfully!**.
- `TRIO_SHARE_DIR=/tmp/noexiste /z/repos/ravn-trio/rv-59/pj-trio --dry-run --kind cursor /z-home-repos/HR-CLAVIS 89 --repo ravn-trio` y `--oneshot --kind cursor`: argv verificado con `--yolo --trust --workspace "/z-home-repos/HR-CLAVIS/ravn-trio" --add-dir "/z-home-repos/HR-CLAVIS"` y rutas absolutas `@/z-home-repos/HR-CLAVIS/.cursor/rules/...`.
- `TRIO_SHARE_DIR=/tmp/noexiste /z/repos/ravn-trio/rv-59/pj-trio --print-kickoff --kind cursor /z-home-repos/HR-CLAVIS 89 --repo ravn-trio`: verificados prefijos `@/z-home-repos/HR-CLAVIS/.cursor/rules/{wk,rv}-hr-clavis.mdc` en `herdr agent prompt`, `--timeout 600000` y verificación de `wk.md`/`rv.md`.
