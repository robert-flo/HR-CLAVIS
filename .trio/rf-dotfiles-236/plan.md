# Plan del Spec #236 — Actualizador horario idempotente de `[omarchy-personal]` en `rf-dotfiles`

- **Spec padre:** `robert-flo/HR-CLAVIS#236`
- **Repo de código / PRs:** `robert-flo/rf-dotfiles`
- **Rama base:** `master`
- **Rama del spec:** `236-omarchy-personal-updater` (ya existe en `origin`, sincronizada en `/z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git`)
- **Bare repo:** `/z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git`
- **Raíz de worktrees:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/`

## Reglas clave del repo (`rf-dotfiles/AGENTS.md` y `comun.md`)

- Usar worktrees desde el bare existente (`git -C /z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git fetch origin`, nunca reclonar, nunca trabajar en el clon `/z-home-repos/HR-CLAVIS/rf-dotfiles`).
- Sin rutas hardcodeadas (`/home/<user>`); usar plantillas `.tmpl` y variables de Chezmoi cuando corresponda.
- Sin trailers `Co-authored-by` ni atribución automática a agentes en commits.
- Los PRs se redactan con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`).
- El WK mergea cada sub-PR a `236-omarchy-personal-updater` tras CI verde y `/code-review` limpio. Al terminar todos los sub-issues, abre el PR final de `236-omarchy-personal-updater` hacia `master` (referenciando `robert-flo/HR-CLAVIS#236`) y escribe `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-236/wk.md`.

## Orden de ejecución (DAG)

### 1. `robert-flo/HR-CLAVIS#237` — Versionar actualizador `[omarchy-personal]`, unidades systemd y tests
- **Estado:** COMPLETADO (PR `robert-flo/rf-dotfiles#167` squash-mergeado en `236-omarchy-personal-updater`).
- **Bloqueado por:** ninguno (arranca primero).
- **Alcance:**
  - Versionar en `rf-dotfiles`:
    - `scripts/omarchy-personal-update`
    - `scripts/systemd/omarchy-personal-update.service`
    - `scripts/systemd/omarchy-personal-update.timer`
  - El script `scripts/omarchy-personal-update` debe:
    - Salir con código `0` limpiamente si existe `/var/lib/pacman/db.lck`.
    - Salir con código `0` y mensaje claro si falta `/etc/pacman.d/omarchy-personal.conf`.
    - Crear una configuración temporal (limpiada con `trap`) que incluya únicamente `/etc/pacman.d/omarchy-personal.conf`, ejecutar `pacman -Sy --config "$tmpconf" --noconfirm >/dev/null`, consultar actualizaciones con `pacman -Qu --config "$tmpconf"` y, si hay paquetes pendientes, instalarlos con `pacman -S --config "$tmpconf" --noconfirm --needed "${updates[@]}"` (sin tocar `core`/`extra`/`omarchy` ni hacer downgrades).
    - Nota: si existen versiones locales en `/usr/local/bin/omarchy-personal-update` o `/etc/systemd/system/omarchy-personal-update.{service,timer}`, se pueden consultar como referencia de contenido (respetando todos los requisitos del spec).
  - Integrar el nuevo script (y su test) en `make lint` (`shellcheck` limpio, sin advertencias).
  - Agregar pruebas automatizadas bajo `scripts/tests/` ejecutadas por `make test` que validen con `PATH` aislado / `pacman` simulado y rutas temporales:
    - salida `0` con `db.lck` presente,
    - salida `0` sin `omarchy-personal.conf`,
    - invocación exclusiva de `pacman -Sy` / `-Qu` / `-S` usando la configuración temporal.
- **Criterios de aceptación:**
  - [ ] `scripts/omarchy-personal-update`, `scripts/systemd/omarchy-personal-update.service` y `scripts/systemd/omarchy-personal-update.timer` versionados en `rf-dotfiles`.
  - [ ] `make lint` incluye el nuevo script y pasa `shellcheck` sin advertencias.
  - [ ] `make test` valida los 3 escenarios con mocks y pasa en verde.
  - [ ] Sub-PR mergeado a `236-omarchy-personal-updater` en `robert-flo/rf-dotfiles`.

### 2. `robert-flo/HR-CLAVIS#238` — Desplegar actualizador `[omarchy-personal]` vía hook `run_onchange_` de Chezmoi
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/238-omarchy-personal-hook` (rama `issue/238-omarchy-personal-hook`, base `236-omarchy-personal-updater`)
- **Bloqueado por:** `robert-flo/HR-CLAVIS#237` (ya resuelto).
- **Alcance:**
  - Agregar `run_onchange_after_04-install-omarchy-personal-updater.sh.tmpl` en `rf-dotfiles` para desplegar de forma idempotente:
    - `/usr/local/bin/omarchy-personal-update`
    - `/etc/systemd/system/omarchy-personal-update.service`
    - `/etc/systemd/system/omarchy-personal-update.timer`
  - El hook debe:
    - Incluir los hashes SHA-256 de los tres archivos fuente para dispararse solo cuando cambien.
    - Verificar `$EUID -eq 0` o `sudo -n true`; si no hay privilegios, avisar y salir con código `0`.
    - Usar `cmp -s` antes de instalar cada archivo con `install` para solo copiar si hay cambios; ejecutar `systemctl daemon-reload` únicamente si cambió alguna unidad; y ejecutar `systemctl enable --now omarchy-personal-update.timer` únicamente si hubo cambios o el timer no está habilitado/activo.
    - Extender los tests del repo (`make test` / `make check`) para validar el renderizado con `chezmoi execute-template` y la salida `0` sin `sudo`.
- **Criterios de aceptación:**
  - [ ] `run_onchange_after_04-install-omarchy-personal-updater.sh.tmpl` versionado y cubierto por pruebas (`make test` / `make check` en verde).
  - [ ] `chezmoi apply --dry-run` (y verificación en usuario sin `sudo` como `niri-test` o equivalente) pasa sin errores y sin filtrar `scripts/` al `$HOME`.
  - [ ] `chezmoi apply` con privilegios (y segunda ejecución idempotente) deja `systemctl is-enabled omarchy-personal-update.timer` en `enabled`.
  - [ ] Sub-PR mergeado a `236-omarchy-personal-updater` en `robert-flo/rf-dotfiles`.

## Cierre del Spec (PR final)

- Abrir PR final en `robert-flo/rf-dotfiles`: base `master`, head `236-omarchy-personal-updater`, redactado con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`) referenciando `robert-flo/HR-CLAVIS#236`, `#237`, `#238`.
- Escribir `/z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-236/wk.md` con URL del PR final, `head sha` y resumen de pruebas ejecutadas.
- RV revisa el PR final y publica `APRUEBO <sha>` o `BLOQUEO`.
- PM evalúa `## Merge Danger` y procede según política de Merge Gate.
