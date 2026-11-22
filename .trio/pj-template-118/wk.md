# Entrega WK — robert-flo/HR-CLAVIS#118

- **PR:** https://github.com/robert-flo/pj-template/pull/92 (`robert-flo/pj-template#92`)
- **Base:** `116-trio-por-spec-y-herdr-resiliente`
- **Head SHA:** `13a41b7b964895b0ba7dbe9891664eab3240500c`
- **Merge SHA:** `ced84d075e30e39af3ad2e4389953c361bde63b3` (squash merge en `116-trio-por-spec-y-herdr-resiliente`, rama `issue/118-pm-md-trio-por-spec` y worktree eliminados)
- **Issue:** `robert-flo/HR-CLAVIS#118` cerrado sin etiquetas de triage; comentario publicado en `robert-flo/HR-CLAVIS#116`.

## Cambios realizados

- `.agents/reglas/pm.md`: restaurado el flujo **1 trío por spec** (el PM Orquestador despacha 1 `pj-trio` por spec completo en Engineering o `--oneshot` en triviales y supervisa con `pj-trio watch --ticket`; el PM del trío orquesta `/implement-spec` con el WK, abre el PR final, lanza al RV en `herdr`, rebota `BLOQUEO` y ejecuta el merge final según `Merge Danger`; eliminadas las instrucciones de despacho por sub-issue en fronteras DAG e `invoke_subagent`).
- `.agents/reglas/comun.md`: alineado el ejemplo de despacho Engineering en Cursor a `<spec>` (1 trío por spec completo).
- `plantillas/cursor/pm.mdc` y `plantillas/cursor/pm-trio.mdc`: alineados al flujo 1 trío por spec y `/implement-spec` dentro de `herdr`.
- `plantillas/agents/PM.md` y `scripts/generar.py`: eliminadas las referencias residuales a lanzar WK/RV como subagentes o con `invoke_subagent`.
- `docs/adr/0021-flujo-herdr-y-politica-merge-danger.md`: enmendado para reflejar 1 trío por spec (`HR-CLAVIS#116/#118`).
- Regenerados con `make generar`: `.agents/agents/{PM,WK,RV,OS}-pj-template.md` y `.cursor/rules/{pm,pm-trio}-pj-template.mdc`.

## Pruebas

- `make generar`: regeneración limpia de fichas y reglas Cursor.
- `bash scripts/check.sh`: `check: todo bien` (exit code 0).
- `code-review` (Standards y Spec): hallazgos iniciales atendidos en `13a41b7b964895b0ba7dbe9891664eab3240500c`.
- CI GitHub (`check/check (pull_request)`): verde en 49s sobre `13a41b7b964895b0ba7dbe9891664eab3240500c`.
