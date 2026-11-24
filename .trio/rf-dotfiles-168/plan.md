# Plan del Spec robert-flo/HR-CLAVIS#168

- **Spec:** `robert-flo/HR-CLAVIS#168` — Makefile modular estilo Rollo en `rf-dotfiles`
- **Repo destino:** `robert-flo/rf-dotfiles` (rama base: `master`)
- **Rama del spec:** `168-makefile-modular-rollo`
- **Orden DAG de sub-issues:** `#175` -> `#178` -> `#180`

## Sub-issues y criterios de aceptación

### 1. `#175` — rf-dotfiles: blindar `.chezmoiignore.tmpl` y base `Makefile` + `help` estilo Rollo
- **Blocked by:** Ninguno (arranca primero)
- **Alcance:** Proteger `Makefile` y `make/**` en `.chezmoiignore.tmpl` bajo ignores globales, y crear `Makefile` raíz + `make/help.mk` usando la paleta visual `Rollo` (`.DEFAULT_GOAL := help`, colores ANSI, divisores de 80 caracteres, `DRY_RUN ?= 0`, salida categorizada y `Quick Actions:`).
- **Criterios de aceptación:**
  - `.chezmoiignore.tmpl` incluye `Makefile` y `make/**` bajo ignores globales.
  - `make help` en `rf-dotfiles` renderiza targets categorizados con formato `Rollo` y sale con código 0.
  - `chezmoi managed -S .` (probado entre perfiles) no incluye `Makefile` ni `make/*`.
  - `chezmoi execute-template < .chezmoiignore.tmpl` pasa sin errores de plantilla.

### 2. `#178` — rf-dotfiles: módulo `chezmoi.mk` (`status`, `diff`, `apply` por perfil y `verify`)
- **Blocked by:** `#175`
- **Alcance:** Agregar `make/chezmoi.mk` e integrarlo en `Makefile` para exponer `status`, `diff`, `apply`, `apply-clavis` / `apply-nyxuri` / `apply-omarchy`, `managed`, `ignored` y `verify`, todos con soporte `DRY_RUN=1`, headers/footers con `ifndef EMBEDDED` y `Quick Actions:`.
- **Criterios de aceptación:**
  - `make status` y `make diff` corren limpio usando el repo como fuente chezmoi (`-S $(CURDIR)`).
  - `make apply DRY_RUN=1` y `make apply-{clavis,nyxuri,omarchy} DRY_RUN=1` pasan sin mutar `$HOME`.
  - `make managed`, `make ignored` y `make verify` pasan limpio y confirman que `Makefile` / `make/*` siguen ignorados.

### 3. `#180` — rf-dotfiles: módulos `packages.mk`, `lint.mk`, `doctor.mk`, `git.mk` y target `check`
- **Blocked by:** `#178`
- **Alcance:** Agregar `make/packages.mk` (`packages-check`, `packages-summary`, `packages-missing` sobre `.chezmoidata/packages.json`), `make/lint.mk` (`lint` vía `shellcheck`, `test` vía `scripts/tests/`, y `check` compuesto), `make/doctor.mk` (`doctor`) y `make/git.mk` (`git-status`), conectados en `Makefile` con estilo `Rollo` y `DRY_RUN=1`.
- **Criterios de aceptación:**
  - `make packages-check`, `make packages-summary` y `make packages-missing` parsean `.chezmoidata/packages.json` con `jq` limpio.
  - `make lint` (`shellcheck` en `scripts/yt-*`, `scripts/yt-common.sh`, `scripts/tests/*.sh`) y `make test` (`scripts/tests/`) pasan en verde.
  - `make doctor`, `make git-status` y el compuesto `make check` pasan en verde.
