# Plan — robert-flo/ravn-trio#50

- **Ticket:** [robert-flo/ravn-trio#50](https://github.com/robert-flo/ravn-trio/issues/50) — `feat(watch): despertar por defecto solo en eventos accionables (--verbose para pasos) · v0.1.12`
- **Repo:** `robert-flo/ravn-trio` (rama base: `origin/HEAD` / `main`)
- **Worktree:** `/z/repos/ravn-trio/50-watch-accionables`
- **Rama:** `issue/50-watch-accionables`

## Objetivo

Optimizar `pj-trio watch` para que por defecto absorba en bash (0 tokens) las transiciones intermedias de `paso` (`plan` → `wk` → `rv` → `merge`) y solo despierte al PM (`exit 0`) ante eventos accionables:
- `estado` pasa a `hecho` o `needs-roberto`
- `pm_herdr_status` pasa a `blocked` o `unknown` (estando `en-curso`)

Agregar `--verbose` para conservar el comportamiento anterior (despertar también en cambios de `paso` / `nuevo`).

## Cambios requeridos

1. **`runtime/scripts/watch`:**
   - Agregar opción `--verbose` (y actualizar `show_watch_help`).
   - Por defecto (`verbose=0`), filtrar eventos en `diff_snapshots` (o mediante parámetro) para que solo sean accionables cuando:
     - `estado` cambia a `hecho` o `needs-roberto` (incluyendo un ticket nuevo que ya llegue en `hecho` o `needs-roberto`), o
     - `pm_herdr_status` cambia a `blocked` o `unknown` (estando `en-curso`).
   - Si en el chequeo inicial contra `--snapshot` o dentro del loop hubo cambios intermedios no accionables, actualizar `write_snapshot "$curr_snapshot"` y `baseline="$curr_snapshot"` sin retornar.
   - Con `--verbose`, mantener la detección completa (nuevos, cambios de `estado`, cambios de `paso`, `blocked`/`unknown`).
2. **`herdr-plugin.toml`:**
   - Subir `version` a `"0.1.12"`.
3. **`tests/test-watch.sh`:**
   - Cubrir modo por defecto: absorbe `plan -> wk -> rv` (actualizando `--snapshot`) y despierta en `hecho`, `needs-roberto` y `blocked`.
   - Cubrir modo `--verbose`: despierta en cambios de `paso`.
   - Mantener cobertura de salida legible, alias `w` y timeout `124`.
4. **Verificación y PR:**
   - Correr `make lint` y `make test` en verde dentro del worktree.
   - Abrir PR contra `main` con título `feat(watch): despertar por defecto solo en eventos accionables (--verbose para pasos) · v0.1.12`, `Closes #50`, evidencia de pruebas y `## Merge Danger` (`Door: two-way`, `Blast Radius: Low`).
