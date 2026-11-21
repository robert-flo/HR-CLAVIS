# WK Report — robert-flo/HR-CLAVIS#54 (robert-flo/rf-dotfiles)

- **Issue:** `robert-flo/HR-CLAVIS#54` (sub-issue de `robert-flo/HR-CLAVIS#51`)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/71
- **Rama base:** `51-fastfetch-dotfiles`
- **Rama de trabajo:** `issue/54-fastfetch-logo-rotation` (borrada tras merge)
- **Head SHA:** `908ce5a26182daf5e26b5ce479e2e5b893e173e2`
- **Merge SHA (`--squash` en `51-fastfetch-dotfiles`):** `a9fd66babb6565afbda0e86e02e4585d916e1295`

## Archivos modificados
- `dot_config/fish/functions/fastfetch.fish` (nuevo): función autoloaded `fastfetch` con rotación round-robin sobre `*.icon`, caché XDG y passthrough de `--logo` / `-l`.
- `dot_config/bash/fastfetch.sh` (nuevo): función `fastfetch()` equivalente para `bash` (`shellcheck` limpio, base 10 explícita `10#$idx`).
- `dot_bashrc.tmpl`: carga condicional de `${XDG_CONFIG_HOME:-$HOME/.config}/bash/fastfetch.sh`.
- `.chezmoiignore.tmpl`: exclusión de `.config/bash/**` únicamente en el perfil `clavis-upstream`.

## Pruebas ejecutadas
1. **Validación estática:**
   - `fish -n dot_config/fish/functions/fastfetch.fish` → 0 errores.
   - `bash -n dot_config/bash/fastfetch.sh` y `shellcheck dot_config/bash/fastfetch.sh` → 0 errores, 0 warnings.
   - `chezmoi execute-template < dot_bashrc.tmpl | bash -n` → 0 errores.
   - Verificación de cero rutas `/home/<user>` hardcodeadas → `ZERO_HARDCODED_HOME_PATHS`.
2. **Chezmoi en testbench `niri-test` y matriz de perfiles:**
   - `sudo -u niri-test -H chezmoi diff --source=/z/repos/rf-dotfiles/54-fastfetch-logo-rotation` y `chezmoi apply --exclude=scripts --force` desplegaron `.bashrc`, `.config/bash/fastfetch.sh` y `.config/fish/functions/fastfetch.fish` limpiamente.
   - `sudo -u niri-test -H env NO_FISH=1 bash -ic 'type -t fastfetch'` → `function`.
   - `CHEZMOI_PROFILE=<clavis|omarchy|nyxuri> chezmoi managed` incluye ambos wrappers; `clavis-upstream` los excluye.
3. **Rotación real en `fish` y `bash`:**
   - 2 ejecuciones reales consecutivas de `fastfetch` en `fish` avanzaron `/home/niri-test/.cache/fastfetch/logo-index` `1 → 2`.
   - 2 ejecuciones reales consecutivas en `bash` continuaron interoperablemente `3 → 4`.
   - Argumentos exactos verificados (`agk_clan.icon` → `aisaka.icon` en `fish`; `geass.icon` → `hyprland.icon` en `bash`; wrap-around `6 → 0`: `ryuzaki.icon` → `agk_clan.icon`) siempre con `--logo-type kitty`.
4. **Respeto de flags del usuario y edge cases:**
   - `fastfetch --logo none` y `fastfetch -l none` en `fish` y `bash` salieron con código `0`, sin sobrescribir el flag ni alterar el índice en caché (`before=1 after=1`).
   - Passthrough verificado para `--logo none`, `--logo=arch`, `-l none`, `-lnone`.
   - Edge cases verificados: índice `08` (sin error octal), índice corrupto `not-a-number` (fallback `0`), directorio de logos ausente (delega a `command fastfetch`).
5. **Code review (two-axis):** Standards 0 violaciones, Spec 0 hallazgos.
