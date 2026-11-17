# Entrega WK — robert-flo/ravn-trio#35

- **PR:** https://github.com/robert-flo/ravn-trio/pull/36
- **Rama:** `issue/35-smoke-prueba-2` → `main`
- **Head SHA:** `1021849524223e057723401440979f9ed78bde60`
- **Archivo:** `.smoke/prueba-2.txt` (`prueba 2 ok`)

## Pruebas ejecutadas
- `make lint` (con `shellcheck` en `/home/niri/.local/share/rvim/mason/bin`): pasó (exit 0).
- `make test`:
  - `test-base-spec.sh`: OK
  - `test-clonar-proyecto.sh`: OK
  - `test-config-capas.sh`: OK
  - `test-dispatch-package.sh`: OK
  - `test-espera-agente.sh`: OK
  - `test-install.sh`: OK
  - `test-pj-trio-dry-run.sh`: falló en entorno `herdr` (`FALLO: notify-send no se llamó`) porque `trio-status` notificó por `herdr` (`cli:notification:show`) en lugar del stub `notify-send`.
- CI en GitHub (`gh pr checks 36 -R robert-flo/ravn-trio`): los 3 checks en verde (`CI/ShellCheck Lint`, `CI/Test Installation Tar...`, `CI/Test Suite`). PR marcado como ready for review.
