# OS · robert-flo/HR-CLAVIS#204 · PR robert-flo/rf-dotfiles#149

**Resultado:** PR abierto y probado; **no se auto-mergeó** (Merge Gate: `Blast Radius: Medium`). Queda `ready-to-merge` + `needs-roberto`.

- PR: https://github.com/robert-flo/rf-dotfiles/pull/149 (rama `issue/204-make-apply-force`, head `d4b12eb901fc5452df654babdd1b9a2884cf0f13`, `MERGEABLE`).
- Cambio: `make apply` y `make apply-{clavis,nyxuri,omarchy}` pasan `--force` a `chezmoi apply` (`make/chezmoi.mk`). `make safe` no cambia y no pasa `--force`. Ayuda, Quick Actions y `docs/makefile-guide.md` avisan que `apply` pisa cambios locales y apuntan a `make lose`. Test nuevo `scripts/tests/test-make-apply-force.sh` (18 aserciones; en rojo sin el cambio, 8 fallos).

## Por qué no auto-merge
- `## Merge Danger`: `Door: two-way`, `Blast Radius: Medium` (el flag destruye cambios locales sin preguntar). El ticket pedía esa clasificación y merge humano.
- El repo no tiene CI: `gh pr checks` no reporta checks.

## Evidencia (resumen; detalle en el PR)
- `niri-test`, archivo `~/.config/alacritty/alacritty.toml` editado a mano. Antes (Makefile de `master`): `could not open a new TTY`, exit 2, archivo sigue modificado. Después: exit 0, archivo igual al repo, `chezmoi verify` OK. Se excluyeron los `run_onchange` con un shim `--exclude=scripts` para no tocar el sistema.
- `DRY_RUN=1` imprime `chezmoi apply --force -S …` en `apply` y `apply-clavis`.
- Segundo `apply-clavis`: exit 0, `chezmoi status` vacío, mtimes de `~/.config` sin cambios.
- `make lint` 0; `make test` todo en verde.
- No se corrió `make safe` real en `niri-test` (sin `~/.local/share/chezmoi`, `chezmoi source-path` falla); lo cubre el test con chezmoi falso.
- `code-review` (dos ejes) sin hallazgos duros. Se aplicaron: quitar la variable `APPLY_FLAGS` (especulativa) y avisar del `--force` en la guía.

## Pendiente para Roberto
Mergear el PR cuando lo decida: `gh pr merge 149 -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit d4b12eb901fc5452df654babdd1b9a2884cf0f13`. Después, cerrar el issue #204 y quitarle `ready-for-agent`.
Worktree `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/204-make-apply-force` se deja hasta el merge.
