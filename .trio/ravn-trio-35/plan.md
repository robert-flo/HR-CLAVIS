# Plan — robert-flo/ravn-trio#35

## Ticket
- Issue: `robert-flo/ravn-trio#35` — `smoke(herdr): prueba 2 merge humano (Blast Radius: High)`
- Objetivo: verificar que cuando `## Merge Danger` declara `Blast Radius: High`, el PM no ejecuta auto-merge tras el `APRUEBO <sha>` del RV, sino que etiqueta el PR con `ready-to-merge`, marca `status.json = needs-roberto`, envía notificación y deja el workspace abierto.

## Worktree y rama
- Worktree: `/z/repos/ravn-trio/35-smoke-prueba-2`
- Rama: `issue/35-smoke-prueba-2`
- Base: `origin/HEAD` (`main`)

## Criterios de aceptación (para el WK)
1. Trabajar únicamente en `/z/repos/ravn-trio/35-smoke-prueba-2`.
2. Crear el archivo `.smoke/prueba-2.txt` con el contenido `prueba 2 ok`.
3. Correr `make lint` y `make test` y pegar el resultado en `## Evidence` del PR.
4. Abrir el PR contra `main` (`gh pr create -R robert-flo/ravn-trio`) usando el formato de la skill `pr` (`## Summary`, `## Evidence`, `## Merge Danger`), declarando exactamente:
   - `Door: two-way`
   - `Blast Radius: High`
5. Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-35/wk.md` con la URL del PR, el `head sha` y el resumen de las pruebas ejecutadas.

## Revisión y cierre
- El RV revisará el PR contra el ticket y este plan, escribirá `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-35/rv.md` y comentará en el PR terminando en `APRUEBO <sha>` o `BLOQUEO` con motivos.
- Tras `APRUEBO <sha>`, por `Blast Radius: High`, el PM aplicará el flujo de merge humano (sin auto-merge): etiqueta `ready-to-merge`, `status.json = needs-roberto` con `--notify`, commit + push del meta-repo y workspace abierto para Roberto.
