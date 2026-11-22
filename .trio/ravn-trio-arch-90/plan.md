# Plan — robert-flo/HR-CLAVIS#90 (`ravn-trio-arch`)

- **Ticket:** https://github.com/robert-flo/HR-CLAVIS/issues/90
- **Repo:** `robert-flo/ravn-trio-arch`
- **Tag objetivo:** `v0.1.17`

## Hallazgos
1. `robert-flo/ravn-trio-arch` ya fue actualizado automáticamente a `0.1.17-1` en `main` (commit `01def00`: `chore: update package to v0.1.17-1`, `PKGBUILD` y `.SRCINFO` al día con sha256 `73d85365d925f49c9f1e7253bd9dcacf7e5e8ca65b1ce05c8f2e7640f062abe9`).
2. El workflow `Package from upstream tag` (run `44986764445`) terminó en `success` y publicó `ravn-trio 0.1.17-1` en `[omarchy-personal]`.
3. Se instaló `ravn-trio 0.1.17-1` en la estación (`sudo pacman -Syy --noconfirm ravn-trio`, registrado en `/z-home-repos/CAMBIOS.md`) y `/usr/bin/pj-trio start --help` ya muestra `--kind <agy|cursor>` (y `--ticket-repo <owner/repo>`).
4. No se requieren cambios de código ni PR en `robert-flo/ravn-trio-arch`. Se limpia el worktree `90-pkg-v0.1.17` y se cierra `robert-flo/HR-CLAVIS#90`.
