# OS · robert-flo/HR-CLAVIS#209 · rf-dotfiles

**Resultado:** hecho, auto-mergeado.

- PR: https://github.com/robert-flo/rf-dotfiles/pull/150
- Head revisado: `ba4907f3312d2ce2292e84289a775c97fe036bc4`
- Merge commit (squash): `078d85ec0d56e183918120fb98772f261efd88d4`
- Merge Danger: `Door: two-way` / `Blast Radius: Low`

## Qué cambió
`run_onchange_after_install-helix-tools.sh.tmpl` y `run_onchange_after_install-hunk.sh.tmpl`: si `mise where node@latest` falla, `mise install node@latest` antes de `mise use -g <tool>@latest`. Con `RF_APPLY_FULL=1` un fallo real sale 1; sin la variable avisa y sale 0. Test nuevo `scripts/tests/test-install-mise-tools.sh` (36 aserciones, rojo contra los scripts viejos: 12 fallos). `docs/makefile-guide.md` actualizado.

## Evidencia
- Repro real con el script viejo (niri-test, sin node): `failed to resolve configured install dependency 'node@latest' ... while offline`.
- niri-test (sin node): corrida 1 instala node (37.8 s) y luego oxfmt y hunk, rc=0; corrida 2 sin instalar nada (0.4 s), rc=0.
- niri-classic con `installs/node` renombrado: corrida 1 instala node (4.3 s), rc=0; corrida 2 rc=0. node original restaurado.
- `make lint` y `make test` en verde. `git log origin/master..HEAD` sin trailers.
- code-review (Standards + Spec en subagentes): sin violaciones duras. Se corrigió la doc desactualizada. Quedan como juicio, sin tocar: `fail_or_warn` duplicado en los dos scripts (el repo no tiene `.chezmoitemplates/`); los `.tmpl` no pasan por shellcheck (el glob de `make lint` solo cubre scripts/ y tests).

## Notas para Roberto
- El repo no tiene CI (`.github/workflows` no existe): `gh pr checks` no reportó checks, ni fallidos ni pendientes.
- El issue no cierra solo (PR en otro repo): lo cerré a mano con comentario.
- Los commits hechos con `git commit` desde el shell del agente de Cursor vuelven a salir con `Co-authored-by: Cursor`, aunque `~/.cursor/cli-config.json` ya tiene `attribution` en `false`. Lo evité creando el commit con `git commit-tree` + `git reset --soft`; el merge squash de GitHub quedó limpio. Conviene mirarlo en el ticket de atribución (#208).
- No usé ni toqué `/z/rf-dotfiles` ni sus cambios sin commitear.
- Cambios de sistema en `/z-home-repos/CAMBIOS.md` (respaldo `CAMBIOS.md.bak-*`) y respaldos `~/.config/mise/config.toml.bak-20261124-1611` en niri-test y niri-classic.
