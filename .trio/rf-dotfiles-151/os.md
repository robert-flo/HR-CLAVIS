# rf-dotfiles-151

- Issue: https://github.com/robert-flo/HR-CLAVIS/issues/151 (cerrado, sin etiquetas de triage)
- PR: https://github.com/robert-flo/rf-dotfiles/pull/118
- Head: `44a1f610610549ad94582c4a8438bc364a53f47b`
- Merge: `92401135e42e6098c3aa76cc392e42c7d11d9fc3`

## Qué quedó

`dot_config/agy/executable_statusline.sh` imprime los tres renglones del prototipo B. El hook `run_onchange_after_configure-agy-statusline.sh.tmpl` escribe `statusLine.command` y `stack_with_default: false` en `~/.gemini/antigravity-cli/settings.json` sin tocar `trustedWorkspaces` ni `model`. Quedó anotado en la taxonomía común de `AGENTS.md`.

## Evidence

- Antes: no existía el script (`exit 127`) y `statusLine.command` era `python3 ~/.gemini/antigravity-cli/statusline.py`.
- `bash -n` y `shellcheck` limpios en el script y en el hook.
- JSON sintético (contexto 0, remaining 0.99 / 0.89 / 1, reset 14400 y 518400) sobre un repo `HR-CLAVIS` en `master`:

```text
Gemini 3.8 Flash medium ░░░░░░░░░░ 0% HR-CLAVIS ⎇ master
  Gemini 5h: 1% usado · reset en 4h | Gemini semanal: 11% usado · reset en 6d
  3p 5h: 0% usado. | 3p semanal: 0% usado.
```

- Bordes: 172799s → `47h`, 172800s → `2d`, remaining 0.995 → `1%`, rama sucia → `master*`.
- `chezmoi apply --source=<worktree> ~/.config/agy/statusline.sh` dejó `~/.config/agy/statusline.sh` ejecutable. Un apply completo también marcaba niri, clavis y otros archivos, así que el hook se corrió con el script que `chezmoi execute-template` renderizó. `statusLine.command` quedó en `/home/niri/.config/agy/statusline.sh`. `cmp` de `jq -S 'del(.statusLine)'` antes/después: idéntico. Modo `600`.
- Respaldo: `~/.gemini/antigravity-cli/settings.json.bak-20261123-2230`.

## Review

Standards: la ruta no estaba en Común ni en `.chezmoiignore.tmpl`. Se registró como herramienta común. Spec: el diff cubre el issue; no había comportamiento mal implementado. CI: el repo no tiene workflows; `statusCheckRollup` vacío, PR `MERGEABLE`, `mergeStateStatus` CLEAN. Merge Gate: Door two-way, Blast Radius Low.
