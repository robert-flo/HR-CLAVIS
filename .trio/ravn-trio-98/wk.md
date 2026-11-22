# Reporte WK — robert-flo/HR-CLAVIS#98

- **PR:** https://github.com/robert-flo/ravn-trio/pull/60
- **Head SHA:** `778e01d87ccd5c8142103ab821ee7e72c7afc4b8`
- **Rama:** `issue/98-ticket-repo`
- **Worktree:** `/z/repos/ravn-trio/98-ticket-repo`
- **Versión:** `0.1.17` (`herdr-plugin.toml`)

## Cambios realizados
- `runtime/scripts/start`: soporte de `--ticket-repo <owner/repo>` y `--ticket-repo=<owner/repo>`, validación estricta `^[A-Za-z0-9._-]+/[A-Za-z0-9._-]+$` antes de tocar disco/`herdr`/red, ayuda y salida `--dry-run`.
- `runtime/commands.tsv`: agregado `--ticket-repo` a `start` y `kickoff`.
- `runtime/lib/trio_project.sh`: separación de `pr_repo` e `issue_repo` (`issue_repo="${ticket_repo_arg:-$pr_repo}"`, manteniendo retrocompatibilidad cuando se omite).
- `runtime/lib/trio_config.sh`: sustitución de `{{ISSUE_REPO}}`, `{{PR_REPO}}`, `{{WK_ISSUE_REF}}`, `{{OS_ISSUE_REF}}` y fallback `{{TICKET_REPO}}`.
- `runtime/lib/trio_herdr.sh`: agregado `--ticket-repo` en el mensaje de recuperación manual.
- `trio-pm-prompt.md` y `trio-os-prompt.md`: separación de `-R {{ISSUE_REPO}}` (issues) y `-R {{PR_REPO}}` (PRs), e indicación de referenciar `{{ISSUE_REPO}}#{{TICKET}}` cuando `ISSUE_REPO != PR_REPO`.
- `herdr-plugin.toml`: bump a `0.1.17`.
- `tests/test-ticket-repo.sh` y `tests/test-pm-prompt-merge.sh`: cobertura de `--ticket-repo`, casos negativos, retrocompatibilidad y versión `0.1.17`.

## Pruebas
- `make lint` (`shellcheck -x ...`): verde sin warnings.
- `make test` (`bash tests/run.sh`, 17 suites incluyendo `tests/test-ticket-repo.sh`): `All tests passed successfully!`.
- CI de GitHub en PR #60 (`778e01d87ccd5c8142103ab821ee7e72c7afc4b8`):
  - `CI/ShellCheck Lint`: pass (14s)
  - `CI/Test Installation Tar...`: pass (4s)
  - `CI/Test Suite`: pass (29s)
