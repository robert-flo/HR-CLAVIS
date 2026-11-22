# Plan — robert-flo/HR-CLAVIS#126

- **Ticket:** `robert-flo/HR-CLAVIS#126` (sub-issue de `robert-flo/HR-CLAVIS#123`)
- **Repo:** `robert-flo/ravn-sessionizer`
- **Base:** `origin/HEAD` (`main`)
- **Rama:** `issue/126-selector-fzf-sesiones`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/ravn-sessionizer/126-selector-fzf-sesiones`

## Objetivo
Implementar en `ravn-sessionizer` (modularizado al estilo `git-setup`) un selector `fzf` de sesiones inspirado en `t-smart-tmux-session-manager` (`bin/t`):
1. Separar visualmente con `———` las sesiones **Estables** (`HR-*`, `scratchpad`), las **Efímeras** (specs activos) y los directorios disponibles para crear nueva sesión listando exclusivamente `/z-home-repos/HR-*` (sin `zoxide` ni `find ~`).
2. Soportar `ctrl-s` dentro de `fzf` para recargar/filtrar sesiones activas.
3. Distinguir estado `attached` vs `detached` en `herdr` para hacer `attach`/switch o crear una nueva sesión según corresponda.

## Criterios de aceptación
- El selector `fzf` separa con `———` sesiones Estables, Efímeras y directorios de `/z-home-repos/HR-*` únicamente (sin `zoxide` ni `find ~`), verificado con test CLI usando `herdr` y `/z-home-repos/HR-*` mockeados.
- Soporta `ctrl-s` y flujo `attach` vs creación de nueva sesión según estado `attached`/`detached` en `herdr`, verificado con test CLI.
- `make lint` (`shellcheck`) y `make test` pasan en verde y su salida se incluye en el PR.

## Reglas del repo a respetar
- Seguir el diseño modular de `git-setup`: `runtime/commands.tsv` + módulo en `runtime/scripts/` + helpers propios en `runtime/lib/ses_*.sh`.
- **No editar** las librerías compartidas vigiladas por `tests/shared-libs.sha256` (`presentation.sh`, `command_catalog.sh`, `command_interface.sh`, `dispatch.sh`, `lifecycle.sh`).
- Usar `$HERDR_BIN_PATH` (CLI de `herdr`).
- Commits sin trailers `Co-authored-by` ni atribución a agentes.
- PR contra la rama por defecto (`main`) usando la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `robert-flo/HR-CLAVIS#126` en el body (no `Closes #126`).
