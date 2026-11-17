# WK — robert-flo/ravn-git-bare-clone#7

- **PR:** https://github.com/robert-flo/ravn-git-bare-clone/pull/8
- **Rama:** `issue/7-emojis-reales` -> `main`
- **Head SHA:** `a81d8ae92295e392e5dc88610cd12aa787da64d5`
- **Worktree:** `/z/repos/ravn-git-bare-clone/7-emojis-reales`

## Cambios realizados

- `runtime/lib/global_fn.sh`:
  - Separada la detección de colores ANSI (`[[ -t 1 && ${TERM:-dumb} != "dumb" && -z ${NO_COLOR:-} ]]`) de la de iconos UTF-8 (`[[ ${TERM:-} != "dumb" && -z ${NO_COLOR:-} ]]`).
  - Reemplazados los glifos Nerd Font (`ICON_KEY`, `ICON_LOCK`, `ICON_GIT`, `ICON_GITHUB`, `ICON_GEAR`, `ICON_ROCKET`, `ICON_PACKAGE`) por emojis UTF-8 reales (`🔑`, `🔒`, `🌿`, `🐙`, `⚙️`, `🚀`, `📦`), e incorporados `ICON_FOLDER` (`📁`), `ICON_BRANCH` (`🌿`), `ICON_CLONE` (`📥`), `ICON_PUSH` (`⬆️`), `ICON_ISSUE` (`🎫`) con fallback ASCII bajo `NO_COLOR` o `TERM=dumb`.
- `runtime/scripts/{clone,create,issue,help}`:
  - Eliminadas las redefiniciones locales con glifos Nerd Font (`󰉋`, `󰘾`, ``, `󰕒`, `󰑌`, `󰋖`); ahora reutilizan los `ICON_*` de `global_fn.sh`.
- `tests/test-icons.sh`:
  - Nuevo test que valida colores vacíos sin TTY, emojis UTF-8 reales sin TTY, y fallback ASCII con `NO_COLOR=1` o `TERM=dumb`.

## Pruebas

- `make check` en `/z/repos/ravn-git-bare-clone/7-emojis-reales`:
  - `make lint` (`shellcheck`): **PASS**
  - `make test` (`bash tests/run.sh`, 8/8 tests incluyendo `tests/test-icons.sh`): **PASS**
- Prueba directa sin TTY (`env -u NO_COLOR TERM=xterm-256color ./ravn-git-bare-clone help | head -n 5`): imprime `ℹ Help and Usage`.
- CI en GitHub Actions (PR #8):
  - `CI/ShellCheck Lint`: **PASS**
  - `CI/Test Suite`: **PASS**
  - `CI/Test Installation Target`: **FAIL preexistente en `origin/main`** (desde commit `3a93a63`, `Makefile` instala en `/opt/ravn-wt` pero `.github/workflows/ci.yml` en `main` sigue verificando `dest/opt/ravn-git-bare-clone/...`).
