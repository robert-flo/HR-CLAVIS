# Plan: robert-flo/HR-CLAVIS#211 — install-spicetify no rompe el apply del segundo usuario

- Issue: https://github.com/robert-flo/HR-CLAVIS/issues/211 (hijo de #207; leerlo primero)
- Repo de código / PR: `robert-flo/rf-dotfiles`, PR contra `master` con `robert-flo/HR-CLAVIS#211` en el body (no `Closes #211`).
- Worktree: `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/211-spicetify-spotify-global`
- Rama: `issue/211-spicetify-spotify-global`
- Base: `origin/HEAD` (`master`)
- Archivo principal: `run_onchange_after_install-spicetify.sh.tmpl` (más sus tests en `scripts/tests/`).

## Problema
Spotify es una instalación global (`/opt/spotify`). El primer usuario con Spicetify (niri) la parchea; para el segundo (niri-classic) `spicetify backup apply` falla («Failed to backup app files … Reinstall Spotify»), el script sale 1 y con `RF_APPLY_FULL=1` rompe todo `make apply`.

## Qué tiene que hacer el WK
1. Decidir y **justificar en el PR con evidencia** (spicetify 2.46.x) cómo detectar de forma fiable «Spotify ya parcheado por otro usuario» y qué hacer.
   - Propuesta: parcheado + sin backup propio => el script lo dice y sale 0 (también con `RF_APPLY_FULL=1`; no es falta de sudo, es «ya hecho»). No parcheado => flujo actual. Nunca destruir el parche del primero ni correr `spicetify restore`/`clear` sobre un parche ajeno.
   - Evaluar si el usuario nuevo necesita `config-xpui.ini` coherente (tema/extensiones) y si `spicetify apply` posterior de niri sigue funcionando.
   - Cubrir `prefs` ausente (usuario que nunca abrió Spotify).
2. Tests automatizados de las ramas: parcheado, limpio, sin prefs, y que `RF_APPLY_FULL=1` sin sudo siga fallando.
3. Pruebas reales con salidas completas adjuntas en el PR:
   - `sudo -u niri-classic -H make apply` x2 seguidos: rc 0, sin tocar el parche (`/opt/spotify/Apps` y `stat` antes/después).
   - `sudo -u niri-test -H` equivalente.
   - Escenario inverso (Spotify limpio): en entorno controlado o con un fake de `/opt/spotify`; primer usuario aplica bien y el segundo no rompe.
4. `make lint` y `make test` en verde.

## Restricciones
- No tocar `/opt/spotify` a mano salvo en el entorno de prueba acordado. Todo cambio de sistema se anota antes en `/z-home-repos/CAMBIOS.md`.
- No tocar los cambios sin commitear ajenos de `/z/rf-dotfiles` (respaldados en `~/.cache/ravn-piloto/wip-lose-sin-commitear-2026-11-24.patch`). Trabajar solo en el worktree.
- Sin `{{ homeDir }}` hardcodeado (`/home/<user>`), cumplir `AGENTS.md` de rf-dotfiles.
- Commits sin trailers de agente; verificar con `git log --format=%B | rg -i "co-authored|cursor"` antes de publicar.
- `## Merge Danger` honesto (toca el parche global de Spotify): se espera `Door: two-way` y `Blast Radius: Medium` => merge humano.
