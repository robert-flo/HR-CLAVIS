# OS · rf-dotfiles-208 (robert-flo/HR-CLAVIS#208)

Estado: **hecho** (auto-merge por Merge Gate).

- PR: https://github.com/robert-flo/rf-dotfiles/pull/151
- Head revisado: `a2d2370b9721d6d45c408d013601b01972684cd5`
- Merge commit (squash): `daccf38890f8bd5b1bf2bbe29f2ae93fa1bc4eb0`
- Merge Danger: Door: two-way · Blast Radius: Low

## Qué se hizo
`run_onchange_after_configure-cursor-attribution.sh.tmpl` fija `attribution.attributeCommitsToAgent=false` y `attributePRsToAgent=false` en el `cli-config.json` de Cursor, sin tocar el resto, idempotente, creando el archivo si falta y fallando con mensaje claro (sin truncar) ante JSON inválido. Test: `scripts/tests/test-cursor-attribution.sh` (38 casos).

## Hallazgo importante para Roberto
Con `XDG_CONFIG_HOME` seteado, la CLI de Cursor lee `~/.config/cursor/cli-config.json`, no `~/.cursor/cli-config.json`. En el usuario **niri** el primero sigue en `true/true` (el PM solo corrigió `~/.cursor`). Por eso el script cubre los dos archivos. Mientras niri no corra `chezmoi apply`, sus commits desde Cursor pueden seguir llevando `Co-authored-by` (me pasó en mi propio commit; lo reescribí con `git commit-tree` antes de publicar). No toqué ese archivo de niri (producción, fuera del alcance del ticket).

## Evidencia
- `make lint`: shellcheck limpio. `make test`: todos los tests en verde (incluye el nuevo, 38/38).
- `sudo -u niri-classic -H` y `sudo -u niri-test -H` (script renderizado, 2 corridas): rc=0 las dos; 2ª corrida sin cambios (sha idéntico); `jq .attribution ~/.cursor/cli-config.json` = false/false; resto del JSON idéntico. Salidas completas en el body del PR.
- No se corrió `make apply` completo: en niri-classic falla por el resto de #207 (Spicetify, mise, `~/.local/share/chezmoi`).
- Repo sin CI (`.github/workflows` no existe; `gh pr checks` sin checks): ningún check fallando ni pendiente. PR MERGEABLE.
- `code-review` (Standards + Spec): sin violaciones duras. Corregido: validación de `attribution` no objeto con mensaje claro (+4 casos de test), comentario obsoleto y chequeo `/home/` genérico en el test.
- Sin trailers: el commit squash en master no contiene `Co-authored-by` ni «Made with».
- Cambio de sistema registrado en `/z-home-repos/CAMBIOS.md`.
