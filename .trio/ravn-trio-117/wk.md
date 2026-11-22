# WK — robert-flo/HR-CLAVIS#117

- **PR:** https://github.com/robert-flo/ravn-trio/pull/61
- **Base:** `116-trio-por-spec-y-herdr-resiliente`
- **Head SHA:** `7bda882bb5d81da3e1f519d015c56a34304b56f7`
- **Merge SHA:** `61d8f0134f0d00527740e8c1cf7b3d1d3db350b0` (`MERGED` vía `--squash --delete-branch --match-head-commit`)

## Cambios implementados

- `runtime/scripts/start` y `runtime/commands.tsv`: flag `--spec` y autodetección vía `gh issue view ... --json labels` cuando el issue tiene etiqueta `spec`, manteniendo compatibilidad con tickets unitarios y `--base <rama>`.
- `runtime/lib/trio_config.sh`: `renderizar_kickoff` en modo `spec` instruye al PM a leer el spec y sub-issues, asegurar la rama del spec, ordenar `/implement-spec` al WK (con `/code-review` por sub-issue y merge a la rama del spec hasta abrir el PR final), pasar el PR final a RV, evaluar `Merge Danger` tras `APRUEBO <sha>` y cerrar el trío.
- `runtime/lib/trio_herdr.sh`: margen configurable `TRIO_PANE_SETTLE_SECS` (0.3s) antes de `herdr agent start`, reintento 1 vez ante `no longer owns the target terminal`, y captura de última salida con `herdr pane read` si vuelve a fallar.
- `trio-pm-prompt.md`: manejo explícito de `idle` sin `wk.md`/`rv.md` (verificar actividad con `herdr agent read` y `gh pr list` y re-esperar antes de contarlo como fallo y escalar a `needs-roberto`).
- `tests/test-modo-spec-y-blindaje.sh`: pruebas con mocks de `gh` y `herdr`.

## Pruebas verificadas

- `make lint`: `shellcheck -x` limpio en `pj-trio`, `trio-status`, `runtime/` y `tests/`.
- `make test`: las 18 suites en verde (incluyendo `test-modo-spec-y-blindaje.sh`, `test-base-spec.sh` y `test-pm-prompt-merge.sh`).
- `code-review`: ejecutado en dos ejes (Standards y Spec); corregida la colisión de `rama_spec` vs `--base` y simplificado el bucle de reintento en `trio_herdr.sh`.
