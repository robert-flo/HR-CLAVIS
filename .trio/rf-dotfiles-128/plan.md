# Plan — robert-flo/HR-CLAVIS#128

## Ticket
- **Issue:** `robert-flo/HR-CLAVIS#128` — `rf-dotfiles: atajo en fish (cualquier terminal) y dentro de herdr para el selector de sesiones`
- **Parent:** `robert-flo/HR-CLAVIS#123` (herdr: 1 sesión persistente por spec, cierre por Orquestador y selector fzf)
- **Bloqueador:** `robert-flo/HR-CLAVIS#126` (`CLOSED`)
- **Repo de código:** `robert-flo/rf-dotfiles`
- **Worktree:** `/z/repos/rf-dotfiles/128-sessionizer-keybindings`
- **Rama:** `issue/128-sessionizer-keybindings` (base: `origin/HEAD` → `master`)

## Objetivo
Configurar en `rf-dotfiles` (chezmoi):
1. Un atajo de teclado en `fish` (en `dot_config/fish/conf.d/`, común a todos los perfiles) para invocar el selector `fzf` de sesiones de `ravn-sessionizer` desde cualquier terminal (tanto fuera como dentro de `herdr`).
2. El atajo correspondiente dentro de `herdr` (`dot_config/herdr/config.toml`) para invocar el selector de sesiones de `ravn-sessionizer`.

## Contexto observado
- `dot_config/herdr/config.toml` actualmente tiene `goto = "prefix+f"` (línea 70) y aún no tiene configurados los `[[keys.command]]` de `ravn.sessionizer.*`.
- El paquete `ravn-sessionizer` ya está instalado en el sistema (`/usr/bin/ravn-sessionizer`) y su repo está en `/z-home-repos/HR-CLAVIS/ravn-sessionizer` (para consultar subcomandos y acciones de `herdr-plugin.toml` introducidas en #126).

## Tareas para el WK
1. Inspeccionar `/usr/bin/ravn-sessionizer` / `ravn-sessionizer --help` y `herdr-plugin.toml` en `ravn-sessionizer` para confirmar el subcomando exacto y la acción del plugin de `herdr` correspondientes al selector de sesiones de #126.
2. Configurar en `dot_config/fish/conf.d/` (p. ej. `ravn-sessionizer.fish` o en `rf-common.fish`, siguiendo la convención de `conf.d/*.fish`) el keybinding interactivo de `fish` que ejecute el selector de sesiones de `ravn-sessionizer` (funcionando tanto fuera como dentro de `herdr`, cuidando `commandline -f repaint` si aplica).
3. Configurar en `dot_config/herdr/config.toml` el atajo dentro de `herdr` (`[[keys.command]]`) para invocar el selector de sesiones de `ravn-sessionizer` (resolviendo limpiamente cualquier colisión con `goto = "prefix+f"` si se usa `prefix+f`, o eligiendo un atajo libre consistente).
4. Verificar `.chezmoiignore.tmpl` para asegurar que los archivos tocados se desplieguen en los perfiles esperados sin contaminación cruzada ni rutas hardcodeadas.
5. Probar con `chezmoi diff` (y validación de sintaxis fish / herdr config) y dejar evidencia en el PR.
6. Abrir el PR contra `master` en `robert-flo/rf-dotfiles` (referenciando `robert-flo/HR-CLAVIS#128` en el body, **sin** `Closes #128`, usando la skill `/pr` con `## Summary`, `## Evidence`, `## Merge Danger`) y escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-128/wk.md`.

## Criterios de aceptación
- El keybinding de `fish` lanza el selector de sesiones de `ravn-sessionizer` tanto fuera como dentro de `herdr`, verificado con `chezmoi diff` limpio en el PR.
- El atajo dentro de `herdr` invoca el selector de sesiones de `ravn-sessionizer`, verificado con `chezmoi diff` limpio en el PR.
- Sin rutas hardcodeadas (`/home/<user>`), sin trailers `Co-authored-by` ni atribución a agentes en commits.
