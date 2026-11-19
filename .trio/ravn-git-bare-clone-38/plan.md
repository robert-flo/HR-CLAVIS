# Plan — Ticket robert-flo/HR-CLAVIS#38 (`ravn-git-bare-clone`)

## Contexto y metadatos
- **Issue:** `robert-flo/HR-CLAVIS#38` (vive en el tablero del Home Repo `robert-flo/HR-CLAVIS`)
- **Spec padre:** `robert-flo/HR-CLAVIS#37` (`ravn-git-bare-clone: podar worktrees y ramas huérfanas al sincronizar`)
- **Bloquea a:** `robert-flo/HR-CLAVIS#39` (`--no-prune`, completions y documentación — **fuera de alcance de este ticket**)
- **Repo de código:** `robert-flo/ravn-git-bare-clone`
- **Tipo:** sub-issue (contra rama del spec)
- **Rama base:** `37-prune-orphan-worktrees-on-sync` (`origin/37-prune-orphan-worktrees-on-sync`)
- **Rama de trabajo:** `issue/38-prune-orphan-worktrees`
- **Worktree de trabajo (ÚNICO donde trabaja WK):** `/z-home-repos/HR-CLAVIS/repos/ravn-git-bare-clone/38-prune-orphan-worktrees`
- **Handoff de entrega de WK:** `/z-home-repos/HR-CLAVIS/.trio/ravn-git-bare-clone-38/wk.md`

## Qué construir
Hacer que `ravn-wt clone` y `ravn-wt sync` (en `bin/git-bare-clone` / `runtime/scripts/clone` / `runtime/scripts/sync` / `runtime/lib/*` según la arquitectura modular del repo) ejecuten explícitamente `git fetch --prune origin` y, al actualizar un bare repository ya existente, poden automáticamente las ramas locales y worktrees huérfanos cuyo upstream `origin/*` dejó de existir, protegiendo trabajo en progreso y dejando intactas las ramas puramente locales y los worktrees activos.

### Comportamiento requerido
1. **Fetch explícito con prune:** pasar siempre `--prune` a `git fetch origin`, sin depender de la configuración global de Git (`fetch.prune`).
2. **Detección de huérfanas (independiente de locale):** identificar ramas locales (`refs/heads/*`) que tengan upstream bajo `refs/remotes/origin/*` cuya ref de tracking remoto ya no exista en el bare repo. Excluir la rama por defecto remota (`origin/HEAD`) y nunca tocar ramas sin upstream.
3. **Remoción segura:**
   - **Huérfana con worktree dentro de `$worktrees_dir`:** verificar que:
     - no esté `locked`,
     - no contenga el `$PWD` resuelto del invocador (`current-dir`),
     - tenga árbol de trabajo limpio (`git status --porcelain` vacío: sin staged, unstaged ni untracked).
     Si pasa todas las guardas: remover el worktree, borrar la rama local con `branch -D` (requerido para ramas squash-merged cuyo commit no es ancestro de `origin/HEAD`), y eliminar directorios padre vacíos que queden estrictamente dentro de `$worktrees_dir` (nunca borrar `$worktrees_dir`).
   - **Guardas / fuera de raíz:** si se dispara cualquier guarda o si el worktree vive fuera de `$worktrees_dir`, saltar tanto la remoción del worktree como el borrado de la rama e imprimir una advertencia clara con el nombre de la rama y el motivo (`dirty`, `locked`, `current-dir`, o fuera de `$worktrees_dir`).
   - **Huérfana sin worktree adjunto (bare-only):** borrar la rama local con `branch -D`.
   - **Limpieza administrativa:** terminar con `git worktree prune`.
4. **Worktrees activos intactos:** no hacer pull ni fast-forward al `HEAD` de worktrees cuya rama remota sigue existiendo.
5. **Resumen de salida:** imprimir un resumen claro de las ramas/worktrees podados y de los huérfanos saltados con su motivo, respetando el lenguaje visual del repo (`runtime/lib/global_fn.sh` / estilo `git-setup`).

## Fuera de alcance en #38
- Flag `--no-prune`, completions bash/zsh y actualización de `README.md` / catálogo de comandos (corresponden a `#39`).
- Fast-forward/pull de worktrees activos, `--dry-run`, o subcomando `prune` separado.

## Criterios de aceptación y pruebas
Todos deben quedar verificados con tests de integración contra repositorios Git temporales reales (`mktemp -d`) con `GIT_CONFIG_GLOBAL=/dev/null` dentro de `make check` (`shellcheck` + `tests/run.sh`):
- [ ] Re-ejecutar `ravn-wt sync` / `ravn-wt clone` sobre un bare existente con `GIT_CONFIG_GLOBAL=/dev/null` poda refs remotas `origin/*` borradas, remueve worktrees `[gone]` limpios (incluyendo una rama squash-merged cuyo commit no es ancestro de `origin/HEAD`), borra sus `refs/heads/*`, borra ramas locales `[gone]` sin worktree y ejecuta `git worktree prune`.
- [ ] Directorios padre vacíos dejados por ramas con barra (ej. `feat/...`, `fix/...`) se eliminan estrictamente dentro de `$worktrees_dir`, sin eliminar `$worktrees_dir`.
- [ ] Worktrees huérfanos `dirty` (archivos modificados o untracked), `locked`, o que contienen `$PWD` (`current-dir`) se saltan con advertencia visible nombrando la rama y el motivo, dejando archivos y rama local intactos.
- [ ] Ramas puramente locales (sin upstream), `origin/HEAD`, y el `HEAD` de worktrees activos nunca se borran ni se mueven.
- [ ] `make check` (`shellcheck` + `tests/run.sh`) pasa en verde completo y su salida queda adjunta como evidencia en el body del PR hacia `37-prune-orphan-worktrees-on-sync`.

## Flujo de entrega de WK
1. Trabajar **solo** en `/z-home-repos/HR-CLAVIS/repos/ravn-git-bare-clone/38-prune-orphan-worktrees` (rama `issue/38-prune-orphan-worktrees`).
2. Seguir el estilo modular `git-setup` del repo (`set -euo pipefail`, `shellcheck` limpio, sin trailers `Co-authored-by` ni atribución a agentes en commits).
3. Correr `make check` y `code-review`.
4. Abrir PR contra `37-prune-orphan-worktrees-on-sync`:
   `gh pr create -R robert-flo/ravn-git-bare-clone --base '37-prune-orphan-worktrees-on-sync' ...`
   Redactar el body con la skill `pr` (`## Summary`, `## Evidence`, `## Merge Danger`) referenciando `robert-flo/HR-CLAVIS#38` y `Part of robert-flo/HR-CLAVIS#37`.
5. Como es un sub-issue (ADR 0018), cuando CI esté verde y `code-review` limpio, WK mergea su propio PR a `37-prune-orphan-worktrees-on-sync` con:
   `gh pr merge <pr> -R robert-flo/ravn-git-bare-clone --squash --delete-branch --match-head-commit <sha-verde>`
6. Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-git-bare-clone-38/wk.md` con: URL del PR, `head` SHA, resumen de pruebas (`make check` + `code-review`) y `merge_sha` del squash merge.
