# OS · rf-dotfiles #153

Ticket [robert-flo/HR-CLAVIS#153](https://github.com/robert-flo/HR-CLAVIS/issues/153), trivial. PR [robert-flo/rf-dotfiles#116](https://github.com/robert-flo/rf-dotfiles/pull/116).

- Head: `61e86961318115ed10df3fabfc1a10474dd5f94a`
- Merge: `c52a99bab2d0a608ce7f6120246779e97135a1d4` (squash)
- Merge Danger: two-way / Low
- Issue cerrado, sin etiquetas de triage.

## Qué cambió

En `dot_config/cursor/executable_statusline.sh` cada token del modelo se imprime una vez (`canon_token` + `take_token`). La cuota de Grok dice `cuota grok usada 0% · resetea en 5d`, o `cuota grok agotada` si no hay cupo. Sin dato, el segmento no sale.

## Evidencia

Antes, JSON `display_name` `Grok 4.7 256K Medium` y `param_summary` `256K Medium`, ancho 80, caché `grok_pct=0` / disponible / reset 5 días:

```text
Grok 4.7 256K 256K Medium medium ░░░░░░░░░░ 1% /tmp
5h $0.00 $0.00/h │ auto 1% 0.2x ritmo api 1% │ grok 0% 5d
```

Después, el mismo JSON:

```text
Grok 4.7 256K medium ░░░░░░░░░░ 1% /tmp
$0.00 1% 0.2x │ cuota grok usada 0% · resetea en 5d
```

`bash -n` y `shellcheck` en 0. `chezmoi apply` del script en esta máquina; el source `~/.local/share/chezmoi` quedó en `c52a99b` y el archivo vivo coincide.

## Review

Standards: sin violaciones duras. El clasificador de esfuerzo quedó en `take_token` y la normalización en `canon_token`. Spec: los tres puntos del ticket quedaron cubiertos.
