# Plan — `robert-flo/HR-CLAVIS#39` (`ravn-git-bare-clone`)

## Identificación
- **Issue:** `robert-flo/HR-CLAVIS#39` (https://github.com/robert-flo/HR-CLAVIS/issues/39)
- **Parent spec:** `robert-flo/HR-CLAVIS#37` (`ravn-git-bare-clone: podar worktrees y ramas huérfanas al sincronizar`)
- **Depende de:** `robert-flo/HR-CLAVIS#38` (ya `CLOSED` y mergeado en PR #11 en `37-prune-orphan-worktrees-on-sync`)
- **Repo de código y PR:** `robert-flo/ravn-git-bare-clone`
- **Tipo:** Sub-issue de spec (el WK mergea su PR a la rama del spec tras CI verde y `code-review` limpio; no pasa por RV)
- **Rama base / destino del PR:** `37-prune-orphan-worktrees-on-sync` (`origin/37-prune-orphan-worktrees-on-sync` @ `8c80c5b`)
- **Rama de trabajo:** `issue/39-no-prune-completions-docs`
- **Worktree de trabajo:** `/z-home-repos/HR-CLAVIS/repos/ravn-git-bare-clone/39-no-prune-completions-docs`

## Qué construir
Exponer el flag `--no-prune` en `ravn-wt clone` y `ravn-wt sync` (incluyendo modo batch `--group`) en `robert-flo/ravn-git-bare-clone` (contra la rama del spec `37-prune-orphan-worktrees-on-sync`), de modo que el usuario pueda desactivar la poda automática de worktrees y ramas huérfanas manteniendo `git fetch --prune` y la creación de worktrees para ramas remotas nuevas.

Además:
1. Registrar `--no-prune` en el catálogo de comandos (`runtime/commands.tsv`) y en las completions de shell (`runtime/completion/git-bare-clone.bash` y `runtime/completion/_git-bare-clone`).
2. Documentar el comportamiento de poda por defecto, las guardas de seguridad (`dirty`, `locked`, `current-dir`), el flag `--no-prune` y el motivo de borrar ramas `[gone]` limpias con `-D` (compatibilidad con squash-merge) en:
   - `--help` (`runtime/scripts/clone`, `runtime/scripts/sync`, `runtime/scripts/help` según corresponda)
   - `README.md`
   - un comentario breve inline en la lógica de sync/prune (`runtime/lib/prune.sh` y/o `runtime/scripts/clone` / `runtime/scripts/sync`).
3. Añadir tests reales de integración y completions:
   - Test real de git en `tests/test-git-bare-clone-prune.sh` (y/o `tests/test-git-bare-clone-batch.sh` para `--group`) verificando que `--no-prune` en `clone`/`sync` (single-repo y `--group`) omite borrar worktrees y ramas huérfanas, mientras sigue ejecutando `fetch --prune` y creando worktrees para ramas remotas nuevas.
   - Tests en `tests/test-completion.sh` verificando que `--no-prune` aparece en el catálogo y en completions bash y zsh para `clone` y `sync`.

## Archivos relevantes a revisar
- `runtime/scripts/clone`
- `runtime/scripts/sync`
- `runtime/scripts/help`
- `runtime/lib/prune.sh`
- `runtime/commands.tsv`
- `runtime/completion/git-bare-clone.bash`
- `runtime/completion/_git-bare-clone`
- `README.md`
- `tests/test-git-bare-clone-prune.sh`
- `tests/test-git-bare-clone-batch.sh`
- `tests/test-completion.sh`
- `tests/run.sh`

## Criterios de aceptación (del issue #39)
- [ ] Pasar `--no-prune` a `ravn-wt sync` / `ravn-wt clone` (tanto single-repo como modo batch `--group`) omite borrar worktrees y ramas locales huérfanas, pero sigue ejecutando `fetch --prune` y creando worktrees para ramas remotas nuevas (verificado por test real de git en `make check`).
- [ ] `--no-prune` está listado en el catálogo de comandos para `clone` y `sync`, se muestra en `--help`, y está expuesto en completions de bash y zsh (verificado por tests de completion en `make check`).
- [ ] El comportamiento por defecto de poda, `--no-prune`, las guardas de seguridad (`dirty`, `locked`, `current-dir`) y la justificación de borrar ramas `[gone]` limpias con `-D` (compatibilidad con squash-merge) están documentados en `README.md`, `--help` y un comentario breve inline.
- [ ] `make check` (`shellcheck` + `tests/run.sh`) pasa completamente en verde y la salida se adjunta como evidencia en el body del PR contra `37-prune-orphan-worktrees-on-sync`.

## Reglas clave para el WK
- Trabajar **SOLO** en `/z-home-repos/HR-CLAVIS/repos/ravn-git-bare-clone/39-no-prune-completions-docs` (rama `issue/39-no-prune-completions-docs`).
- Respetar el estilo modular `git-setup` del repo (`shellcheck` limpio, `set -euo pipefail`).
- Sin trailers `Co-authored-by` ni atribución a agentes en commits. Identidad git ya configurada.
- Redactar el PR con la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), contra `--base 37-prune-orphan-worktrees-on-sync`, referenciando `Closes robert-flo/HR-CLAVIS#39` (o `Part of robert-flo/HR-CLAVIS#37` + cierre del `#39`).
- Correr `make check` y `code-review` limpio.
- Es un sub-issue: con CI verde y `code-review` limpio, mergear el PR a `37-prune-orphan-worktrees-on-sync` con:
  `gh pr merge <pr> -R robert-flo/ravn-git-bare-clone --squash --delete-branch --match-head-commit <sha-verde>`
- Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-git-bare-clone-39/wk.md` con: URL del PR, `head sha`, pruebas ejecutadas y `merge sha`.
