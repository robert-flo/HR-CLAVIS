# WK — robert-flo/HR-CLAVIS#93

- **PR:** https://github.com/robert-flo/HR-CLAVIS/pull/107 (`draft`, base `master`, rama `issue/93-sync-molde-cursor`)
- **Head SHA:** `a6bdf10851fac20b1d3ff24d8c94bbe8a554b26e`
- **Worktree:** `/z/repos/HR-CLAVIS/93-sync-molde-cursor`

## Cambios aplicados

- `make -C /z/repos/HR-CLAVIS/93-sync-molde-cursor sync`:
  - `proyecto.yml`: `template_ref` `6ec365ee9b83065bf6ea70afed44f70a78a13b3f` → `188ca453a00c`
  - `pj`: actualizado desde `plantillas/hijo/pj`
  - `Makefile`: actualizado desde `plantillas/hijo/Makefile`

## Pruebas verificadas

1. **`make -C /z/repos/HR-CLAVIS/93-sync-molde-cursor sync` (`./pj check`)**:
   - `check: todo bien`
   - `ok   humo estático: PM/WK/RV/OS-HR-CLAVIS encuentran reglas, skills y adaptadores de Cursor`
   - `ok   skills-lock, .agents/skills y fichas cuadran; 17 skills requeridas visibles para agy y Cursor`
2. **Verificación de adaptadores Cursor en disco**:
   - `.cursor/agents/`: vacío (`0` archivos; eliminados `rv-hr-clavis.md` y `wk-hr-clavis.md`).
   - `.cursor/rules/`: presentes `pm-trio-hr-clavis.mdc`, `pm-hr-clavis.mdc`, `wk-hr-clavis.mdc`, `rv-hr-clavis.mdc`, `os-hr-clavis.mdc` y `no-cursor-coauthor.mdc`.
3. **`pj-trio --dry-run --kind cursor HR-CLAVIS 93`**:
   - Genera `herdr agent start` con `--yolo --trust --workspace "/z-home-repos/HR-CLAVIS" --add-dir "/z-home-repos/HR-CLAVIS"` y `@/z-home-repos/HR-CLAVIS/.cursor/rules/pm-trio-hr-clavis.mdc` para `pm-hr-clavis-93`, además de `wk-hr-clavis-93` y `rv-hr-clavis-93`.
