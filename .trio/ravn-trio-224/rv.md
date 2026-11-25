# Revisión RV — Spec #224 (`robert-flo/ravn-trio#75`)

- **PR:** https://github.com/robert-flo/ravn-trio/pull/75
- **Spec:** `robert-flo/HR-CLAVIS#224`
- **Base → Head:** `main` ← `224-fleet-estado-verdadero` (`2de2a91b3620531bfa0852d98eadaa26780652de`)
- **Veredicto:** `APRUEBO 2de2a91b3620531bfa0852d98eadaa26780652de`

## Hallazgos y verificación contra el spec y `plan.md`

1. **Sub-issue #225 (`trio-status` y `trio.conf`):**
   - `runtime/scripts/status` acepta `--owner`, `--branch`, `--last-commit`, `--note`, `--heartbeat`, `--worktree`, documenta todos los flags en `--help` y los persiste en `status.json` preservando las claves previas (`$p + ...`).
   - `trio.conf.example` y `runtime/lib/trio_config.sh` definen `TRIO_STALE_MINUTES="15"` y `TRIO_QUOTA_PATTERNS` sobrescribibles por capas.
2. **Sub-issue #226 (cruce `herdr` + `git` en `pj-trio fleet`):**
   - `runtime/scripts/fleet` inspecciona el agente correspondiente al `paso` activo (`wk`, `rv`, `pm`, `os`) en `herdr` y revisa las últimas 40 líneas contra `TRIO_QUOTA_PATTERNS` para detectar `sin-cuota`.
   - Resuelve el worktree `git` e inspecciona rama, último commit, `commits_ahead` y antigüedad para calcular `estado_real` (`vivo`, `sin-cuota`, `colgado`, `muerto`, `trabado`, `terminado`) y `step` (`worktree`, `commits`), exponiéndolos en `--json` y en la tabla sin romper `--activos`.
3. **Sub-issue #227 (cruce `gh` y `--no-gh`):**
   - `runtime/scripts/fleet` y `runtime/commands.tsv` soportan `--no-gh`.
   - Sin `--no-gh` y para tickets con `estado != "hecho"`, consulta `gh` para inferir `step` (`pr-abierto`, `en-review`, `mergeado`) y confirmar `estado_real="terminado"` cuando el PR está `MERGED` y el issue `CLOSED`.
4. **Reglas del repo y del proyecto:**
   - Base correcta (`main`), `MERGEABLE`, sin `Co-authored-by` ni trailers de agente en los 4 commits (`8d83f69`, `5be121d`, `e21cbdb`, `2de2a91`).
   - `herdr-plugin.toml` actualizado a `0.1.21` y título con `· v0.1.21` según `ravn-trio/AGENTS.md`.
   - Sin imágenes commiteadas ni secretos; body del PR con `## Summary`, `## Evidence` y `## Merge Danger` (`two-way` / `Low`).

## Pruebas corridas en el worktree efímero `rv-75` (`2de2a91b3620531bfa0852d98eadaa26780652de`)

- `make check` (`shellcheck -x` + `bash tests/run.sh`, 23/23 suites incluyendo `test-status.sh`, `test-config-capas.sh` y `test-fleet.sh`): **PASS** (exit 0).
- Prueba real en vivo sobre `/z-home-repos/HR-CLAVIS`:
  - `pj-trio fleet /z-home-repos/HR-CLAVIS --activos --no-gh --json`: detectó `estado_real: "vivo"`, `step: "commits"`, `owner: "rv-ravn-trio-224"`, `workspace_id: "w2"`.
  - `pj-trio fleet /z-home-repos/HR-CLAVIS --activos --json`: completó `step: "en-review"` cruzando GitHub (`gh`).
- CI en GitHub (`ShellCheck Lint`, `Test Suite`, `Test Installation Target`): 3/3 en verde.
