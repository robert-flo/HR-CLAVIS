# .trio/ — handoffs del trío PM/WK/RV en herdr

Cada ticket que corre el trío deja acá su carpeta `.trio/<clave>/` (`plan.md`, `wk.md`, `rv.md`, `status.json`), y se commitea a git. `<clave>` es `<repo hijo>-<ticket>` si se lanzó con `--repo`, o `<ticket>` si no.

## Cómo lanzarlo

Desde la estación de trabajo, con `pj-trio` (instalado como `/usr/bin/pj-trio` por el paquete `ravn-trio` de `[omarchy-personal]`; fuente de verdad: `robert-flo/ravn-trio`):

```bash
pj-trio /z-home-repos/HR-CLAVIS <ticket> --repo <dir del repo hijo> [--base <rama del spec>]
```

- Pasá la ruta del Home Repo: `pj-trio` busca por nombre solo en `$PJ_TRIO_ROOTS` (por defecto `/z-drive/tries`).
- `<ticket>`: con `--repo`, número del issue **en ese repo hijo**; sin `--repo`, de HR-CLAVIS.
- `<dir del repo hijo>`: el `dir` del repo en `proyecto.yml` (`PJ-NIRI-rf-omarchy-workstation` o `rf-dotfiles`).
- `--base`: la rama del spec si el ticket es un sub-issue; sin `--base`, el worktree sale de `origin/HEAD`.

Abre en herdr las pestañas `pm`, `wk` y `rv`, cada una con `agy --agent PM-HR-CLAVIS` / `WK-HR-CLAVIS` / `RV-HR-CLAVIS` (agentes de herdr `pm|wk|rv-<miembro corto>-<ticket>`). El estado se lee en `.trio/<clave>/status.json` (lo escribe el PM con `trio-status`, que no es un visor).

Regla de merge (la misma de `AGENTS.md` §7.3): el PM mergea tras el `APRUEBO <sha>` del RV, con `gh pr merge <N> --squash --delete-branch --match-head-commit <sha>`, sin esperar la orden de Roberto.
