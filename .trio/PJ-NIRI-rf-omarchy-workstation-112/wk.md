# WK — Ticket #112 (`robert-flo/HR-CLAVIS#112`)

- **PR:** https://github.com/robert-flo/PJ-NIRI-rf-omarchy-workstation/pull/171
- **Rama:** `issue/112-fix-ci-checks` → `master`
- **Head SHA:** `ee2b04207fbd292e40fd3bab554a8dd2fbc7b2d7`
- **Estado:** Listo para revisión (no draft)

## Cambios realizados
1. `.github/workflows/ci.yml` y `scripts/omarchy-onboarding/ci-idempotencia.sh`: cambio de `pacman -Sy --noconfirm` a `pacman -Syu --noconfirm` en el bootstrap de contenedores `archlinux:latest` para evitar partial upgrades que rompían `openssl`/`libcrypto.so=3-64`.
2. `scripts/omarchy-onboarding/onboard-machine.sh` (línea 729): prefijo `env` en `env DROPIN_PATH="${PACMAN_D}/omarchy-personal.conf" DEV_FLAG=... PACMAN_CONF=... PACMAN_D=... bash "$TMP_H"` para resolver `SC2097` y `SC2098` preservando el comportamiento de `--dry-run`.
3. Vault (`04.8`, `04.9`, `07.1` y `08.0 - AI Developer Workflows (MOC).md`): removidos los 9 wikilinks rotos (`[[PENDIENTE - ...]]` y enlaces a notas futuras `08.1`, `08.2`, `08.4`, `08.5`, `08.6`), dejándolos como texto plano / ítems pendientes.

## Pruebas locales
- `shellcheck scripts/omarchy-onboarding/onboard-machine.sh scripts/omarchy-onboarding/ci-idempotencia.sh` → `SHELLCHECK OK` (exit 0)
- `python3 scripts/validate_vault.py` → `66 notas analizadas, 543 wikilinks auditados, 0 advertencias, 0 errores críticos` (exit 0)
- `bash tests/secrets/test-export-staging-dotfiles.sh` → `RESULTADO: OK` (exit 0)
- `LF_SIN_BATCHPASS=1 bash tests/secrets/test-export-staging-dotfiles.sh` → `RESULTADO: OK` (exit 0)
- `bash scripts/omarchy-onboarding/ci-idempotencia.sh` → `Modo PADRE` y `Modo HIJA` 100% idempotentes, sandbox/pacman.conf/git config intactos (exit 0)
- `make audit` (`sudo ./scripts/omarchy-onboarding/converge-workstation.sh --dry-run`) → `Simulación de convergencia de capas completada exitosamente` (exit 0)
- Nota sobre `make test-secrets`: ejecuta `scripts/secrets/probar-descifrado.sh` (interactivo con frase maestra humana); los tests automatizados de `age` (`tests/secrets/test-export-staging-dotfiles.sh`) pasaron en ambos modos.

## Checks de CI en PR #171 (4/4 en verde)
- `CI/Idempotencia de Onboarding` (24s) — ✓ pass
- `CI/ShellCheck` (7s) — ✓ pass
- `CI/Tests de secretos (age)` (1m5s) — ✓ pass
- `CI/Validación del Vault` (11s) — ✓ pass
