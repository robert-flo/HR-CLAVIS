# Plan — robert-flo/ravn-trio#39

- **Ticket:** `robert-flo/ravn-trio#39` — `smoke(herdr): sub-issue 3b del spec #37`
- **Spec padre:** `#37`
- **Repo:** `robert-flo/ravn-trio`
- **Rama base (PR target):** `37-smoke-spec-herdr` (`origin/37-smoke-spec-herdr`)
- **Rama de trabajo:** `issue/39-smoke-3b`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b`

## Criterios de aceptación
1. Crear `.smoke/prueba-3b.txt` con contenido `prueba 3b ok`.
2. Correr `make lint` y `make test` en `/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b` y pegar el resultado completo en `## Evidence` del PR.
3. Abrir PR contra `37-smoke-spec-herdr` (`gh pr create -R robert-flo/ravn-trio --base '37-smoke-spec-herdr'`) con secciones `## Summary`, `## Evidence` y `## Merge Danger`.
4. Con CI verde y `code-review` limpio, mergear el PR a `37-smoke-spec-herdr`:
   `gh pr merge <pr> -R robert-flo/ravn-trio --squash --delete-branch --match-head-commit <sha-verde>`

## Reglas para el WK
- Trabajar **solo** en `/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b`.
- Commits sin trailers `Co-authored-by` ni atribución a agentes.
- Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-39/wk.md` con:
  - URL del PR
  - Head SHA del PR antes del merge
  - Resultado de `make lint` y `make test`
  - SHA del merge en `37-smoke-spec-herdr`
