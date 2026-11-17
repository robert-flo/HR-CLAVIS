# Entrega WK — robert-flo/ravn-trio#43

- **PR:** https://github.com/robert-flo/ravn-trio/pull/45
- **Base:** `42-fleet-watch`
- **Head SHA:** `7ee3bfd54f0dbee2ab11b82289b86a799e2b15b2`
- **Merge SHA:** `310f950c37ba0e7f966a532a9edc6df93b0bbe1f` (squash merge a `42-fleet-watch`)
- **Spec #42:** comentado en https://github.com/robert-flo/ravn-trio/issues/42#issuecomment-6953749674

## Cambios
- `runtime/commands.tsv`: registrado `fleet` (alias `fl`).
- `runtime/scripts/fleet`: implementado `pj-trio fleet <proyecto> [--activos] [--json]`, resolviendo `<proyecto>` vía `lib/trio_project.sh`, leyendo `<proyecto>/.trio/*/status.json`, cruzando `herdr workspace list` y `herdr agent get <pm-agente>` con degradación limpia (`unavailable` / `cerrado`), y salida en tabla o `--json`.
- `tests/test-fleet.sh`: pruebas con stubs para tabla, alias `fl`, `--json`, `--activos`, degradación sin `herdr` y proyecto sin `.trio`.

## Pruebas verificadas
- `make lint`: `shellcheck` limpio (exit 0).
- `bash tests/test-fleet.sh`: `OK fleet` (exit 0).
- `make install DESTDIR=/tmp/ravn-trio-install-test`: exit 0 (`runtime/scripts/fleet` instalado con permisos `755`).
- Prueba real en `HR-CLAVIS`: `pj-trio fleet HR-CLAVIS` y `pj-trio fl HR-CLAVIS --activos --json` ejecutados contra `.trio/*/status.json` y `herdr` reales.
