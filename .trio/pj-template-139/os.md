# OS Report — robert-flo/HR-CLAVIS#139

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/139 (cerrado)
- **PR:** https://github.com/robert-flo/pj-template/pull/95 (mergeado por squash)
- **Head SHA:** `7f30aa635046c3fca7449db000f6a29b6772c61c`
- **Merge SHA:** `be5f62933d76c2a6198c160d457d44e53c14a773`
- **Merge Danger:** `two-way / Low`

## Cambio realizado
- Documentado en `.agents/reglas/pm.md` (paso 7, fuente canónica en `pj-template` distribuida vía `template-paths.txt`) el protocolo de recuperación del PM Orquestador cuando `pj-trio watch` despierta con `blocked` en el paso `plan`:
  1. Inspeccionar con `herdr agent get` y `herdr pane read`.
  2. Reenviar el prompt de `.trio/<clave>/kickoff.md` con `herdr agent prompt` si el agente está listo sin orden.
  3. Relanzar limpio (`herdr workspace close` + re-despacho de `pj-trio` / `pj-trio --oneshot`) si el panel murió o no responde, escalando a Roberto solo si vuelve a fallar por entorno.

## Evidencia
- `make check` local en verde (`check: todo bien`).
- `code-review` (ejes Standards y Spec en paralelo): limpio, 0 hallazgos.
- CI `check/check (pull_request)` en GitHub Actions: verde (40s) sobre `7f30aa635046c3fca7449db000f6a29b6772c61c`.
