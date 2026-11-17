# WK — robert-flo/ravn-trio#44

- **PR:** https://github.com/robert-flo/ravn-trio/pull/46 (base `42-fleet-watch`, estado: `MERGED`)
- **Head SHA:** `3c7c8f7ebd3d888944f84e86c4ec3c8ead132d0f`
- **Merge SHA:** `9ddedf9a8e0ed1f59a94fbb14031f80c2dc059b8`
- **Spec:** `https://github.com/robert-flo/ravn-trio/issues/42#issuecomment-6954132238`

## Qué se implementó
- `runtime/scripts/watch`: subcomando `pj-trio watch <proyecto> [--snapshot <archivo>] [--timeout <seg>] [--json]`.
- `runtime/commands.tsv`: registrado `watch` con alias `w`.
- `herdr-plugin.toml`: bump de `version = "0.1.10"` a `"0.1.11"`.
- `tests/test-watch.sh`: pruebas con stubs de `herdr` e `inotifywait` para diff contra `--snapshot`, cambio de `paso` (y alias `w`), `needs-roberto` (últimas 25 líneas de `herdr agent read`), `blocked` en loop, salida legible y `timeout` (`exit 124`).
- `tests/test-pm-prompt-merge.sh`: actualizada la versión esperada en `herdr-plugin.toml` a `0.1.11`.

## Pruebas
- `make lint`: en verde (`shellcheck -x ...`).
- `bash tests/test-watch.sh`: `OK watch`.
- `bash tests/test-pm-prompt-merge.sh`: `OK prompt del PM: mergea tras APRUEBO sobre el head exacto`.
- `make DESTDIR=dest install && make clean`: en verde.
- Nota: `make test` global falla en `tests/test-pj-trio-dry-run.sh` en esta máquina porque `/usr/bin/herdr` está instalado y `pj-trio` usa `herdr notification show` en lugar de `notify-send`.
