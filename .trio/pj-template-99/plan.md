# Plan — robert-flo/HR-CLAVIS#99 (PR en robert-flo/pj-template)

## Contexto y nota sobre ISSUE_REPO vs PR_REPO
- **Issue:** `robert-flo/HR-CLAVIS#99` (`[pj-template] Pasar --ticket-repo <home-repo> en instrucciones de despacho multi-repo de pj-trio`)
  - Parent spec: `robert-flo/HR-CLAVIS#97`
  - Bloqueante `#98`: `CLOSED` (desbloqueado)
- **PR / Código:** `robert-flo/pj-template` (el PR debe referenciar `Closes robert-flo/HR-CLAVIS#99` en el body).
- **Worktree:** `/z/repos/pj-template/99-ticket-repo-despacho`
- **Rama:** `issue/99-ticket-repo-despacho`
- **Base:** `origin/HEAD` (`main`)

## Qué hay que construir
Actualizar las reglas e instrucciones de despacho del molde (`comun.md`, `pm.md` y la regla del Orquestador `plantillas/cursor/pm.mdc`) para que, cuando un Home Repo (`HR-*`) despache un ticket hacia un repo miembro (`--repo <miembro>`), pase explícitamente `--ticket-repo <dueño/home-repo>` a `pj-trio`.

Archivos identificados en el molde:
- `.agents/reglas/comun.md`
- `.agents/reglas/pm.md`
- `plantillas/cursor/pm.mdc`

## Criterios de aceptación
1. Las reglas e instrucciones de despacho documentan e incluyen `--ticket-repo <dueño/home-repo>` al invocar `pj-trio` con `--repo <miembro>`.
2. `humo.py` y `make check` en `pj-template` pasan en verde.
3. El PR se abre contra `main` en `robert-flo/pj-template` con body redactado según `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`) y referencia a `robert-flo/HR-CLAVIS#99`.
