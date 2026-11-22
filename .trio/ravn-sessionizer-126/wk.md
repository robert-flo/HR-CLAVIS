# WK — robert-flo/HR-CLAVIS#126

- **PR:** https://github.com/robert-flo/ravn-sessionizer/pull/20 (`robert-flo/ravn-sessionizer#20`)
- **Rama:** `issue/126-selector-fzf-sesiones` → `main`
- **Head SHA:** `befb2284cb155712c8ee5caed77ba828cf46c68b`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/ravn-sessionizer/126-selector-fzf-sesiones`

## Pruebas ejecutadas
- `make lint` (`shellcheck -x ravn-sessionizer runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/completion/*.bash tests/*.sh tests/stubs/*`): OK (exit 0).
- `make test` (`bash tests/run.sh`): 11/11 suites en verde, incluyendo `tests/test-sessions.sh` (`ok test-sessions`: separación `———` Estables/Efímeras/`/z-home-repos/HR-*`, `ctrl-s` toggle/filtro activas, flujo `attached` vs `detached`, separador/ESC sin acción, preview de sesión).
- `sha256sum -c tests/shared-libs.sha256`: 5/5 OK (sin tocar librerías compartidas).
- Prueba real en máquina con `herdr` y `/z-home-repos/HR-*`:
  - `NO_COLOR=1 ./ravn-sessionizer sessions --list`: listó `default`, `scratchpad`, `———`, `HR-CLAVIS`, `HR-GIT-SETUP`, `HR-KODIGO-GRUPO-12`.
  - `NO_COLOR=1 ./ravn-sessionizer sessions --list --active`: filtró solo `default` (`running`).
