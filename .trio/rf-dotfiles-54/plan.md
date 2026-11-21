# Plan — robert-flo/HR-CLAVIS#54 (PR en robert-flo/rf-dotfiles)

## Datos del ticket
- **Issue:** `robert-flo/HR-CLAVIS#54` (vive en `robert-flo/HR-CLAVIS`; sub-issue del spec `robert-flo/HR-CLAVIS#51`).
- **Repositorio de código:** `robert-flo/rf-dotfiles`
- **Worktree:** `/z/repos/rf-dotfiles/54-fastfetch-logo-rotation`
- **Rama de trabajo:** `issue/54-fastfetch-logo-rotation`
- **Rama base del PR:** `51-fastfetch-dotfiles` (actualizada a `258112e9d7736e0d35eda6e6f079224773a4a35e`, que ya incluye `#53` / PR `#67`).
- **Tipo:** sub-issue (el WK mergea su PR a `51-fastfetch-dotfiles` con CI verde y `code-review` limpio; sin paso de RV).

## Objetivo
Añadir un wrapper liviano para el comando `fastfetch` en `fish` y en `bash` (gestionados por Chezmoi como herramienta común en `robert-flo/rf-dotfiles`) que rote en orden circular (round-robin) por todos los archivos `.icon` del directorio de logos de `fastfetch` del usuario en cada ejecución.

## Requisitos técnicos
1. **Ubicación en Chezmoi (herramienta común):**
   - **Fish:** función autoloaded en `dot_config/fish/functions/fastfetch.fish` (revisar `dot_config/fish/` para mantener convenciones del repo).
   - **Bash:** función `fastfetch` en `dot_bashrc.tmpl` (o archivo dedicado cargado desde `dot_bashrc.tmpl`; revisar `dot_bashrc.tmpl` y `.chezmoiignore.tmpl` para garantizar que se despliegue como herramienta común sin contaminar perfiles).
2. **Comportamiento del wrapper en ambas shells:**
   - Directorio de logos: `${XDG_CONFIG_HOME:-$HOME/.config}/fastfetch/logo` (cero rutas hardcodeadas `/home/<user>`).
   - Archivo de índice persistente: dentro de `${XDG_CACHE_HOME:-$HOME/.cache}` (por ejemplo `${XDG_CACHE_HOME:-$HOME/.cache}/fastfetch/logo-index`, creando el directorio con `mkdir -p` si no existe).
   - Si el usuario pasa `--logo`, `--logo=*`, `-l`, o `-l*` explícitamente, el wrapper delega directamente a `command fastfetch "$@"` (o `command fastfetch $argv` en fish) sin sobrescribir el logo.
   - Si el directorio de logos no existe o no contiene archivos `*.icon`, delega directamente a `command fastfetch` con los argumentos originales.
   - En el caso normal: lista de forma ordenada (determinista) todos los archivos `*.icon` del directorio de logos, lee el índice actual (default `0` si no existe o no es entero válido), calcula el siguiente índice circular `(idx + 1) % total` (o selecciona `idx % total` y persiste `(idx + 1) % total` para no repetir), guarda el nuevo índice en caché y ejecuta `command fastfetch --logo <archivo> --logo-type kitty <args...>`.
3. **Reglas de `rf-dotfiles/AGENTS.md`:**
   - Cero rutas hardcodeadas (`/home/<user>`).
   - Sin trailers `Co-authored-by` ni atribución a agentes en commits.
   - Mantener `.chezmoiignore.tmpl` coherente (herramienta común; no debe quedar excluida en otros perfiles).

## Criterios de aceptación y pruebas obligatorias (incluir salidas en el body del PR)
1. **Validación estática:** `fish -n` sobre la función de `fish` y `bash -n` + `shellcheck` sobre lo modificado en `bash` pasan con 0 errores y 0 warnings.
2. **Despliegue Chezmoi en testbench (`niri-test`):** `sudo -u niri-test -H chezmoi diff --source=/z/repos/rf-dotfiles/54-fastfetch-logo-rotation` y `chezmoi apply` (o probar limpiamente con `--source` al worktree) despliegan el wrapper en ambas shells sin rutas hardcodeadas ni sorpresas.
3. **Rotación real en `fish` y `bash`:** ejecutar el wrapper `fastfetch` dos veces consecutivas en `fish` y dos veces en `bash` (usando el despliegue real / función cargada), verificando que el índice en caché avanza y selecciona dos archivos `.icon` distintos en orden circular pasando `--logo-type kitty`.
4. **Respeto de flags del usuario:** invocar `fastfetch --logo none` (y probar `-l none`) en ambas shells verificando que el wrapper no sobrescribe el flag y termina con código 0.

## Entrega del WK
- Abrir PR en `robert-flo/rf-dotfiles` contra `--base 51-fastfetch-dotfiles` usando la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `robert-flo/HR-CLAVIS#54` y `Part of robert-flo/HR-CLAVIS#51`.
- Correr `code-review` sobre los cambios y asegurar CI verde.
- Al ser sub-issue: mergear el PR a `51-fastfetch-dotfiles` con:
  `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>`
- Escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-54/wk.md` con: URL del PR, head SHA, pruebas ejecutadas y SHA del merge.
