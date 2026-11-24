# RV — spec robert-flo/HR-CLAVIS#195 (rf-dotfiles)

## Veredicto
**APRUEBO `f8160335982976cc29acbfe8eb97504c821e7ff7`**

- PR: https://github.com/robert-flo/rf-dotfiles/pull/148 (`195-flags-dolphin-niri` → `master`)
- Comentario en el PR: https://github.com/robert-flo/rf-dotfiles/pull/148#issuecomment-7142318362
- Comentario en el spec: https://github.com/robert-flo/HR-CLAVIS/issues/195#issuecomment-7142318981
- Worktree de revisión `repos/rf-dotfiles/rv-148` y rama `rv/148`: borrados (nunca se subieron).

## Pruebas (sobre el head exacto, en worktree efímero)
| Prueba | Resultado |
| --- | --- |
| `make lint` | shellcheck limpio |
| `make test` | todo en verde: 14 + 29 + 26 + 50 + 29 + 63 + 32 + 22, 0 fallos |
| `make browser-flags-check-all` (binarios instalados: Chrome, Beta, Chromium, Edge, Brave) | `missing=0` en los cinco |
| `chezmoi managed` por perfil (`clavis`, `omarchy`, `nyxuri`, `clavis-upstream`) | `dolphinrc`, `40-xdg-menu.conf` y `chrome-h264` solo en `clavis` |
| `chezmoi apply` a destino temporal (`clavis`, `omarchy`) | 0 líneas de diff tras aplicar; `--load-extension` solo en `omarchy`; Chrome y Beta idénticos |
| Edición manual de `dolphinrc` + 2.º apply | se conserva (`create_`) |
| Merge de `origin/master` (f816033) | `make/lint.mk` bien unido; `mergeStateStatus=CLEAN` |

## Exigencias del proyecto
- Sin `Co-authored-by`, «Cursor» ni firma de agente en `origin/master..HEAD` (7 commits, autor Roberto Flores).
- Sin force-push, imágenes commiteadas, secretos ni rutas `/home/<user>` nuevas; `git diff --check` limpio.
- Paquetes solo en `.chezmoidata/packages.json` (válido, ordenado, existen en `extra`).
- Rutas no comunes en `.chezmoiignore.tmpl`; Qt global y default de Nautilus intactos.
- Scripts bash nuevos con `set -euo pipefail`; `chrome-h264` es Python.
- Lo de «cómo prueba el WK» (`chezmoi diff` / `apply`) corrido de verdad (destino temporal; la evidencia en `niri-test` es la del WK sobre a75e52b).

## Observaciones (no bloquean)
1. El body del PR conserva el párrafo «Conflicto con `master`», ya obsoleto: quitarlo antes de mergear.
2. `## Merge Danger` no usa el formato `**Door:** … / **Blast Radius:** …`; irrelevante porque el radio es Medium.
3. Paquetes de Dolphin en el manifiesto único (global por diseño); archivos y entorno sí aislados a `clavis`.
4. `XDG_MENU_PREFIX` llega a la sesión en el siguiente login; seguimientos del WK (konsole, Antigravity/fcitx5, Obsidian) fuera de alcance.

## Merge Gate
`Door: two-way` + `Blast Radius: Medium` ⇒ **sin auto-merge**: `ready-to-merge` + `needs-roberto`, merge humano:
`gh pr merge 148 -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit f8160335982976cc29acbfe8eb97504c821e7ff7`.
Si el head se mueve, vuelve a revisión.
