# Plan — robert-flo/rf-dotfiles#43

- **Issue:** `robert-flo/rf-dotfiles#43` — `feat(packages): agregar herdr-app a .chezmoidata/packages.json`
- **Repo:** `robert-flo/rf-dotfiles`
- **Base:** `origin/HEAD` (`master`)
- **Worktree:** `/z/repos/rf-dotfiles/43-herdr-app`
- **Rama:** `issue/43-herdr-app`
- **Clasificación:** Trivial

## Objetivo
Agregar `herdr-app` (repo `omarchy`) al array `packages.official` de `.chezmoidata/packages.json` en orden alfabético (después de `helix` y antes de `hyprpicker`), para que `chezmoi` instale la app de escritorio de Herdr de forma declarativa.

## Criterios de aceptación y pruebas
1. `herdr-app` agregado en `.chezmoidata/packages.json` (`packages.official`, entre `helix` y `hyprpicker`).
2. `jq . .chezmoidata/packages.json >/dev/null` y `pacman -Si herdr-app` en verde.
3. `chezmoi diff` sin sorpresas (sin tocar `.chezmoiignore.tmpl` ni rutas hardcodeadas).
4. PR a `master` redactado con la skill `pr` (`## Summary`, `## Evidence`, `## Merge Danger` con `Door: two-way`, `Blast Radius: Low`).

## Notas para el WK
- Trabajar únicamente en `/z/repos/rf-dotfiles/43-herdr-app`.
- Commits sin trailers `Co-authored-by` ni atribución a agentes.
- Escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-43/wk.md` con URL del PR, head SHA y resultado de las pruebas.
