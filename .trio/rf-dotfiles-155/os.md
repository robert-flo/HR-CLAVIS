# rf-dotfiles-155

Ticket [robert-flo/HR-CLAVIS#155](https://github.com/robert-flo/HR-CLAVIS/issues/155). Cambio en `dot_config/cursor/executable_statusline.sh`.

## PR

- https://github.com/robert-flo/rf-dotfiles/pull/117
- Head: `48ec64ef9afd318048cde95f737e46812d3cd507`
- Merge: `9aabbe25293f0f092e649587931dbc7723e9ce32` (squash a `master`)
- Merge Danger: two-way / Low
- Issue cerrado, sin etiquetas de triage.

## Prueba

`bash -n` y `shellcheck` limpios.

JSON de sesión Grok 4.7 256K Medium, contexto 42%, y caché con total 52, auto 56, api 0, grok 0, ciclo en 9 días y grok en 5 días:

```text
Grok 4.7 256K medium ▓▓▓▓░░░░░░ 42% /tmp/.../HR-CLAVIS ⎇ master
  Cursor: 52% usado · reset en 9d | <bot>: 0% usado · reset en 5d
  Balde Auto: 56% usado. | Balde API: 0% usado.
```

El segundo renglón usa el nombre del bot de la cuota, `usado` y `5d`, como en el prototipo del issue.

No imprime `5h`, `ritmo`, `cuota grok` ni `Fin de ciclo`.

## code-review

Standards y spec, contra `origin/master`, sin hallazgos pendientes. El repo no tiene checks de CI (`statusCheckRollup` vacío, PR `MERGEABLE`).
