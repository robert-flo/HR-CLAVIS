# WK — robert-flo/ravn-trio#38

- **PR:** https://github.com/robert-flo/ravn-trio/pull/40
- **Base:** `37-smoke-spec-herdr`
- **Rama:** `issue/38-smoke-3a`
- **Head SHA:** `8e2c14fb387fa9b05245a23bf4b13f44c1d316a2`
- **Merge SHA:** `50fbb3962b5b98035ab796e5b0a1fa355039128f` (squash merge a `37-smoke-spec-herdr`)

## Pruebas
- `make lint` (con `/home/niri/.local/share/rvim/mason/bin` en `PATH` para `shellcheck`): pasó (exit 0).
- `make test`: pasaron `test-base-spec.sh`, `test-clonar-proyecto.sh`, `test-config-capas.sh`, `test-dispatch-package.sh`, `test-espera-agente.sh` y `test-install.sh`; `test-pj-trio-dry-run.sh` falló localmente porque `herdr` está activo en esta sesión (`herdr notification show` responde antes del fallback a `notify-send`).
- `code-review`: limpio en ambos ejes (Standards y Spec); identidad de commit verificada (`Roberto Flores <25asab015@ujmd.edu.sv>`).
- CI en GitHub Actions: `ci.yml` solo dispara en `main` (`no checks reported` en `37-smoke-spec-herdr`).
