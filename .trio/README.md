# .trio/ — handoffs del trío PM/WK/RV en herdr

Cada ticket que corre el trío deja acá su carpeta `.trio/<ticket>/` (plan, reporte del WK, veredicto del RV), y se commitea a git.

## Cómo lanzarlo

Desde la estación de trabajo, con `pj-trio` (instalado como `/usr/bin/pj-trio` por el paquete [`ravn-trio-arch`](https://github.com/robert-flo/ravn-trio-arch); fuente canónica: [`robert-flo/ravn-trio`](https://github.com/robert-flo/ravn-trio/blob/main/pj-trio)):

```bash
pj-trio HR-CLAVIS <ticket> [--repo <miembro>]
```

- `<ticket>`: número del issue a trabajar.
- `<miembro>`: el `dir` del repo en `proyecto.yml` donde cae el ticket.

Abre en herdr las pestañas `pm`, `wk` y `rv`, cada una con `agy --agent PM-HR-CLAVIS` / `WK-HR-CLAVIS` / `RV-HR-CLAVIS` (agentes `pm-hr-clavis`, `wk-hr-clavis`, `rv-hr-clavis`). El estado se ve con `trio-status`.

Regla de merge (la misma de `AGENTS.md`): el PM mergea solo cuando Roberto lo ordena, sobre un PR con `ready-to-merge` y después del `APRUEBO <sha>` del RV, con `gh pr merge <N> --squash --delete-branch --match-head-commit <sha>`. Sin la orden de Roberto no se mergea, aunque haya `APRUEBO`.
