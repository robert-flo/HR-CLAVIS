# Plan — Spec #123 (`ravn-trio`)

- **Spec padre:** `robert-flo/HR-CLAVIS#123` — *herdr: 1 sesión persistente por spec, cierre por Orquestador y selector fzf*
- **Repo de código:** `robert-flo/ravn-trio`
- **Rama base del PR final:** `123-sesiones-herdr-por-spec`
- **Rama del spec (trabajo de `ravn-trio`):** `123-ravn-trio-sesiones-herdr` (creada desde `origin/123-sesiones-herdr-por-spec`)

## Alcance en `ravn-trio`

Del spec #123, dos sub-issues pertenecen a `robert-flo/ravn-trio`, en orden DAG estricto:

### 1. `robert-flo/HR-CLAVIS#125` — `ravn-trio: crear y usar 1 sesión herdr persistente por spec`
- **Dependencias:** ninguna.
- **Worktree:** `ravn-git-bare-clone` sobre `origin/123-ravn-trio-sesiones-herdr` (sub-PR hacia `123-ravn-trio-sesiones-herdr`).
- **Qué construir:**
  - Al despachar un spec en `ravn-trio` (`pj-trio <proyecto> <spec>`, sin `--oneshot`), derivar y sanitizar un nombre legible `#<N> - <título corto>` compatible con `herdr --session` (en `~/.config/herdr/sessions/<name>`), crear o reusar esa sesión persistente dedicada para el spec y registrar el nombre de sesión en `.trio/<clave>/status.json`.
  - Los triviales `--oneshot` siguen corriendo sin sesión persistente propia.
- **Criterios de aceptación:**
  - `pj-trio` en modo spec crea/usa una sesión `herdr` con formato `#<N> - <título corto>` sanitizado para `herdr --session` y la guarda en `.trio/<clave>/status.json`, verificado con tests CLI con mocks de `herdr` y `gh`.
  - `pj-trio --oneshot` no crea ni adjunta una sesión persistente propia de spec, verificado con test CLI.
  - `make lint` y `make test` pasan en verde y su salida se incluye en el PR.

### 2. `robert-flo/HR-CLAVIS#127` — `ravn-trio: comando de cierre de sesión controlado por el Orquestador tras verificar merge`
- **Dependencias:** bloqueado por `#125` (ejecutar tras mergear el sub-PR de `#125` en `123-ravn-trio-sesiones-herdr`).
- **Worktree:** `ravn-git-bare-clone` sobre `origin/123-ravn-trio-sesiones-herdr` actualizado (sub-PR hacia `123-ravn-trio-sesiones-herdr`).
- **Qué construir:**
  - Agregar en `ravn-trio` un subcomando para cerrar la sesión `herdr` de un spec controlado por el PM Orquestador: lee `.trio/<clave>/status.json` y verifica el estado del PR final en GitHub (`gh`).
  - Solo ejecuta `herdr session stop` y `herdr session delete` si el PR está mergeado y el spec no está en `needs-roberto`; si está en `needs-roberto` o sin merge, conserva la sesión viva y reporta el estado.
  - Asegurar también que el PM del trío cierre únicamente su workspace al terminar, sin eliminar la sesión.
- **Criterios de aceptación:**
  - El subcomando de cierre ejecuta `herdr session stop` + `delete` únicamente cuando el PR final en GitHub está mergeado y el estado no es `needs-roberto`, verificado con tests CLI con mocks de `herdr` y `gh`.
  - Ante `needs-roberto` o PR abierto/sin merge, el comando falla/rechaza el cierre y conserva viva la sesión `herdr`, verificado con test CLI.
  - El cierre del trío PM cierra su workspace sin borrar la sesión, y `make lint` + `make test` pasan en verde (salida en el PR).

## Reglas del repo (`ravn-trio/AGENTS.md`)
- Diseño modular estilo `git-setup`: launcher + `runtime/commands.tsv` + módulos en `runtime/scripts/`. No editar los archivos copiados de `runtime/lib/` (`presentation.sh`, `command_catalog.sh`, `command_interface.sh`, `dispatch.sh`, `lifecycle.sh`). Mensajes con `print_*`.
- `make lint` (shellcheck) y `make test` deben pasar antes de cada PR.
- Sin trailers `Co-authored-by` ni atribución a agentes en commits.

## PR final
Al mergear `#125` y `#127` en `123-ravn-trio-sesiones-herdr`, abrir PR final de `123-ravn-trio-sesiones-herdr` hacia `123-sesiones-herdr-por-spec` en `robert-flo/ravn-trio`, redactado con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `robert-flo/HR-CLAVIS#123`, y escribir `.trio/ravn-trio-123/wk.md`.
