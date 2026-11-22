# Plan — robert-flo/HR-CLAVIS#129

- **Ticket:** `robert-flo/HR-CLAVIS#129` (`pj-template: documentar regla de cierre de sesión por el PM Orquestador en pm.md`)
- **Repo:** `robert-flo/pj-template`
- **Worktree:** `/z/repos/pj-template/129-pm-session-close-rule`
- **Rama:** `issue/129-pm-session-close-rule`
- **Base:** `origin/HEAD` (`main`) — PR contra la rama por defecto (`main`)

## Contexto y objetivo
Actualizar la plantilla de reglas del PM en `pj-template` para documentar el ciclo de vida de la sesión `herdr` por spec:
1. El **PM del trío** cierra únicamente su workspace al concluir, sin eliminar la sesión `herdr`.
2. El **PM Orquestador** es el único propietario de cerrar y eliminar la sesión `herdr` tras verificar en GitHub que el PR final quedó mergeado (conservándola viva si el spec queda en `needs-roberto` y avisando a Roberto).

> Nota de ruta: el issue menciona `plantillas/reglas/pm.md`, pero en `pj-template` la fuente real de las reglas del PM (que `scripts/generar.py` copia en `COPIAS = [".agents/reglas", ...]`) es `.agents/reglas/pm.md`. Editá `.agents/reglas/pm.md` (y verificá si existe alguna referencia en `plantillas/` o `docs/` que deba mantenerse consistente).

## Criterios de aceptación
1. `.agents/reglas/pm.md` documenta la responsabilidad exclusiva del PM Orquestador para cerrar y eliminar la sesión `herdr` de un spec tras verificar en GitHub que el PR final quedó mergeado, y conservarla viva si queda en `needs-roberto` (avisando a Roberto), mientras que el PM del trío cierra únicamente su workspace al concluir.
2. `make check` en `pj-template` pasa en verde tras los cambios y su salida completa se incluye en el PR.

## Instrucciones de entrega para el WK
- Trabajá solo en `/z/repos/pj-template/129-pm-session-close-rule`.
- No uses trailers `Co-authored-by` ni atribución a agentes en commits.
- Corré `make check` y `code-review` antes de abrir el PR.
- Abrí el PR contra la rama por defecto (`main`) con `gh pr create -R robert-flo/pj-template`, redactado con la skill `pr` (`## Summary`, `## Evidence` con salida de `make check`, y `## Merge Danger`), referenciando `robert-flo/HR-CLAVIS#129` en el body (sin `Closes #129`).
- Escribí `/z-home-repos/HR-CLAVIS/.trio/pj-template-129/wk.md` con URL del PR, head SHA y resultado de las pruebas.
