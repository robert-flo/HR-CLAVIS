# Handoff WK — robert-flo/HR-CLAVIS#129

- **Ticket:** `robert-flo/HR-CLAVIS#129`
- **Repo:** `robert-flo/pj-template`
- **PR:** https://github.com/robert-flo/pj-template/pull/94
- **Rama:** `issue/129-pm-session-close-rule` → `main`
- **Head SHA:** `40ca444a368cb477ef48f126d1dedfb8281d87c5`
- **Worktree:** `/z/repos/pj-template/129-pm-session-close-rule`

## Cambios realizados
- [.agents/reglas/pm.md](file:///z/repos/pj-template/129-pm-session-close-rule/.agents/reglas/pm.md#L49): se agregó el bullet **Cierre de sesión `herdr` por spec** en §Tu flujo paso 8, estableciendo que el PM del trío cierra únicamente su workspace al concluir (sin eliminar la sesión `herdr`) y el PM Orquestador es el único propietario de cerrar y eliminar la sesión `herdr` tras verificar en GitHub que el PR final quedó mergeado (`gh pr view <M> --json state,mergedAt`), conservándola viva si queda en `needs-roberto` y avisando a Roberto.

## Pruebas y verificación
- `make check` en `/z/repos/pj-template/129-pm-session-close-rule` (post-rebase sobre `origin/main` `47ece79`): pasó en verde (`check: todo bien`, código de salida 0).
- `code-review` (subagentes paralelos Standards y Spec): ambos limpios, 0 hallazgos bloqueantes.
- CI en GitHub (`check/check (pull_request)`): pasó en verde (46s); PR marcado como listo para revisión (`gh pr ready`).
