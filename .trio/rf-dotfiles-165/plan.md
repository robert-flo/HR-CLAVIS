# Plan — Spec #165: Integrar configuración de Zsh y Starship de Rollo en rf-dotfiles

- **Spec padre:** `robert-flo/HR-CLAVIS#165`
- **Repo destino:** `robert-flo/rf-dotfiles`
- **Rama del spec:** `165-zsh-starship-rollo` (base `master`)
- **Carpeta de trabajo (worktrees):** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles` (bare en `/z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git`)

## Orden de ejecución (DAG)

1. **`#167` — `rf-dotfiles: nucleo zsh, starship y gobernanza chezmoi (spec #165)`** (sin bloqueos)
   - Integrar núcleo arrancable de Zsh y Starship desde `repos/Rollo/master/Configs/.config/{zsh,starship}` depurado de HyDE, Oh-My-Zsh, Hyprland, Powerlevel10k y rutas hardcodeadas `/home/<user>`.
   - `dot_zshenv` en raíz fijando `ZDOTDIR="${XDG_CONFIG_HOME:-$HOME/.config}/zsh"` (reemplazando `dot_zshrc` actual).
   - `dot_config/zsh/`: `dot_zshenv`, `dot_zshrc.tmpl`, `conf.d/00-env.zsh`, `conf.d/10-core.zsh`, `conf.d/binds.zsh`, `plugin.zsh` (Zinit activado), `prompt.zsh` (Starship con guard `TERM != linux` y `STARSHIP_CONFIG`).
   - `dot_config/starship/`: `starship.toml` (sin duplicados en `right_format`) y `powerline.toml`.
   - `.chezmoiignore.tmpl`: excluir `.zshenv`, `.config/zsh/**` y `.config/starship/**` en `clavis-upstream`.
   - **Criterios:**
     - `zsh -n` pasa sin errores en todos los archivos Zsh creados/modificados.
     - `rg -n '/home/(niri|rollo|dominus|ravn|omarchy)'` sin coincidencias en versionados.
     - `CHEZMOI_PROFILE=clavis-upstream` excluye `.zshenv`, `.config/zsh/` y `.config/starship/`.
     - En `$HOME` temporal o `niri-test`, `zsh -ic 'echo OK'` arranca sin errores y `$STARSHIP_CONFIG` apunta a `~/.config/starship/starship.toml`.
     - PR mergeado en `165-zsh-starship-rollo`.

2. **`#171` — `rf-dotfiles: funciones, widgets y paridad de ecosistema en zsh (spec #165)`** (bloqueado por `#167`)
   - Migrar y limpiar `user.zsh.tmpl`, `functions/` (`bat.zsh`, `bind_M_n_history.zsh`, `duf.zsh`, `eza.zsh`, `fzf.zsh`, `kb_help.zsh`, `error-handlers.zsh`) y `completions/`.
   - Eliminar `skim.zsh` y referencias a HyDE/Hyprland/pokego/waybar.
   - Paridad con Fish en `conf.d/tools.zsh` (`atuin`, `zoxide`, `pay-respects`, `elio`, `ravn-sessionizer` en `Alt+s`, `try` en `/zt` con `TRY_PROJECTS=$HOME/Work`, `term-light`/`term-dark`, `agy`, `cursor-agent`, `claude-yolo`, `cz`, `pn`) y `functions/{cpwd,fastfetch,yazi}.zsh`.
   - **Criterios:**
     - `zsh -n` pasa en todos los archivos de `functions/`, `completions/`, `conf.d/` y `user.zsh.tmpl`.
     - `rg -n '/home/(niri|rollo|dominus|ravn|omarchy)|hyde-shell|hydectl|waybar/scripts|pokego|skim'` sin coincidencias activas en `dot_config/zsh/`.
     - En `zsh -ic`, funciones/aliases clave (`ffec`, `ffe`, `df`, `l`, `ld`, `lt`, `cpwd`, `yazi`, `fm`, `term-light`, `term-dark`, `h`) y widgets (`_insert-history-1-ago`, `append_help_and_run`) existen y están registrados.
     - PR mergeado en `165-zsh-starship-rollo`.

3. **`#172` — `rf-dotfiles: validacion testbench niri-test y despliegue en PC (spec #165)`** (bloqueado por `#171`)
   - Validar `chezmoi diff` y `chezmoi apply --force` en `niri-test` desde la rama del spec.
   - Verificar `CHEZMOI_PROFILE=clavis-upstream`.
   - Verificar bajo `niri-test` que `zsh -ic` inicia Zinit, carga plugins sin errores, activa Starship y mantiene `clavis-fish-theme` en Fish sin regresiones.
   - Aplicar en la PC de Roberto (`niri` / `rollo` según corresponda) y documentar evidencia real para el PR final hacia `master`.
   - **Criterios:**
     - `sudo -u niri-test -H chezmoi apply --source=<worktree> --force` exit code 0.
     - `sudo -u niri-test -H zsh -ic 'echo OK && echo $STARSHIP_CONFIG && which ffec df lt'` limpio con `STARSHIP_CONFIG=/home/niri-test/.config/starship/starship.toml`.
     - `sudo -u niri-test -H fish -ic 'echo OK'` confirma Fish intacto.
     - Despliegue verificado en cuenta activa de Roberto y evidencia documentada en el PR.

## Reglas del repo destino (`rf-dotfiles/AGENTS.md`)
- Cero rutas `/home/<user>` hardcodeadas (usar `.tmpl` con `{{ .chezmoi.homeDir }}` o `$HOME`).
- Gobernanza estricta en `.chezmoiignore.tmpl`.
- Cero trailers `Co-authored-by` ni atribución a IAs en commits.
- Sin `git push --force`.
