# RV — robert-flo/ravn-git-bare-clone#7

- **PR:** https://github.com/robert-flo/ravn-git-bare-clone/pull/8
- **Head SHA revisado:** `a81d8ae92295e392e5dc88610cd12aa787da64d5`
- **Base:** `main`
- **Veredicto:** **APRUEBO** `a81d8ae92295e392e5dc88610cd12aa787da64d5`

## Verificación en worktree (`rv-8` sobre `origin/issue/7-emojis-reales`)

- **Rama y commit:** `a81d8ae92295e392e5dc88610cd12aa787da64d5` (`Roberto Flores <25asab015@ujmd.edu.sv>`), sin trailers `Co-authored-by` ni atribución de agentes, sin force-push, sin secretos ni imágenes commiteadas.
- **Diff vs ticket #7 y `plan.md`:**
  - `runtime/lib/global_fn.sh`: separa la detección de ANSI (`[[ -t 1 && ${TERM:-dumb} != "dumb" && -z ${NO_COLOR:-} ]]`) de la de iconos UTF-8 (`[[ ${TERM:-} != "dumb" && -z ${NO_COLOR:-} ]]`); reemplaza los glifos Nerd Font en `ICON_{KEY,LOCK,GIT,GITHUB,GEAR,ROCKET,PACKAGE}` por emojis UTF-8 reales y agrega `ICON_{FOLDER,BRANCH,CLONE,PUSH,ISSUE}` con fallback ASCII bajo `NO_COLOR` o `TERM=dumb`.
  - `runtime/scripts/{clone,create,issue,help}`: eliminan overrides locales con glifos Nerd Font y reutilizan `ICON_*` de `global_fn.sh`.
  - `tests/test-icons.sh`: valida el contrato sin TTY, con `NO_COLOR=1` y con `TERM=dumb`.
- **Pruebas ejecutadas en `/z/repos/ravn-git-bare-clone/rv-8`:**
  - `make check` (`shellcheck` + `bash tests/run.sh`, 8/8 suites): **PASS**.
  - `./ravn-git-bare-clone help` sin TTY (`TERM=xterm-256color`, sin `NO_COLOR`): imprime `ℹ Help and Usage`.
  - `./ravn-git-bare-clone help` con `NO_COLOR=1`: imprime `[INFO] Help and Usage`.
- **CI:** `ShellCheck Lint` y `Test Suite` en verde; `Test Installation Target` falla por desfase preexistente en `main` entre `Makefile` (`PREFIX ?= /opt/ravn-wt`) y `.github/workflows/ci.yml` (`dest/opt/ravn-git-bare-clone/...`), ajeno a este PR.
- **Limpieza:** worktree `/z/repos/ravn-git-bare-clone/rv-8` y rama local `rv/8` eliminados.
