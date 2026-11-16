# PJ-CLAVIS — Home Repo

Este es el **Home Repo** del ecosistema **PJ-CLAVIS** (siguiendo el [Home Repo Pattern](https://www.aihero.dev/the-home-repo-pattern)).
Desde aquí se inician las sesiones de agentes de desarrollo IA (`MonoCode`, `T3 Code`, `herdr`, `Claude Code`, `Codex`, `Antigravity`) para orquestar cambios multi-repositorio con una única fuente de contexto, reglas y skills compartidas.

---

## Reglas Globales del Ecosistema

1. **Git y Ramas:**
   - Rama por defecto de repositorios propios: `master` (nunca `main`).
   - Rama de trabajo en forks propios: `personal`.
   - Nunca hacer commit directo a `master` en producción: usar rama auxiliar / worktree → PR → rebase → merge → limpiar rama.
   - Sin trailers `Co-authored-by` ni atribución automática a agentes en commits.
   - Nunca abrir PRs hacia `StatIndet` (no acepta PRs generados con IA).
2. **Idempotencia y Cero Adivinanzas:**
   - Toda automatización debe ser 100% reproducible e idempotente (`make converge`, `chezmoi apply`).
   - La única puerta a `/` es `pacman` vía `[omarchy-personal]` (ADR 0001 / ADR 0002).
3. **Topología Canónica:**
   - Bare clones y worktrees activos: `/z/bare/<repo>.git` y `/z/repos/<repo>/` (`ravn-git-bare-clone`).
   - Experimentos rápidos: `/z-drive/tries/` (`~/Work/tries`, `try-clone`).
   - Zona temporal efímera: `/zt/` (prohibido mantener trabajo permanente aquí).
   - Ver la especificación completa en `/z/vaults/PJ-NIRI-rf-omarchy-workstation/08 - AI Developer Workflows/08.3 - Topologia Canonica de Repositorios, Worktrees y GitHub Workflows.md`.

---

## Other Repos (Punteros de Navegación)

- **workstation** (Vault de arquitectura, ADRs, workflows y proyectos): `/z/vaults/PJ-NIRI-rf-omarchy-workstation`. Clonar desde `https://github.com/robert-flo/PJ-NIRI-rf-omarchy-workstation.git` si falta. *Leer su `AGENTS.md` primero.* *(Renombre diferido a `PJ-CLAVIS-rf-workstation`).*
- **dotfiles** (Chezmoi SSoT y `packages.json` para `niri`, `niri-classic`, `omarchy`): `/z/rf-dotfiles`. Clonar desde `https://github.com/robert-flo/rf-dotfiles.git` si falta. *Leer su `AGENTS.md` primero.* *(Renombre diferido a `PJ-CLAVIS-rf-dotfiles`).*
- **pj-omarchy** (Meta-workspace Omarchy, `fo-omarchy`, `omarchy-personal-repo` y skills compartidas): `/z/pj-omarchy`. *Leer su `AGENTS.md` primero.*
- **forkhub (proyecto en transición)** (Orquestador SSoT de forks, bare clones y worktrees): actualmente en `/z/vaults/PJ-NIRI-rf-omarchy-workstation/scripts/forkhub/` mientras se extrae hacia `/z/repos/PJ-CLAVIS-rf-forkhub` y `/z/repos/PJ-CLAVIS-rf-forkhub-arch`. Ver `/z/vaults/PJ-NIRI-rf-omarchy-workstation/proyectos/forkhub.md`.
- **nvims** (Suite Neovim compartida `rvim`, `dotfiles-craftzdog`, `nvim2k`): `/z/nvims`.
- **tries** (Experimentos y PoCs rápidos): `/z-drive/tries` (también `~/Work/tries`).
- **opt-src** (Fuentes de compilación de forks del escritorio): `/opt/src`.
