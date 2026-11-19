# Entrega WK — Ticket robert-flo/HR-CLAVIS#38 (`ravn-git-bare-clone`)

- **Issue:** `robert-flo/HR-CLAVIS#38` (sub-issue de `robert-flo/HR-CLAVIS#37`)
- **Repo:** `robert-flo/ravn-git-bare-clone`
- **Base:** `37-prune-orphan-worktrees-on-sync`
- **Rama:** `issue/38-prune-orphan-worktrees` (eliminada tras squash merge)
- **PR:** https://github.com/robert-flo/ravn-git-bare-clone/pull/11 (`MERGED`)
- **Head SHA:** `d1ed6fffb8bb5bf0eeb9b239cfb4e6076ab83e56`
- **Merge SHA:** `8c80c5bca94fb3052b3f9df672f9b4aab2181cb5`

## Cambios realizados
- `runtime/lib/prune.sh` (nuevo): módulo de poda segura (`cleanup_empty_parent_dirs`, `prune_orphan_worktrees`) independiente de locale usando `git for-each-ref --format='%(refname:short)%09%(upstream)'` + `git show-ref --verify --quiet` + `git worktree list --porcelain`. Protege worktrees `outside`, `locked`, `current-dir` y `dirty`, excluye `origin/HEAD` y ramas sin upstream, borra ramas squash-merged y bare-only con `branch -D`, limpia directorios padre vacíos estrictamente dentro de `$worktrees_dir` y cierra con `git worktree prune`.
- `runtime/scripts/clone`: pasa explícitamente `git fetch --prune origin`, integra el paso `[5/6] Pruning orphan worktrees and branches` cuando `bare_existed == 1`, e imprime resumen de huérfanos podados/saltados usando `global_fn.sh`.
- `Makefile`: instala `runtime/lib/prune.sh` en el target `install`.
- `tests/test-git-bare-clone-prune.sh` (nuevo): tests de integración reales con `mktemp -d` y `GIT_CONFIG_GLOBAL=/dev/null` cubriendo todos los criterios de aceptación (`merged-clean`, `squash-merged`, ramas con barra y directorios padre vacíos, `bare-only`, `stale-dir`, `dirty` modificado/untracked, `locked`, `current-dir`, `outside`, `local-only`, `origin/HEAD`, `active-keep` sin mover `HEAD`, `git worktree prune`, `ravn-wt sync` y `ravn-wt clone`).
- `tests/test-git-bare-clone-batch.sh`: actualiza contrato mock de `fetch origin` a `fetch --prune origin`.

## Pruebas verificadas
- **`make check`** (`shellcheck` + `tests/run.sh`): verde completo (9/9 suites pasando, incluida `test-git-bare-clone-prune.sh`).
- **`make DESTDIR=dest install`**: verde completo (`dest/opt/ravn-wt/runtime/lib/prune.sh` instalado correctamente) + `make clean`.
- **`code-review`** (Standards + Spec sobre `origin/37-prune-orphan-worktrees-on-sync...HEAD`): limpio, sin hallazgos pendientes.
- **CI remoto:** `.github/workflows/ci.yml` solo dispara `pull_request` sobre `main`; los 3 jobs equivalentes (`make lint`, `make test`, `make DESTDIR=dest install`) se ejecutaron y pasaron localmente.
