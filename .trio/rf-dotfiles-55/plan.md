# Plan — HR-CLAVIS#55 (rf-dotfiles)

## Identificación
- **Ticket:** `robert-flo/HR-CLAVIS#55` (Part of `robert-flo/HR-CLAVIS#50`)
- **Tipo:** Sub-issue de spec (el WK mergea su propio PR a la rama del spec con CI verde y `code-review` limpio; ADR 0018)
- **Repo de trabajo:** `robert-flo/rf-dotfiles`
- **Worktree (único donde trabaja WK):** `/z/repos/rf-dotfiles/55-elio-config`
- **Rama de trabajo:** `issue/55-elio-config`
- **Rama base del PR:** `50-elio-config` (origin/50-elio-config)
- **Handoff:** `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-55/wk.md`

## Qué construir
Integrar la configuración optimizada de `elio` 1.15.0 como Herramienta Común en `rf-dotfiles` (`dot_config/elio/config.toml`) y declarar `"elio-bin"` en el array `"aur"` de `.chezmoidata/packages.json`, desplegado de forma idempotente vía Chezmoi.

### 1. `dot_config/elio/config.toml`
- Herramienta Común para todos los perfiles (sin tocar `.chezmoiignore.tmpl`).
- **Cero rutas `/home/<user>` hardcodeadas** (solo `~/` y rutas `/z*`).
- Revisar `~/.config/elio/config.toml` local si sirve de referencia para el formato de íconos y sintaxis de `elio` 1.15.0, pero respetar al pie de la letra el contrato de abajo:
- `[ui]`:
  - `show_top_bar = false`
  - `show_hidden = true`
  - `folders_first = true`
  - `default_sort = "name"`
- `[preview]`:
  - `tab_width = 2`
- `[open]`:
  - `rules = [{ type = ["text", "code", "config"], command = "$EDITOR", terminal = true }]`
- `[places]`:
  - `show_devices = true`
  - Mantener builtins: `home`, `desktop`, `documents`, `{ builtin = "downloads", icon = "" }`, `pictures`, `music`, `videos`, `root`, `trash`
  - Reemplazar `~/Work` legado por entradas de la topología `/z` (`/z`, `/z-home-repos`, `/z/repos`, `/z/vaults`, `/zt`) con íconos Nerd Font limpios.
- `[goto]`:
  - Builtins: `"top"`, `"home"`, `"config"`, `{ builtin = "downloads", key = "D" }`, `"trash"`
  - Entradas canónicas PJ-CLAVIS (sin colisiones de teclas con builtins ni entre sí):
    - `r` → `Repo root` (`command = "git rev-parse --show-toplevel"`)
    - `H` → `HR-CLAVIS` (`/z-home-repos/HR-CLAVIS`)
    - `p` → `Home Repos` (`/z-home-repos`)
    - `R` → `Repos` (`/z/repos`)
    - `d` → `Dotfiles` (`/z/rf-dotfiles`)
    - `v` → `Vaults` (`/z/vaults`)
    - `z` → `/z` (`/z`)
    - `w` → `Tries` (`/z-drive/tries`)
    - `Z` → `z-drive` (`/z-drive`)
    - `x` → `Temp (/zt)` (`/zt`)
- `[layout.panes]`:
  - `places = 0`, `files = 40`, `preview = 60`
- **Fuera de alcance:** `[portal]`, temas `theme*.toml`, wrapper `executable_elio`, `elio.fish`, rebinds en `[keys]`.

### 2. `.chezmoidata/packages.json`
- Agregar `"elio-bin"` al array `"aur"` (sin duplicados y manteniendo formato JSON limpio).

## Criterios de aceptación y pruebas obligatorias (documentar salida en `## Evidence` del PR)
1. **TOML válido y sin `/home/`:**
   - `python3 -c 'import tomllib; tomllib.load(open("dot_config/elio/config.toml","rb"))'` pasa con exit 0.
   - `grep -n '/home/' dot_config/elio/config.toml` devuelve 0 coincidencias.
2. **Carga real en `elio` 1.15.0 y unicidad de teclas en `[goto]`:**
   - `elio --config <ruta> --version` sin errores ni advertencias de configuración.
   - Chequeo programático de unicidad de teclas en `[goto]` (sin colisiones ni con los builtins `top`/`home`/`config`/`downloads(D)`/`trash`).
3. **Manifiesto de paquetes:**
   - `jq -e '[.aur[] | select(. == "elio-bin")] | length == 1' .chezmoidata/packages.json` pasa con exit 0.
   - `jq . .chezmoidata/packages.json > /dev/null` válido.
4. **Chezmoi idempotente en testbench (`niri-test`) y producción (`niri`):**
   - `chezmoi diff` previo (apuntando al source del worktree) sin sorpresas.
   - `chezmoi apply` despliega `~/.config/elio/config.toml` en `niri-test` y en `niri`.
   - Segundo `chezmoi diff` vacío (idempotencia confirmada).
   - Nota: no contaminar otros archivos en `niri-test`/`niri` (usar `--source=/z/repos/rf-dotfiles/55-elio-config` o aplicar específicamente lo que corresponda sin romper `/z/rf-dotfiles` que sigue en `master`).
5. **Verificación funcional de rutas:**
   - `git rev-parse --show-toplevel` resuelve ruta absoluta en un repo.
   - `test -d` pasa para todas las rutas estáticas de `[goto]` y `[places]`.
6. **PR, `code-review`, CI y merge:**
   - Sin trailers de agente ni `Co-authored-by` en commits.
   - PR en `robert-flo/rf-dotfiles` contra `--base 50-elio-config`, redactado con la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), referenciando `Closes robert-flo/HR-CLAVIS#55` y `Part of robert-flo/HR-CLAVIS#50`.
   - Correr `code-review` limpio y verificar CI verde.
   - Como es sub-issue contra `50-elio-config`, el WK mergea su PR con `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>`.
   - Escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-55/wk.md` con URL del PR, head SHA, merge SHA y resumen de pruebas.
