# WK — spec robert-flo/HR-CLAVIS#236 (rf-dotfiles)

## PR final
- **URL:** https://github.com/robert-flo/rf-dotfiles/pull/175 — `236-omarchy-personal-updater` → `master`
- **Head SHA:** `982f8421d79f7126b93a1b52e2726e1cecfccf54`
- **GitHub:** `mergeable=MERGEABLE`, `mergeStateStatus=CLEAN`
- **Merge Danger:** `**Door:** two-way` · `**Blast Radius:** Low` → Califica para auto-merge tras revisión y aprobación del RV.

## Sub-PRs mergeados (squash) a la rama del spec 236-omarchy-personal-updater
| Sub-issue | PR | Commit | Resumen |
| --- | --- | --- | --- |
| robert-flo/HR-CLAVIS#237 | robert-flo/rf-dotfiles#167 | `06f068d` | Versionar actualizador `scripts/omarchy-personal-update`, unidades systemd y tests |
| robert-flo/HR-CLAVIS#238 | robert-flo/rf-dotfiles#174 | `982f842` | Desplegar actualizador vía hook `run_onchange_after_04-install-omarchy-personal-updater.sh.tmpl` |

Ambos sub-issues (#237 y #238) cerrados en `robert-flo/HR-CLAVIS` tras sus respectivos merges.

## Pruebas y verificación técnica
- **Suite completa (`make check` en rf-dotfiles):**
  - `✓ profile=clavis template OK and excludes Makefile + make/*`
  - `✓ profile=nyxuri template OK and excludes Makefile + make/*`
  - `✓ profile=omarchy template OK and excludes Makefile + make/*`
  - `✓ packages.json is valid JSON with official, personal, and aur arrays`
  - `✓ shellcheck passed cleanly`
  - `✓ all script tests passed` (incluye `test-omarchy-personal-update.sh` con 26 checks en verde)
  - `✓ all checks passed`
- **Pruebas de `scripts/tests/test-omarchy-personal-update.sh`:**
  - Sin `omarchy-personal.conf`: sale limpiamente con código 0 y mensaje explicativo.
  - Con `/var/lib/pacman/db.lck`: sale limpiamente con código 0 sin tocar nada.
  - Con updates pendientes: genera configuración temporal, ejecuta `pacman -Sy` y `-Qu` sobre ella, e instala solo los paquetes pendientes con `pacman -S --needed --noconfirm` sin tocar otros repositorios.
  - Hook Chezmoi:
    - Renderiza limpiamente con `chezmoi execute-template`.
    - No contiene rutas `/home/<user>` hardcodeadas.
    - Pasa validación de sintaxis `bash -n` y `shellcheck`.
    - Incluye los hashes SHA-256 de los 3 archivos fuente.
    - Ejecución sin privilegios `sudo`: emite aviso y sale con código 0 sin llamar a `install` ni `systemctl`.
    - `chezmoi managed` en perfiles `clavis`, `omarchy` y `nyxuri`: no filtra `scripts/` al `$HOME`.
- **Verificación en testbench (`niri-test`):**
  - `sudo -u niri-test -H chezmoi apply --dry-run --source=...` finaliza con código 0 sin errores.
- **Despliegue real en el sistema y verificación de idempotencia:**
  - Respaldo previo creado en `/usr/local/bin/omarchy-personal-update.bak-20261126-1248` y asentado en `/z-home-repos/CAMBIOS.md`.
  - Primera ejecución del hook con privilegios:
    - Instaló `/usr/local/bin/omarchy-personal-update`.
    - Habilitó y activó `omarchy-personal-update.timer`.
  - Segunda ejecución del hook con privilegios:
    - 0 cambios (`cmp -s` detectó archivos idénticos; no hubo recarga ni reinstalación; salida limpia).
  - Estado del timer verificado:
    - `systemctl is-enabled omarchy-personal-update.timer` → `enabled`.
    - `systemctl is-active omarchy-personal-update.timer` → `active`.
