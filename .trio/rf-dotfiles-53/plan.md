# Plan — robert-flo/HR-CLAVIS#53 (`rf-dotfiles`)

## Metadatos
- **Ticket:** `robert-flo/HR-CLAVIS#53` — *rf-dotfiles: integrar config base de fastfetch, logos PNG, clima de Ilopango y progreso anual*
- **Spec padre:** `robert-flo/HR-CLAVIS#51` — *rf-dotfiles: integrar configuración de fastfetch agnóstica al compositor*
- **Repo objetivo:** `robert-flo/rf-dotfiles`
- **Rama base del PR:** `51-fastfetch-dotfiles` (`origin/51-fastfetch-dotfiles`)
- **Rama de trabajo:** `issue/53-fastfetch-base`
- **Worktree asignado (trabajar SOLO acá):** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/53-fastfetch-base`
- **Tipo de ticket:** Sub-issue de spec (`--base 51-fastfetch-dotfiles`). Según ADR 0018, con CI verde y `code-review` limpio, el WK mergea directamente el PR a `51-fastfetch-dotfiles`.

> **IMPORTANTE sobre referencias de issues:** El issue #53 vive en `robert-flo/HR-CLAVIS`, **no** en `robert-flo/rf-dotfiles` (en `rf-dotfiles` el #53 es un issue viejo cerrado de fish). En el título/body del PR y commits usá siempre `robert-flo/HR-CLAVIS#53` y `robert-flo/HR-CLAVIS#51` (ej. `Closes robert-flo/HR-CLAVIS#53`, `Part of robert-flo/HR-CLAVIS#51`), nunca `#53` a secas.

## Qué construir
Integrar la configuración actual de `fastfetch` (`/home/niri/.config/fastfetch/config.jsonc`) y los 7 íconos PNG de `/home/niri/.config/fastfetch/logo/` (`agk_clan.icon`, `aisaka.icon`, `geass.icon`, `hyprland.icon`, `loli.icon`, `pochita.icon`, `ryuzaki.icon`) como herramienta común gestionada por Chezmoi en `robert-flo/rf-dotfiles`, agnóstica al compositor para todos los perfiles (manteniendo intacta la excepción existente de `niri-upstream`):

1. **Plantilla y cero rutas hardcodeadas:**
   - Adaptar `dot_config/fastfetch/config.jsonc.tmpl` preservando el bloque `{{- if eq .chezmoi.username "niri-upstream" -}}` con `{{ include ".upstream/fastfetch/config.jsonc" }}` y poniendo la configuración base en el bloque `{{- else -}}`.
   - Usar `{{ .chezmoi.homeDir }}` para toda referencia al home del usuario. Prohibido hardcodear `/home/niri` u otro `/home/<user>`.
   - Copiar los 7 archivos `.icon` a `dot_config/fastfetch/logo/`.
   - Verificar `.chezmoiignore.tmpl` para confirmar que `.config/fastfetch/**` no se excluya para ningún perfil (es herramienta común; revisar también que `niri-upstream` no reciba archivos indeseados si corresponde, o que conserve su config upstream intacta).
2. **Logo por defecto:**
   - Configurar `ryuzaki.icon` (`{{ .chezmoi.homeDir }}/.config/fastfetch/logo/ryuzaki.icon`) con protocolo gráfico `kitty`, eliminando la llamada a `hyde-shell fastfetch logo` para que nunca vuelva a caer al logo de Omarchy.
3. **Clima:**
   - Módulo nativo `weather` de `fastfetch` apuntando a `Ilopango,El_Salvador` con timeout de `5000` ms.
4. **Progreso anual (reemplazo de `Splash` / `hyprctl splash`):**
   - Comando POSIX/coreutils agnóstico al compositor que calcule el porcentaje transcurrido del año actual (contemplando años normales de 365 días y bisiestos de 366) e imprima `El <N>% del año ya se fue 😥`.

## Criterios de aceptación y pruebas obligatorias (pegar salidas reales en el body del PR)
1. `chezmoi diff` contra el worktree en el testbench (`niri-test`) muestra la configuración de `fastfetch` y los 7 archivos `.icon` sin ninguna ruta `/home/<user>` hardcodeada, y `chezmoi execute-template` confirma que el perfil `niri-upstream` conserva su configuración upstream intacta.
2. Tras aplicar en testbench (`sudo -u niri-test -H chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/53-fastfetch-base --force`), los 7 `.icon` y la configuración quedan desplegados en `~niri-test/.config/fastfetch/` con `ryuzaki.icon` y protocolo `kitty` configurados como logo por defecto.
3. Al ejecutar `sudo -u niri-test -H fastfetch --logo none` con la configuración desplegada: termina con código de salida 0, no aparece ningún error de `hyde-shell` ni de `HYPRLAND_INSTANCE_SIGNATURE`, el módulo `Weather` muestra `Ilopango,El_Salvador` y el primer módulo imprime `El <N>% del año ya se fue 😥` con el porcentaje correcto del año.
4. El cálculo del porcentaje anual maneja correctamente tanto un año no bisiesto (365 días) como uno bisiesto (366 días) — incluir salida de prueba verificando ambos casos.

## Reglas y restricciones
- Leer `AGENTS.md` del repo `rf-dotfiles` antes de editar.
- Cero trailers `Co-authored-by` ni atribución a agentes en commits.
- Redactar el PR con la skill `pr` (`## Summary`, `## Evidence`, `## Merge Danger`), dirigido a `--base 51-fastfetch-dotfiles`.
- Correr `code-review` antes de mergear.
- Mergear a `51-fastfetch-dotfiles` con `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>`.
- Escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-53/wk.md` con URL del PR, head SHA, pruebas ejecutadas y SHA del merge.
