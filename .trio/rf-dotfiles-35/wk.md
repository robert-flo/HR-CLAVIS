# WK — robert-flo/HR-CLAVIS#35 (rf-dotfiles)

## Resumen
- **Issue:** `robert-flo/HR-CLAVIS#35` (spec padre: `robert-flo/HR-CLAVIS#32`)
- **Repo:** `robert-flo/rf-dotfiles`
- **Base del PR:** `32-fish-default-niri`
- **Rama WK:** `issue/35-fish-default-niri` (eliminada tras squash-merge)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/55 (`MERGED`)
- **Head SHA:** `771f09ae050f441763db401cb5290ea8d12ac234`
- **Merge SHA (squash en `32-fish-default-niri`):** `d0726c300c641979ade25a11395fd0dd36c1c50c`
- **Archivo tocado:** `dot_bashrc.tmpl` (+17/-0 líneas)

## Cambios implementados
- Se agregó un bloque condicional de Chezmoi restringido exclusivamente a `niri`, `niri-classic` y `niri-test` después de `[[ $- != *i* ]] && return` y antes de `source "$OMARCHY_PATH/default/bash/rc"`.
- Antes del salto a `fish`, carga `: "${OMARCHY_PATH:=/usr/share/omarchy}"` y `[[ -r "$OMARCHY_PATH/default/bash/envs" ]] && source "$OMARCHY_PATH/default/bash/envs"`.
- Aplica el guard seguro de Arch Wiki (`-t 0 && -t 1`, `NO_FISH`, `BASH_EXECUTION_STRING`, `PPID != fish`, `command -v fish`) propagando `--login` si `shopt -q login_shell`.
- `source "$OMARCHY_PATH/default/bash/rc"` permanece después del guard para cuando el usuario se queda en `bash`.
- Sin cambios para `rollo`, `niri-upstream` ni ningún otro perfil fuera de la lista.

## Pruebas reales verificadas
1. **Render de plantilla (`chezmoi execute-template` + `bash -n`):**
   - `rollo` y `niri-upstream`: render byte-a-byte idéntico al anterior (`diff` vacío, sin `exec fish`).
   - `niri`, `niri-classic` y `niri-test`: render idéntico entre sí con `envs` antes de `exec fish` y `rc` después; `bash -n` OK en los 5 perfiles.
   - `shellcheck -s bash -e SC1091` sobre el hunk renderizado: 0 warnings.
2. **Aplicación idempotente en testbench (`niri-test`):**
   - `sudo -u niri-test -H chezmoi diff --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri`: mostró únicamente el cambio esperado en `.bashrc` (tras alinear `niri-test` a `master`).
   - `sudo -u niri-test -H chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri --force`: exit 0.
   - Segunda corrida de `chezmoi diff`: salida vacía (`DIFF_LEN=0`).
   - *Nota:* `/home/niri-test/.bash_profile` no existía; se restauró desde `/etc/skel/.bash_profile` (registrado en `/z-home-repos/CAMBIOS.md`), igual que `niri` y `niri-classic`, para validar `bash -li`.
3. **Sesión interactiva real `bash -li` con PTY en `niri-test`:**
   - Antes: `BASH=5.3.20(1)-release`, `FISH=` (vacío).
   - Después: `exit=0`, `FISH_VERSION=4.9.3`, `IS_LOGIN=yes`, `OMARCHY_PATH=/z/pj-omarchy/fo-omarchy`, `ANDROID_HOME=/opt/android-sdk`, `BROWSER=omarchy-launch-browser`, `PATH` conserva `/z/pj-omarchy/fo-omarchy/bin`, `/usr/lib/jvm/default/bin` (JDK de `/etc/profile.d`), `/opt/android-sdk/*` y `mise`.
4. **Escapes y casos borde en `niri-test`:**
   - `bash -c`: `exit=0`, `BASH_VERSION=5.3.20(1)-release`, `FISH_VERSION=` (se queda en `bash`).
   - `NO_FISH=1 bash -li` con PTY: `exit=0`, `BASH_VERSION=5.3.20(1)-release`, `FISH_VERSION=`, `OMARCHY_PATH=/z/pj-omarchy/fo-omarchy`, `alias agy='/home/niri-test/.local/bin/agy'` (se queda en `bash` con entorno interactivo cargado).
   - `bash -i` desde `fish -i` con PTY: `exit=0`, `INNER_BASH=5.3.20(1)-release`, `INNER_FISH=`, `PPID_COMM=fish` (sin bucle infinito).
   - Sin `fish` en `PATH` con PTY: `exit=0`, `NO_FISH_BIN_BASH=5.3.20(1)-release`, `NO_FISH_BIN_FISH=`, `CMD_FISH=MISSING` (permanece en `bash` sin errores).

## Code Review
- **Standards (`rf-dotfiles/AGENTS.md` + smell baseline):** 0 hallazgos (cero contaminación cruzada, sin rutas hardcodeadas, sin trailers de agentes, `shellcheck` limpio).
- **Spec (`robert-flo/HR-CLAVIS#35` + `plan.md`):** 0 hallazgos (todos los requisitos y criterios de aceptación cumplidos, cero scope creep).
- **CI:** sin checks configurados en `robert-flo/rf-dotfiles`.
