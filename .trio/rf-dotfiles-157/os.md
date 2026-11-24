# rf-dotfiles-157

Ticket [robert-flo/HR-CLAVIS#157](https://github.com/robert-flo/HR-CLAVIS/issues/157). Cambio en `dot_config/cursor/executable_statusline.sh`.

## PR

- https://github.com/robert-flo/rf-dotfiles/pull/119
- Head: `11fe18ce26bf0c7f2a9b7bf1dc420aeebb2cadec`
- Merge: `fed344a7e2548e25b863701e951c83945a86f094` (squash a `master`)
- Merge Danger: two-way / Low
- Issue cerrado, sin etiquetas de triage.

## Prueba

`bash -n` y `shellcheck` limpios. Caché aislada, sin tocar `~/.cache`.

Antes, con 9 días 14 horas y 4 días 23 horas, en `9aabbe2`:

```text
  Cursor: 52% usado · reset en 9d | Grok Bot: 0% usado · reset en 4d
  Balde Auto: 56% usado. | Balde API: 0% usado.
```

Después:

```text
  📅 Cursor: 52% usado · reset en 9d 14h | 🤖 Grok Bot: 0% usado · reset en 4d 23h
  Balde Auto: 56% usado. | Balde API: 0% usado.
  📅 Cursor: 52% usado · reset en 3h 12m | 🤖 Grok Bot: 0% usado · reset en 40m
  📅 Cursor: 1% usado · reset en 1d 0h | 🤖 Grok Bot: 0% usado · reset en 0m
```

No imprime `reset en 9d` ni `reset en 5d`. Los baldes no llevan emoji.

## code-review

Standards y spec, contra `origin/master`, sin hallazgos duros. El repo no tiene checks de CI (`statusCheckRollup` vacío, PR `MERGEABLE`).
