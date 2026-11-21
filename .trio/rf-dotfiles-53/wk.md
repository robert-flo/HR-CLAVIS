# WK — robert-flo/HR-CLAVIS#53 (`rf-dotfiles`)

## Resultado
- **Estado:** MERGED en rama del spec `51-fastfetch-dotfiles`
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/67
- **Rama base:** `51-fastfetch-dotfiles`
- **Rama de trabajo:** `issue/53-fastfetch-base` (eliminada en remoto tras squash-merge)
- **Head SHA:** `dde991cca7f0f931be0b06818593c086e6b7b42c`
- **Merge SHA:** `258112e9d7736e0d35eda6e6f079224773a4a35e`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/53-fastfetch-base`

## Archivos modificados
- `.chezmoiignore.tmpl` (excluye `.config/fastfetch/logo/**` únicamente en `clavis-upstream`; `.config/fastfetch/**` sigue común para todos los perfiles)
- `dot_config/fastfetch/config.jsonc.tmpl` (preserva `niri-upstream` con `.upstream/fastfetch/config.jsonc`; agrega config base con `{{ .chezmoi.homeDir }}`, `ryuzaki.icon` + `type: kitty`, clima `Ilopango,El_Salvador` con `timeout: 5000`, progreso anual POSIX 365/366 días y escape Go template para `playerctl`)
- `dot_config/fastfetch/logo/{agk_clan,aisaka,geass,hyprland,loli,pochita,ryuzaki}.icon` (7 íconos PNG en modo `0644`)

## Pruebas reales ejecutadas
1. **Plantillas y cero rutas hardcodeadas:**
   - `grep -rn '/home/' dot_config/fastfetch/` → 0 coincidencias.
   - `chezmoi execute-template` con `username=niri-upstream` → `diff -u .upstream/fastfetch/config.jsonc -` sin diferencias (idéntico al upstream).
   - `chezmoi managed` para `niri-upstream` → solo `.config/fastfetch/config.jsonc` (sin los 7 `.icon` personales).
   - `chezmoi managed` para `omarchy-test` y `niri-test` → `config.jsonc` + los 7 `.icon`.
   - `chezmoi execute-template` con `username=niri-test` validado con `python3 -c "import json, sys; json.load(sys.stdin)"` → `JSON VALIDO`.
   - `sudo -u niri-test -H chezmoi status --source=<worktree>` y `chezmoi diff` verificados antes de aplicar.
2. **Despliegue en testbench (`niri-test`):**
   - `sudo -u niri-test -H chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/53-fastfetch-base --force --exclude=scripts /home/niri-test/.config/fastfetch` (registrado previamente en `/z-home-repos/CAMBIOS.md`).
   - Confirmados `/home/niri-test/.config/fastfetch/config.jsonc` (`"type": "kitty"`, `"source": "/home/niri-test/.config/fastfetch/logo/ryuzaki.icon"`) y los 7 archivos `.icon` en `/home/niri-test/.config/fastfetch/logo/`.
3. **Ejecución real de `fastfetch --logo none` en `niri-test`:**
   - `EXIT_CODE=0`, cero errores de `hyde-shell` ni `HYPRLAND_INSTANCE_SIGNATURE`.
   - Primer módulo: ` Year │ El 89% del año ya se fue 😥`.
   - Clima: `󰖕 Weather │ +28°C - Patchy rain nearby (Ilopango,El_Salvador)`.
   - Spotify: `󰎆 Spotify │ Not playing`.
4. **Cálculo anual en años no bisiestos (365) y bisiestos (366):**
   - `2026 d=008 t=365 -> El 2% del año ya se fue 😥` (sin error octal)
   - `2026 d=325 t=365 -> El 89% del año ya se fue 😥`
   - `2026 d=365 t=365 -> El 100% del año ya se fue 😥`
   - `2024 d=365 t=366 -> El 99% del año ya se fue 😥`
   - `2024 d=366 t=366 -> El 100% del año ya se fue 😥`
   - `2100 d=365 t=365 -> El 100% del año ya se fue 😥`
   - `2000 d=366 t=366 -> El 100% del año ya se fue 😥`

## Code Review (`/code-review` sobre `origin/51-fastfetch-dotfiles...HEAD`)
- **Spec:** 0 hallazgos (Spec-compliant).
- **Standards:** 0 violaciones a `AGENTS.md`. 2 smells de criterio descartados con razón: (1) bordes repetidos en `config.jsonc.tmpl` se mantienen literales por legibilidad idiomática de JSONC; (2) los 7 `.icon` son requerimiento explícito de `robert-flo/HR-CLAVIS#53` y `#51` para la rotación secuencial.
- **CI:** `rf-dotfiles` no tiene workflows en `.github/workflows/`.
