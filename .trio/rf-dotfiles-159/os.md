# OS · rf-dotfiles #159

Ticket [robert-flo/HR-CLAVIS#159](https://github.com/robert-flo/HR-CLAVIS/issues/159), cerrado, sin etiquetas de triage.

PR [robert-flo/rf-dotfiles#120](https://github.com/robert-flo/rf-dotfiles/pull/120) mergeado por squash.

- Head: `f23d8b0d90b18db18825a572970c71f47c8db967`
- Merge: `b91b367c04e9995bb87b7d0031f1087040efaa70`
- Merge Danger: two-way / Low

## Qué quedó

`dot_config/agy/executable_statusline.sh` muestra la cuota que queda (entero más cercano), el tiempo compuesto (`Nd Nh`, `Nh Nm`, `Nm`) y color en el renglón 2 (verde ≥50, amarillo ≥20, rojo por debajo). El renglón 3 queda en texto plano, sin `renueva`. El hook deja `statusLine.type` en `command`.

## Prueba

JSON sintético: `gemini-weekly` 0.89 / 584176 s, `gemini-5h` 0.99 / 14880 s, 3p en 1.

Antes (`master`):

```
Gemini 3.8 Flash medium ░░░░░░░░░░ 0%
  Gemini 5h: 1% usado · reset en 4h | Gemini semanal: 11% usado · reset en 6d
  3p 5h: 0% usado. | 3p semanal: 0% usado.
```

Después (script del worktree y `~/.config/agy/statusline.sh` aplicado):

```
Gemini 3.8 Flash medium ░░░░░░░░░░ 0%
  📅 Semanal disp.: 89% (renueva en 6d 18h) │ ⚡ Ventana (5h) disp.: 99% (renueva en 4h 8m)
  3p 5h: 100% disp. | 3p semanal: 100% disp.
```

`bash -n` y `shellcheck` en 0. `chezmoi diff` de `~/.config/agy/statusline.sh` solo mostraba este cambio. No hay CI en el repo; el PR quedó `MERGEABLE` / `CLEAN`. `code-review`: sin violaciones de estándar; el color de más en el renglón 3 se quitó antes del PR.
