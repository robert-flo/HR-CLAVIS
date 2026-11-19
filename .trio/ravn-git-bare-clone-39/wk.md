# WK — `robert-flo/HR-CLAVIS#39` (`ravn-git-bare-clone`)

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/39 (sub-issue de `#37`)
- **PR:** https://github.com/robert-flo/ravn-git-bare-clone/pull/12 (base: `37-prune-orphan-worktrees-on-sync`)
- **Head SHA:** `5e553d144eac961bc37ebf402aa4be4499e44ea8`
- **Merge SHA:** `e4217cd4b66c387dd2806e51c9191359b1736676` (`--squash --delete-branch --match-head-commit 5e553d144eac961bc37ebf402aa4be4499e44ea8`)

## Cambios
- `runtime/scripts/clone`: flag `--no-prune` en `parse_params` y `clone_repository` (single-repo y `--group`), actualización de `usage()` con comportamiento de poda, guardas (`dirty`, `locked`, `current-dir`), `--no-prune` y motivo de `git branch -D` (squash-merge), más comentario inline.
- `runtime/lib/prune.sh`: comentario inline documentando el uso de `git branch -D` sobre ramas `[gone]` limpias por compatibilidad con squash-merge.
- `runtime/scripts/help`: uso y ejemplos con `ravn-wt sync [--no-prune]`.
- `runtime/commands.tsv`: `--no-prune` registrado en `clone` y `sync` (consumido automáticamente por completions bash y zsh).
- `README.md`: documentación de poda por defecto, guardas de seguridad, justificación de `-D` (squash-merge), `--no-prune` y estructura actualizada.
- `tests/test-git-bare-clone-prune.sh`: tests reales de git para `sync --no-prune`, `clone --no-prune` y `--no-prune --group` (verificando `fetch --prune`, preservación de huérfanas y creación de nuevas ramas remotas).
- `tests/test-completion.sh`: verificación de `--no-prune` en bash/zsh completions para `clone` y `sync`, y de `--help`.

## Pruebas y revisión
- `make check` (`shellcheck` + `tests/run.sh`, 9/9 suites): **PASS**
- `make DESTDIR=<tmp> install` + verificación de binarios/completions/librerías (equivale al job `install` de `.github/workflows/ci.yml`, cuyo trigger es solo `main`): **PASS**
- `code-review` (Standards + Spec en paralelo): **limpio** (0 violaciones de estándares, 0 errores de implementación, 0 scope creep).
