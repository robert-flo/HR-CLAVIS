# Entrega WK — Spec #231 (`robert-flo/HR-CLAVIS#231`)

- **PR final:** https://github.com/robert-flo/ravn-trio/pull/82 (`231-watch-estado-real-notify` → `main`)
- **Head SHA:** `b59d65a98a52f3f774c9f1fa739b054d4154f4ab`
- **Versión (`herdr-plugin.toml`):** `0.1.23`

## Sub-issues implementados y mergeados a `231-watch-estado-real-notify`

1. **`robert-flo/HR-CLAVIS#232`** — PR `robert-flo/ravn-trio#79` (`2ed91b1`)
   - `pj-trio watch` reutiliza `fleet --json --no-gh` (con `--ticket` y caché por `.session` en `fleet` y `trio_resolver_agente_herdr`).
   - Confirmación selectiva con `gh` solo en transiciones `muerto`/`colgado` para detectar `terminado`.
   - Detección de transiciones de `estado_real` hacia `sin-cuota`, `colgado`, `muerto`, `terminado` y `trabado`, exponiendo `estado_real` y `prev_estado_real` en `--json` y salida legible.
2. **`robert-flo/HR-CLAVIS#233`** — PR `robert-flo/ravn-trio#80` (`5bf61e0`)
   - Alertas vía `herdr notification show` (`--sound request` en críticos, `--sound done` en `terminado`/`hecho`) con fallback a `notify-send` (`-u critical` / `-u normal`).
   - `--no-notify` para silenciar alertas.
   - `--loop` para vigilancia continua actualizando `baseline` hasta `--timeout` o señal; modo por defecto intacto.
3. **Bump de versión** — PR `robert-flo/ravn-trio#81` (`ca05bfc`)
   - `herdr-plugin.toml`: `0.1.22` → `0.1.23`.

## Pruebas verificadas

- `make check` (`shellcheck -x` + `bash tests/run.sh`, incluyendo `tests/test-watch.sh` casos `#12` y `#13` y `tests/test-fleet.sh`): **All tests passed successfully!**
- Revisión `/code-review` (ejes Standards y Spec) ejecutada en ambos sub-issues con hallazgos corregidos antes del merge.
