# RV — robert-flo/ravn-trio#50

- **PR:** https://github.com/robert-flo/ravn-trio/pull/51
- **Head revisado:** `875475c7a4b928b5d50fee1a73a606556f285de6` (avanzó desde `c4b705bc2375891a681d2ca53ffff13849c730dd` con el commit `test: actualizar versión esperada a 0.1.12 en test-pm-prompt-merge.sh`)
- **Veredicto:** `APRUEBO 875475c7a4b928b5d50fee1a73a606556f285de6`

## Revisión contra ticket #50 y plan

- `runtime/scripts/watch`:
  - Agrega `--verbose` y actualiza `show_watch_help`.
  - Por defecto (`verbose=0`), `diff_snapshots` filtra eventos para despertar solo en `estado` -> `hecho` / `needs-roberto` (tanto en tickets existentes como nuevos) o `pm_herdr_status` -> `blocked` / `unknown` estando `en-curso`.
  - Actualiza `--snapshot` y `baseline` cuando hay cambios intermedios no accionables tanto en el chequeo inicial como dentro del loop, sin retornar.
  - Con `--verbose`, conserva la detección completa anterior (`nuevo`, `estado`, `paso`, `blocked`/`unknown`).
- `runtime/commands.tsv`: agrega `--verbose` a la entrada de `watch`.
- `herdr-plugin.toml`: sube `version` a `"0.1.12"`, y `tests/test-pm-prompt-merge.sh` actualiza la versión esperada a `0.1.12`.
- `tests/test-watch.sh`: cubre absorción de `plan -> wk -> rv` con actualización de `--snapshot`, despertar en `hecho`, `needs-roberto` (25 líneas de tail) y `blocked`, ticket nuevo en `en-curso` vs `hecho`, `--verbose` con alias `w`, salida legible y timeout `124`.

## Verificación (worktree `rv-51` sobre `875475c7a4b928b5d50fee1a73a606556f285de6`)

- `make lint` (`shellcheck -x ...`): OK (exit 0).
- `bash tests/test-watch.sh`: `OK watch` (exit 0).
- `bash tests/test-pm-prompt-merge.sh`: `OK prompt del PM: mergea tras APRUEBO sobre el head exacto` (exit 0).
- Suite completa (`tests/test-*.sh`): 13/13 restantes en verde; `test-pj-trio-dry-run.sh` falla localmente solo por tener `herdr` instalado en la máquina (prioriza `herdr notification show` sobre `notify-send`).
- CI en GitHub (`gh pr checks 51`): 3/3 checks en verde (`CI/ShellCheck Lint`, `CI/Test Installation Tarball`, `CI/Test Suite`).
- Commits con identidad `Roberto Flores <25asab015@ujmd.edu.sv>`, sin `Co-authored-by` ni trailers de agente, sin imágenes ni secretos, y PR con `Closes #50`, `## Summary`, `## Evidence` y `## Merge Danger` (`Door: two-way`, `Blast Radius: Low`).
