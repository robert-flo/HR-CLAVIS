# Plan — Ticket #112 (`robert-flo/HR-CLAVIS#112`)

## Contexto
- **Issue:** `robert-flo/HR-CLAVIS#112` — `[workstation] Reparar los 4 checks rojos de CI en PJ-NIRI-rf-omarchy-workstation`
- **Repo de código / PR:** `robert-flo/PJ-NIRI-rf-omarchy-workstation`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/PJ-NIRI-rf-omarchy-workstation/112-fix-ci-checks`
- **Rama:** `issue/112-fix-ci-checks`
- **Base:** `origin/HEAD` (`master` @ `f8e6788`)

## Qué hay que hacer
Reparar los 4 checks preexistentes que están fallando en el CI de `PJ-NIRI-rf-omarchy-workstation`:

1. **Contenedor Arch en `.github/workflows/ci.yml`** (`Idempotencia de Onboarding` y `Tests de secretos (age)`):
   - Fallan en `Instalar dependencias en contenedor` (`Errors occurred, no packages were upgraded`) por usar `pacman -Sy --noconfirm ...` (partial upgrade) en lugar de `pacman -Syu --noconfirm ...` (o actualizar keyring si hace falta).
2. **ShellCheck**:
   - Corregir el warning `SC2097` / `SC2098` en `scripts/omarchy-onboarding/onboard-machine.sh` línea 729 (separando la asignación o usando `env` según corresponda).
3. **Validación del Vault (`scripts/validate_vault.py`)**:
   - Resolver los 9 wikilinks rotos preexistentes sin inventar contenido falso:
     - En `04.8`, `04.9`, `07.1`: los 4 enlaces `[[PENDIENTE - ...]]` (convertirlos a texto plano o notas stub / quitar los corchetes `[[...]]` si son ítems pendientes).
     - En `08.0 - AI Developer Workflows (MOC).md`: los 5 enlaces a notas aún no creadas (`08.1`, `08.2`, `08.4`, `08.5`, `08.6`) — dejarlos como ítems pendientes sin wikilink roto (revisar con `validate_vault.py` que pase con 0 errores críticos).

## Criterios de aceptación
- Los 4 checks de `.github/workflows/ci.yml` (`Idempotencia de Onboarding`, `Tests de secretos (age)`, `ShellCheck`, `Validación del Vault`) pasan en verde en el PR.
- `make audit`, `make test-idempotence` y `make test-secrets` en verde (con la salida incluida en el PR) y `shellcheck` limpio (`set -euo pipefail` en cada script que se toque).
- `python3 scripts/validate_vault.py` pasa sin errores críticos.
- El body del PR sigue la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`) y referencia `Closes robert-flo/HR-CLAVIS#112` (o `Refs robert-flo/HR-CLAVIS#112` si es cross-repo; aclarar que cierra `robert-flo/HR-CLAVIS#112`).
- Commits sin trailers `Co-authored-by` ni atribución a agentes.

## Entregables del WK
1. Cambios implementados, probados localmente y pusheados en `issue/112-fix-ci-checks`.
2. PR abierto en `robert-flo/PJ-NIRI-rf-omarchy-workstation` contra `master` (listo para revisión, no dejarlo en draft al terminar) con CI en verde.
3. Archivo `/z-home-repos/HR-CLAVIS/.trio/PJ-NIRI-rf-omarchy-workstation-112/wk.md` con URL del PR, head SHA y resultado de las pruebas (incluyendo estado de CI).
