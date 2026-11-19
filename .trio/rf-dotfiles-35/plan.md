# Plan — robert-flo/HR-CLAVIS#35 (rf-dotfiles)

## Contexto y referencias
- **Issue (sub-issue):** `robert-flo/HR-CLAVIS#35` — *Arranque seguro de fish en sesiones interactivas de niri, niri-classic y niri-test*
- **Spec padre:** `robert-flo/HR-CLAVIS#32` — *Fish como shell interactiva por defecto en niri, niri-classic y niri-test*
- **Repo objetivo:** `robert-flo/rf-dotfiles`
- **Worktree del WK:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri`
- **Rama del WK:** `issue/35-fish-default-niri`
- **Rama base del PR:** `32-fish-default-niri` (sub-issue: el WK mergea su PR a `32-fish-default-niri` con CI verde y `code-review` limpio)

## Qué construir
Actualizar `dot_bashrc.tmpl` en `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri` para que los usuarios `niri`, `niri-classic` y `niri-test` arranquen automáticamente en `fish` al abrir cualquier sesión interactiva con TTY, manteniendo `/bin/bash` como login shell en `/etc/passwd`.

### Detalles técnicos clave
1. **Alcance de usuarios:** aplicar el bloque condicional de Chezmoi **solo** para `niri`, `niri-classic` y `niri-test` (ej. `{{- if or (eq .chezmoi.username "niri") (eq .chezmoi.username "niri-classic") (eq .chezmoi.username "niri-test") }}`).
2. **Perfiles intactos:** `rollo`, `niri-upstream`, `omarchy`, `omarchy-test`, `nyxuri*`, `sura` y cualquier otro usuario fuera de la lista deben producir un render idéntico al actual, sin `exec fish`.
3. **Carga de entorno antes del salto:** en `dot_bashrc.tmpl`, tras `[[ $- != *i* ]] && return` y **antes** del guard `exec fish`, cargar las variables de entorno de Omarchy (`: "${OMARCHY_PATH:=/usr/share/omarchy}"` y `[[ -r "$OMARCHY_PATH/default/bash/envs" ]] && source "$OMARCHY_PATH/default/bash/envs"`), evitando ejecutar los hooks interactivos pesados de `bash` (`source "$OMARCHY_PATH/default/bash/rc"` queda **después** del guard, para cuando el usuario permanece en `bash`).
4. **Guardia de seguridad (patrón Arch Wiki):**
   - Solo disparar `exec fish` si:
     - la sesión es interactiva (`[[ $- == *i* ]]`, ya garantizado tras el `return`),
     - stdin y stdout son TTY (`[[ -t 0 && -t 1 ]]`),
     - `NO_FISH` no está definida (`[[ -z ${NO_FISH:-} ]]`),
     - no hay cadena de ejecución no interactiva (`[[ -z ${BASH_EXECUTION_STRING:-} ]]`),
     - el proceso padre no es `fish` (`[[ $(ps --no-header --pid=$PPID --format=comm) != "fish" ]]`),
     - el binario `fish` existe en `PATH` (`command -v fish >/dev/null 2>&1`).
   - Propagar login shell si aplica: `shopt -q login_shell && LOGIN_OPTION='--login' || LOGIN_OPTION=''`, y ejecutar `exec fish $LOGIN_OPTION`.
5. **Fuera de alcance:** no tocar `/etc/passwd`, no tocar `.chezmoiignore.tmpl`, no tocar configs de `fish`, Ghostty, Herdr ni Alacritty, no tocar `packages.json`.

## Criterios de aceptación y pruebas obligatorias (pegar evidencia en el PR)
1. **Render de plantilla (Chezmoi):**
   - Renderizar `dot_bashrc.tmpl` para `niri`, `niri-classic` y `niri-test` (ej. con `chezmoi execute-template` o `--override-data '{"chezmoi":{"username":"..."}}'`): confirmar carga de `envs` de Omarchy antes de `exec fish` y `source "$OMARCHY_PATH/default/bash/rc"` después.
   - Renderizar para `rollo` y `niri-upstream`: confirmar que el render es idéntico al actual (sin `exec fish`).
2. **Aplicación idempotente en testbench (`niri-test`):**
   - `sudo -u niri-test -H chezmoi diff --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri` muestra únicamente el cambio esperado en `.bashrc`.
   - `sudo -u niri-test -H chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/35-fish-default-niri --force` termina limpio.
   - Una segunda corrida de `chezmoi diff` devuelve salida vacía.
3. **Sesión interactiva real en `niri-test`:**
   - Una sesión `bash -li` con PTY real (ej. `script -qec ...` o `python3 -c 'import pty...'`) como `niri-test` aterriza en `fish` (`$FISH_VERSION` no vacío) y conserva `OMARCHY_PATH`, `ANDROID_HOME`, `BROWSER` y las rutas de `/etc/profile.d/*` (incluyendo JDK y Omarchy) en `PATH`.
4. **Escapes y casos borde en `niri-test`:**
   - `bash -c 'echo "$BASH_VERSION"'` permanece en `bash`.
   - `NO_FISH=1 bash -li` con PTY permanece en `bash` con entorno interactivo cargado.
   - Abrir `bash` desde adentro de `fish` con PTY permanece en `bash` sin bucle infinito.
   - Simular ausencia de `fish` en `PATH` (con PTY) permanece en `bash` sin errores.
5. **PR y merge (sub-issue):**
   - Commits sin trailers de agentes ni `Co-authored-by`.
   - Abrir PR contra `32-fish-default-niri` en `robert-flo/rf-dotfiles` con body redactado con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `robert-flo/HR-CLAVIS#35` y `robert-flo/HR-CLAVIS#32`.
   - Correr `code-review` limpio.
   - Mergear el PR a `32-fish-default-niri` con `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>` y anotar PR, head SHA, pruebas y merge SHA en `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-35/wk.md`.
