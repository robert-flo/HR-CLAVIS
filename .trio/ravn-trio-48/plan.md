# Plan — robert-flo/ravn-trio#48

- **Issue:** [robert-flo/ravn-trio#48](https://github.com/robert-flo/ravn-trio/issues/48) — `chore: limpiar archivo temporal .smoke/prueba-1.txt`
- **Clasificación:** Trivial (`ready-for-agent`)
- **Worktree:** `/z/repos/ravn-trio/48-limpiar-smoke-prueba-1`
- **Rama:** `issue/48-limpiar-smoke-prueba-1`
- **Base / PR target:** `origin/HEAD` (`main`)

## Alcance
Eliminar el archivo temporal `.smoke/prueba-1.txt` creado durante la prueba sintética 1 (#33). Si el directorio `.smoke` queda vacío tras eliminar `.smoke/prueba-1.txt`, git lo dejará de rastrear automáticamente (no dejar archivos residuales).

## Criterios de aceptación
1. Eliminar `.smoke/prueba-1.txt` en `/z/repos/ravn-trio/48-limpiar-smoke-prueba-1`.
2. Correr `make lint` y `make test` en `/z/repos/ravn-trio/48-limpiar-smoke-prueba-1` y pegar el resultado real en `## Evidence` del PR.
3. Abrir el PR contra `main` (`gh pr create -R robert-flo/ravn-trio`) incluyendo `Closes #48` y las secciones `## Summary`, `## Evidence` y `## Merge Danger` declarando:
   - `Door: two-way`
   - `Blast Radius: Low`
4. No incluir trailers `Co-authored-by` ni atribución a agentes en commits. Identidad git del repo ya configurada (`Roberto Flores <25asab015@ujmd.edu.sv>`).

## Entrega del WK
Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-48/wk.md` con:
- URL del PR
- Head SHA del commit del PR
- Resultado de `make lint` y `make test`
