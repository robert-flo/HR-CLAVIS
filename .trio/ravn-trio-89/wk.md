# WK: robert-flo/HR-CLAVIS#89 — Soportar `--kind <agy|cursor>` en `pj-trio`

- **PR:** https://github.com/robert-flo/ravn-trio/pull/59
- **Rama:** `issue/89-kind-cursor` → `main`
- **Head SHA:** `c7332b2ad2b3e7746f5fd941b827c48f6302c861`
- **Worktree:** `/z/repos/ravn-trio/89-kind-cursor`
- **Versión:** `0.1.16` en `herdr-plugin.toml`

## Cambios realizados
- `runtime/scripts/start` y `runtime/commands.tsv`: soporte y ayuda para `--kind <agy|cursor>` (default `agy`, sin `PJ_TRIO_KIND`).
- `runtime/lib/trio_herdr.sh`: validación de `cursor-agent` (en `PATH` o vía `mise x cursor-agent`) antes de abrir panes; helper `trio_construir_args_agente` compartido entre `--dry-run` y `iniciar_agente_rol` (`--yolo --trust --workspace <repo> --add-dir <home-repo>`, kickoff inline en `pm`/`os`, sin prompt posicional en `wk`/`rv`).
- `runtime/lib/trio_config.sh` y `trio-pm-prompt.md`: helper `trio_cursor_rule_ref` y placeholders `{{WK_RULE_PREFIX}}` / `{{RV_RULE_PREFIX}}` para anteponer `@<home-repo>/.cursor/rules/{wk,rv}-<slug>.mdc` en `herdr agent prompt` cuando el kind es `cursor`.
- `herdr-plugin.toml` y `tests/test-pm-prompt-merge.sh`: bump a `0.1.16`.
- `tests/test-kind-cursor.sh`: suite de pruebas para `--dry-run`, `--oneshot`, `--print-kickoff`, regresión `agy`, `--kind` inválido y validación de `cursor-agent`.

## Pruebas verificadas
- `make lint` (`shellcheck -x pj-trio trio-status runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/plugin/* runtime/completion/*.bash tests/*.sh`): **0 errores**.
- `make test` (`tests/run.sh`, 16 suites): **All tests passed successfully!**.
- `TRIO_SHARE_DIR=/tmp/noexiste ./pj-trio --dry-run --kind cursor /z-home-repos/HR-CLAVIS 89 --repo ravn-trio`: verificado argv exacto con `--yolo --trust --workspace`, `--add-dir` y `@/z-home-repos/HR-CLAVIS/.cursor/rules/pm-trio-hr-clavis.mdc`.
