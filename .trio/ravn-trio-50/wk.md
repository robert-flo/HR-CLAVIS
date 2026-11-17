# WK — robert-flo/ravn-trio#50

- **PR:** https://github.com/robert-flo/ravn-trio/pull/51
- **Rama:** `issue/50-watch-accionables` -> `main`
- **Head SHA:** `875475c7a4b928b5d50fee1a73a606556f285de6`
- **Worktree:** `/z/repos/ravn-trio/50-watch-accionables`

## Cambios realizados

- `runtime/scripts/watch`: se agregó `--verbose` (y se actualizó `show_watch_help`); por defecto `diff_snapshots` solo despierta ante `estado` -> `hecho` / `needs-roberto` (incluyendo ticket nuevo ya en esos estados) o `pm_herdr_status` -> `blocked` / `unknown` estando `en-curso`, actualizando `--snapshot` y `baseline` cuando hay transiciones intermedias no accionables sin salir del loop.
- `runtime/commands.tsv`: se agregó `--verbose` a las opciones de `watch`.
- `herdr-plugin.toml`: se subió `version` a `"0.1.12"`.
- `tests/test-watch.sh`: se cubrió absorción de `plan -> wk -> rv` con actualización de `--snapshot`, despertar en `hecho`, `needs-roberto` (con `tail` de 25 líneas) y `blocked`, ticket nuevo en `en-curso` vs `hecho`, modo `--verbose` con alias `w`, salida legible y timeout `124`.
- `tests/test-pm-prompt-merge.sh`: se actualizó la versión esperada de `herdr-plugin.toml` a `"0.1.12"`.

## Pruebas

- `make -C /z/repos/ravn-trio/50-watch-accionables lint`: **OK** (`shellcheck -x ...`, exit 0).
- `bash /z/repos/ravn-trio/50-watch-accionables/tests/test-watch.sh`: **OK watch** (exit 0).
- `bash /z/repos/ravn-trio/50-watch-accionables/tests/test-pm-prompt-merge.sh`: **OK** (exit 0).
- `make -C /z/repos/ravn-trio/50-watch-accionables test`: falló en `test-pj-trio-dry-run.sh` en local porque `herdr` está instalado en esta máquina y `trio-status` prioriza `herdr notification show` sobre `notify-send`.
- **CI (GitHub Actions PR #51):** 3/3 en verde (`CI/ShellCheck Lint`, `CI/Test Installation Target`, `CI/Test Suite`).
