# Plan — robert-flo/HR-CLAVIS#98 (código en robert-flo/ravn-trio)

## Contexto y repositorios
- **Issue:** `robert-flo/HR-CLAVIS#98` (Parent spec: `robert-flo/HR-CLAVIS#97`)
- **Repo de código y PR:** `robert-flo/ravn-trio`
- **Worktree:** `/z/repos/ravn-trio/98-ticket-repo`
- **Rama:** `issue/98-ticket-repo`
- **Base:** `origin/HEAD` (`main`, actualmente en `d6b5caa`, `v0.1.16`)

## Objetivo
Soportar el flag opcional `--ticket-repo <owner/repo>` en `pj-trio` (`start`, `kickoff`, `--oneshot`) para separar el repositorio donde vive el issue (`ISSUE_REPO`) del repositorio donde viven el código y el PR (`PR_REPO`).

## Alcance y criterios de aceptación
1. **Validación estricta de `--ticket-repo <owner/repo>`:**
   - Debe cumplir estrictamente `^[A-Za-z0-9._-]+/[A-Za-z0-9._-]+$`.
   - Cualquier otro valor (ej. `solo-nombre`) debe fallar rápido en `stderr` con error claro, sin consultar disco, `herdr` ni red.
2. **Retrocompatibilidad:**
   - Si se omite `--ticket-repo`, mantener exactamente el comportamiento actual: `ISSUE_REPO == PR_REPO`.
3. **Plantillas de kickoff (`trio-pm-prompt.md` y `trio-os-prompt.md`):**
   - Introducir `{{ISSUE_REPO}}` (para `gh issue view` y referencia `{{ISSUE_REPO}}#{{TICKET}}`) y `{{PR_REPO}}` (para `gh pr create`, `gh pr merge` y `gh pr view`).
   - Cuando `--ticket-repo` difiere de `PR_REPO`, el encargo al WK debe indicar referenciar `{{ISSUE_REPO}}#{{TICKET}}` en el PR (en vez de `Closes #N` local que apuntaría al repo equivocado).
4. **Pruebas y calidad (`ravn-trio/AGENTS.md`):**
   - Cubrir en la suite `--dry-run` / `--print-kickoff`:
     - Caso con `--repo miembro --ticket-repo robert-flo/HR-CLAVIS`: `-R robert-flo/HR-CLAVIS` en `gh issue view` y `-R robert-flo/miembro` en `gh pr create` / `gh pr merge`.
     - Caso negativo `--ticket-repo solo-nombre`: falla rápido sin tocar `herdr` ni red.
     - Caso sin `--ticket-repo`: mantiene `ISSUE_REPO == PR_REPO`.
   - `make lint` (shellcheck) y `make test` en verde antes de abrir el PR.
   - Subir `version` en `herdr-plugin.toml` a `0.1.17` en el mismo PR (título `… · v0.1.17`).
   - Referenciar `robert-flo/HR-CLAVIS#98` en el cuerpo del PR (con `/pr`: `## Summary`, `## Evidence`, `## Merge Danger`).
