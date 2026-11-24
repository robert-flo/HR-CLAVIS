# Plan — Spec #169 (`robert-flo/HR-CLAVIS#169`)

- **Repo de trabajo:** `robert-flo/PJ-NIRI-rf-omarchy-workstation` (rama base: `master`)
- **Rama del spec:** `169-makefile-modular-rollo`
- **Orden DAG de sub-issues:** `#176` -> `#179`

## Sub-issues y criterios de aceptación

### 1. `#176` — workstation: modularizar Makefile base + help.mk + install.mk + doctor.mk estilo Rollo
- **Blocked by:** None
- **Qué construir:** Refactorizar el `Makefile` raíz monolítico en un orquestador estilo `Rollo` con `make/help.mk`, `make/install.mk` (`install`, `install-padre`, `install-hija`, `converge`, `audit`, `test-idempotence`) y `make/doctor.mk` (las 7 comprobaciones de sistema), preservando los nombres de targets y llamadas a scripts existentes y agregando soporte `DRY_RUN=1`.
- **Criterios de aceptación:**
  - `make help` renderiza los targets categorizados dinámicamente con la paleta `Rollo` y sale con código 0.
  - Todos los targets en `install.mk` y `doctor` mantienen sus nombres exactos e invocaciones de scripts, y soportan `DRY_RUN=1`.
  - `make -n` tiene éxito para todos los targets migrados y `make audit` pasa en verde.
  - `make doctor` ejecuta las 7 comprobaciones con encabezados y divisores uniformes estilo `Rollo`.

### 2. `#179` — workstation: módulos secrets.mk, forks.mk, sync.mk, sandbox.mk y quality.mk
- **Blocked by:** `#176`
- **Qué construir:** Completar la modularización extrayendo `secrets.mk` (`secrets`, `lock-secrets`, `test-secrets`), `forks.mk` (`forks`, `forks-audit`, `forks-sync`), `sync.mk` (`sync`, `sync-replica`, `sync-status`), `sandbox.mk` (`sandbox-status`, `sandbox-reset`), y agregando `quality.mk` (`vault-validate`, `lint` con `shellcheck`), siguiendo la paleta `Rollo` y el estándar `DRY_RUN=1`.
- **Criterios de aceptación:**
  - Todos los targets legacy en `secrets`, `forks`, `sync` y `sandbox` preservan nombres exactos e invocaciones de scripts, y soportan `DRY_RUN=1`.
  - `make test-secrets`, `make vault-validate` y `make lint` (`shellcheck` sobre `scripts/`) pasan en verde.
  - `make help` lista cada módulo limpiamente sin targets sobrantes en el `Makefile` raíz.

## Criterios del PR final (`169-makefile-modular-rollo` -> `master`)
- Todos los 18 targets legacy preservan nombre y comportamiento exacto.
- `make -n` y `DRY_RUN=1` verificados en todos los targets legacy y los nuevos de calidad.
- `make audit`, `make test-secrets`, `make vault-validate` y `make lint` (`shellcheck`) en verde.
- `set -euo pipefail` y `shellcheck` limpio en cualquier script tocado (si aplica).
