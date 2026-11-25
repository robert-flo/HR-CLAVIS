# Plan — Spec #231 (`robert-flo/HR-CLAVIS#231`)

- **Repo de código:** `robert-flo/ravn-trio`
- **Rama del spec:** `231-watch-estado-real-notify` (desde `origin/231-watch-estado-real-notify`, base de integración hacia `main` / `231-watch-estado-real-notify`)
- **Spec:** `robert-flo/HR-CLAVIS#231` — *ravn-trio: pj-trio watch con estado real y alertas notify-send (Spec 3)*

## Orden de sub-issues (DAG)

1. **`robert-flo/HR-CLAVIS#232`** — *ravn-trio: integrar estado_real de fleet y .session en pj-trio watch* (sin bloqueos)
   - Reutilizar `pj-trio fleet --json --no-gh` (o su lógica compartida) en cada tick rápido de `watch`, respetando `.session` de `status.json` al consultar `herdr`.
   - Consultar `gh` únicamente cuando sea necesario confirmar cierre (`hecho` / `terminado`) o una transición relevante.
   - Detectar transiciones de `estado_real` hacia `sin-cuota`, `colgado`, `muerto`, `terminado` o `trabado` como eventos accionables junto con `estado` / `pm_herdr_status`, exponiendo `estado_real` y `prev_estado_real` en `--json` y salida legible.
   - **Criterios:**
     - `pj-trio watch --json` detecta transiciones a `sin-cuota`, `colgado`, `muerto` y `terminado`, respeta `.session` al consultar `herdr` y expone `estado_real` / `prev_estado_real`; verificado en `tests/test-watch.sh`.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa en verde.

2. **`robert-flo/HR-CLAVIS#233`** — *ravn-trio: alertas notify-send, --loop y --no-notify en pj-trio watch* (bloqueado por `#232`)
   - Disparar notificación usando `herdr notification show` con fallback a `notify-send` (urgencia crítica en `sin-cuota`, `colgado`, `muerto`; normal/done en `terminado`) ante transiciones a esos estados.
   - Agregar `--no-notify` para silenciar alertas.
   - Mantener comportamiento por defecto (salir tras el primer lote accionable) y agregar `--loop` para actualizar el snapshot base y seguir vigilando hasta `--timeout` o señal.
   - **Criterios:**
     - `pj-trio watch` dispara `herdr notification show` o fallback `notify-send` en transiciones a `sin-cuota`, `colgado`, `muerto` y `terminado`, y las omite con `--no-notify`; verificado con stubs en `tests/test-watch.sh`.
     - `--loop` procesa múltiples eventos consecutivos sin salir en el primero, mientras el modo default sigue saliendo en el primer evento; verificado en `tests/test-watch.sh`.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa completo en verde.
