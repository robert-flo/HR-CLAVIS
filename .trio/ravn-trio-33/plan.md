# Plan — robert-flo/ravn-trio#33

- **Ticket:** `robert-flo/ravn-trio#33` (`smoke(herdr): prueba 1 trivial two-way + Low`)
- **Repo:** `robert-flo/ravn-trio`
- **Worktree:** `/z/repos/ravn-trio/33-smoke-prueba-1`
- **Rama:** `issue/33-smoke-prueba-1`
- **Base:** `origin/HEAD` (`main`)
- **Clasificación:** Trivial (`ready-for-agent`)

## Objetivo

Verificar el circuito completo de un ticket trivial con auto-merge (`Door: two-way` + `Blast Radius: Low`).

## Criterios de aceptación

1. Crear el archivo `.smoke/prueba-1.txt` en el worktree con el contenido `prueba 1 ok`.
2. Correr `make lint` y `make test` en el worktree y pegar el resultado en `## Evidence` del PR.
3. Abrir el PR contra la rama por defecto (`main`) redactado con la skill `pr` (`## Summary`, `## Evidence`, `## Merge Danger`), declarando exactamente:
   - `Door: two-way`
   - `Blast Radius: Low`
4. Al aprobar el RV (`APRUEBO <sha>`), el PM ejecuta auto-merge, limpia etiquetas y worktree, marca `status.json = hecho` y cierra el workspace en `herdr`.
