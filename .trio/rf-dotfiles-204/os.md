# OS · robert-flo/HR-CLAVIS#204 · PR robert-flo/rf-dotfiles#149

**Resultado:** PR abierto y probado; **no se auto-mergeó** (Merge Gate: `Blast Radius: Medium`). Queda `ready-to-merge` + `needs-roberto`.

- PR: https://github.com/robert-flo/rf-dotfiles/pull/149 (rama `issue/204-make-apply-force`, head `378f4c56c5d978b4decfad595f22e821125f02a8`, `MERGEABLE`).
- Ampliación (13:55 y 13:57, mismo PR): apply completo = `sudo -v` una vez + keepalive que no queda vivo, falla sin TTY ni sudo sin contraseña antes de tocar nada, `RF_APPLY_FULL=1` hace fallar los 3 scripts que omitían sin sudo, y re-ejecuta todos los `run_onchange_*` (verificado con chezmoi real: el estado vive en `entryState`, no en `scriptState`). `DRY_RUN=1` no pide sudo ni toca estado. Extra necesario: `chmod a+wrX` en Spicetify (sin `x`, `niri-test` no podía escribir `/opt/spotify/Apps`).
- Cambio original: `make apply` y `make apply-{clavis,nyxuri,omarchy}` pasan `--force` a `chezmoi apply` (`make/chezmoi.mk`). `make safe` no cambia y no pasa `--force`. Ayuda, Quick Actions y `docs/makefile-guide.md` avisan que `apply` pisa cambios locales y apuntan a `make lose`. Test nuevo `scripts/tests/test-make-apply-force.sh` (63 aserciones).

## Por qué no auto-merge
- `## Merge Danger`: `Door: two-way`, `Blast Radius: Medium` (destruye cambios locales sin preguntar y corre todos los scripts como root en cada apply). El ticket pedía esa clasificación y merge humano.
- El repo no tiene CI: `gh pr checks` no reporta checks.

## Evidencia (resumen; detalle en el PR)
- `niri-test`, archivo `~/.config/alacritty/alacritty.toml` editado a mano. Antes (Makefile de `master`): `could not open a new TTY`, exit 2, archivo sigue modificado. Después: exit 0, archivo igual al repo, `chezmoi verify` OK. Se excluyeron los `run_onchange` con un shim `--exclude=scripts` para no tocar el sistema.
- Dos `make apply-clavis` seguidos con chezmoi real: rc=0 las dos, los scripts corren las dos veces (19 líneas `==> [chezmoi]`), `chezmoi status` vacío; `chezmoi apply` directo: 0 scripts. Sin TTY y sin sudo: falla antes de tocar nada. Directo sin sudo: tolerante; con `RF_APPLY_FULL=1`: exit 1. `pgrep` del keepalive vacío tras éxito, fallo y TERM.
- `DRY_RUN=1` imprime `chezmoi apply --force -S …` en `apply` y `apply-clavis`.
- Segundo `apply-clavis`: exit 0, `chezmoi status` vacío, mtimes de `~/.config` sin cambios.
- `make lint` 0; `make test` todo en verde.
- No pude mostrar el prompt de contraseña tras `sudo -k` (niri-test tiene sudo sin contraseña): lo cubre el test con sudo falso y pty.
- No se corrió `make safe` real en `niri-test` (sin `~/.local/share/chezmoi`, `chezmoi source-path` falla); lo cubre el test con chezmoi falso.
- `code-review` (dos ejes, dos rondas) sin hallazgos duros. Se aplicaron: quitar `APPLY_FLAGS`, avisar del `--force` en la guía, reenviar TERM al chezmoi en curso, tests sin `/home/<user>` ni sleeps fijos.

## Pendiente para Roberto
Mergear el PR cuando lo decida: `gh pr merge 149 -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit 378f4c56c5d978b4decfad595f22e821125f02a8`. Después, cerrar el issue #204 y quitarle `ready-for-agent`.
Worktree `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/204-make-apply-force` se deja hasta el merge.
