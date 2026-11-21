# OS-HR-CLAVIS — rf-dotfiles / HR-CLAVIS#52

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/52 (cerrado, sin etiquetas de triage)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/66 (squashed & merged contra `master`)
- **Head SHA:** `553635082b8610c8036c8128a02274fece43a643`
- **Merge SHA:** `dd2c96db92c0178a6da7ccce0d7a90d01a1b1236`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`

## Cambio aplicado
- Se creó `dot_config/fish/conf.d/agy.fish` en `robert-flo/rf-dotfiles` con:
  ```fish
  fish_add_path -g ~/.local/bin
  alias agy="$HOME/.local/bin/agy"
  ```
- Sin rutas `/home/<user>` hardcodeadas; `fish_variables` intacto fuera de Chezmoi.

## Evidencia de pruebas
- `grep -rn '/home/' dot_config/fish/conf.d/agy.fish` → sin coincidencias.
- `fish -n dot_config/fish/conf.d/agy.fish` → exit 0.
- `chezmoi diff --source=/z/repos/rf-dotfiles/52-versionar-agy-fish ~/.config/fish/conf.d/agy.fish` → sin diferencias (exit 0).
- Testbench `niri-test`: `sudo -u niri-test -H chezmoi apply --source=... /home/niri-test/.config/fish/conf.d/agy.fish --force` + `fish -c` confirmó `alias agy loaded OK` y `PATH ~/.local/bin OK`, y `chezmoi diff` posterior limpio (exit 0).
- Despliegue canónico: `git -C /z/rf-dotfiles pull --ff-only origin master` (`59829ca..dd2c96d`), `chezmoi apply ~/.config/fish/conf.d/agy.fish`, y `chezmoi unmanaged ~/.config/fish` ahora solo lista `.config/fish/fish_variables`.
- `code-review`: 0 hallazgos bloqueantes en Standards (`rf-dotfiles/AGENTS.md` + baseline) y 0 en Spec (`HR-CLAVIS#52`).
