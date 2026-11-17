# WK — robert-flo/ravn-trio#33

- **PR:** https://github.com/robert-flo/ravn-trio/pull/34
- **Head SHA:** `c15968c4f99a59c816394bb296a7f06d0f0ba069`
- **Rama:** `issue/33-smoke-prueba-1` → `main`
- **Worktree:** `/z/repos/ravn-trio/33-smoke-prueba-1`

## Cambios
- Creado `.smoke/prueba-1.txt` con contenido `prueba 1 ok`.

## Pruebas
- **Local (`make lint`):** falló con `shellcheck: command not found` (`Error 127`).
- **Local (`make test`):** 6 suites pasaron (`test-base-spec.sh`, `test-clonar-proyecto.sh`, `test-config-capas.sh`, `test-dispatch-package.sh`, `test-espera-agente.sh`, `test-install.sh`); `test-pj-trio-dry-run.sh` falló en línea 55 porque `herdr` está activo en `/usr/bin/herdr` y `trio-status --notify` usó `herdr notification show` en lugar del stub `notify-send`.
- **CI (PR #34):** los 3 checks pasaron en verde (`CI/ShellCheck Lint`, `CI/Test Installation Target`, `CI/Test Suite`).
