# Plan — robert-flo/ravn-git-bare-clone#7

- **Ticket:** [robert-flo/ravn-git-bare-clone#7](https://github.com/robert-flo/ravn-git-bare-clone/issues/7) — `fix(ui): usar emojis reales en ICON_* de global_fn.sh (incluso sin TTY)`
- **Repo:** `robert-flo/ravn-git-bare-clone`
- **Worktree:** `/z/repos/ravn-git-bare-clone/7-emojis-reales`
- **Rama:** `issue/7-emojis-reales`
- **Base:** `origin/HEAD` (`main`)
- **Tipo:** Trivial (PR directo contra `main`, revisión del RV, merge por PM)

## Alcance y criterios de aceptación

1. **Emojis UTF-8 reales en `runtime/lib/global_fn.sh`:**
   - Reemplazar los glifos Nerd Font (`ICON_KEY`, `ICON_LOCK`, `ICON_GIT`, `ICON_GITHUB`, `ICON_GEAR`, `ICON_ROCKET`, `ICON_PACKAGE`, y revisar si aplica a los demás `ICON_*` como `ICON_CHECK`, `ICON_CROSS`, `ICON_ARROW`, `ICON_WARN`, `ICON_INFO`) por emojis/símbolos UTF-8 reales (ej. `🚀`, `⚙️`, `📦`, `🔑`, `🔒`, etc.) para todos los comandos del repo (`git-bare-clone`, `git-create-worktree`, `git-issue-worktree`, `ravn-wt`, etc.).
   - Separar la detección de colores ANSI (`[[ -t 1 ]]`) de la de emojis UTF-8: los emojis deben mantenerse visibles incluso en pipes/subprocesos sin TTY, salvo que `NO_COLOR` esté definido o `TERM=dumb`.
   - Mantener `shellcheck` limpio y respetar la estructura modular existente.
2. **Pruebas y linters:**
   - Correr `make check` (`make lint` + `make test`) en el worktree `/z/repos/ravn-git-bare-clone/7-emojis-reales` y verificar que todo pase en verde.
   - Si aplica, agregar o actualizar un test en `tests/` que verifique que los iconos UTF-8 se emiten sin TTY y se desactivan con `NO_COLOR=1` / `TERM=dumb`.
3. **Versión y PR:**
   - Revisar si aplica subir versión según `Makefile` / `.github/workflows/` (`release.yml` si existe).
   - Abrir PR contra `main` con `gh pr create -R robert-flo/ravn-git-bare-clone` redactado con la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger` con `Door: two-way` y `Blast Radius: Low`).
   - Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-git-bare-clone-7/wk.md` con URL del PR, head SHA y resultado de pruebas.
