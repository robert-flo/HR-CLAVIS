# Entrega WK — robert-flo/ravn-trio#48

- **PR:** https://github.com/robert-flo/ravn-trio/pull/49 (listo para revisión, CI 3/3 verde)
- **Head SHA:** `abe895d2f9ae058ecd19088d55ed986c0512c970`
- **Rama:** `issue/48-limpiar-smoke-prueba-1` → `main`

## Pruebas en `/z/repos/ravn-trio/48-limpiar-smoke-prueba-1`

### `make lint`
- Estado: **verde** (exit 0)
- Comando: `shellcheck -x pj-trio trio-status runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/plugin/* runtime/completion/*.bash tests/*.sh`

### `make test`
- Estado local: **exit 1** en `test-pj-trio-dry-run.sh` (`FALLO: notify-send no se llamó`) porque `herdr notification show` responde en la sesión activa de `herdr` antes de llegar al fallback `notify-send`.
- Detalle por suite:
  - `test-base-spec.sh`: `OK base del spec`
  - `test-clonar-proyecto.sh`: `OK clonar proyecto`
  - `test-config-capas.sh`: `OK config por capas`
  - `test-dispatch-package.sh`: `ok test-dispatch-package`
  - `test-espera-agente.sh`: `OK espera de agente`
  - `test-install.sh`: `OK`
  - `test-pj-trio-dry-run.sh`: `FALLO: notify-send no se llamó` (6/7 suites OK localmente; sin cambios en archivos de código ni tests)
