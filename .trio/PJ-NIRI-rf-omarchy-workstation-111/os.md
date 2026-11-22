# OS Handoff — `PJ-NIRI-rf-omarchy-workstation#111` (`robert-flo/HR-CLAVIS#111`)

- **Issue origen:** `robert-flo/HR-CLAVIS#111` (`[workstation] Agregar guía de keybindings y scripts de herdr en 08 - AI Developer Workflows`)
- **PR:** https://github.com/robert-flo/PJ-NIRI-rf-omarchy-workstation/pull/170
- **Rama / Head SHA:** `issue/111-herdr-guia` @ `c8661946df1d0957bfe2a3fe3e3235ebc3c59c75`
- **Merge SHA:** `f8e6788eca428dd21123f1ac852766f05732dee8`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`
- **Estado:** `hecho` (mergeado por Roberto; cierre completado y worktree eliminado)

## Qué se hizo

1. Se creó `08 - AI Developer Workflows/08.10 - Guía de Keybindings y Scripts de herdr.md` con el contenido de `/zt/herdr-guia.md`, frontmatter YAML completo, navegación superior (`08.9`, `08.0`, `00.0`, `08.8`) y callout nativo `> [!INFO]`.
2. Se enlazó `08.10` en la tabla de `08 - AI Developer Workflows/08.0 - AI Developer Workflows (MOC).md`.
3. Se ejecutó `code-review` (Standards + Spec) y se corrigieron los 2 hallazgos de formato Obsidian (`Anterior: [[08.9 ...]]` y callout nativo `> [!INFO]`).

## Pruebas locales

- `make audit` (`sudo ./scripts/omarchy-onboarding/converge-workstation.sh --dry-run`): **OK** (`[✓] Simulación de convergencia de capas completada exitosamente.`).
- `bash ./scripts/omarchy-onboarding/ci-idempotencia.sh` (Fase 2 de `make test-idempotence`, sin `sudo` porque `sudo` ejecuta el bootstrap `pacman -Sy` de contenedor): **OK** (`==> ✅ Todas las pruebas de idempotencia e inocuidad pasaron exitosamente.`).
- `make test-secrets`: no completado porque `./scripts/secrets/probar-descifrado.sh` solicita passphrase maestra interactiva por `stdin`.

## Por qué no se auto-mergeó

Por política del Merge Gate (`os.md`), no se auto-mergea si CI no está en verde o si falta una prueba requerida:

1. Los 4 checks de CI del run `44995297423` fallaron por causas **preexistentes en `master`**, ajenas a este PR:
   - `CI/Idempotencia de Onboarding` y `CI/Tests de secretos (age)`: fallan en el contenedor Arch al correr `pacman -Sy` sin `-Syu` (`openssl 4.1.0-1` rompe `libcrypto.so=3-64` en `pacman`).
   - `CI/Validación del Vault`: falla por 9 wikilinks rotos preexistentes (`PENDIENTE - ...` y `08.1`, `08.2`, `08.4`–`08.6`); `08.10` no reportó errores.
   - `CI/ShellCheck`: falla sin que este PR modifique scripts.
2. `make test-secrets` requiere entrada interactiva de passphrase.
