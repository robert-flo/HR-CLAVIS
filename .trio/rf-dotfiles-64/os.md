# OS report — rf-dotfiles / HR-CLAVIS#64

## Resultado
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/76 — MERGED
- **Head SHA:** `45fbf5713950b8b9c1431dcc4fe77fa13a8e67fb`
- **Merge SHA:** `06ad93b97b2f741253ecd28e53aba266440cea9a`
- **Merge Danger:** Door two-way / Blast Radius Low → auto-merge OK
- **Issue:** [HR-CLAVIS#64](https://github.com/robert-flo/HR-CLAVIS/issues/64) cerrado; `ready-for-agent` quitada

## Cambios
1. `.personal/niri/config.kdl` — binds invertidos (Kitty solo Mod+T; Ghostty en Return/Grave/Ctrl+Return/Shift+N).
2. `dot_local/bin/executable_clavis-scratch-toggle.sh` — con arg `ghostty` spawnea Ghostty (`--class=com.clavis.scratchpad --title=Scratchpad`, herdr/tmux/fallback).
3. `window-rule` scratchpad — match ampliado a `^(scratchpad|com\.clavis\.scratchpad)$` porque Ghostty rechaza `class=scratchpad` (no es GTK Application ID válido).

## Evidence
- `chezmoi diff` (targets niri config + script): solo hunks de este ticket.
- Backup + bitácora: `~/.config/niri/config.kdl.bak-20261121-1742`, script `.bak-…`, línea en `/z-home-repos/CAMBIOS.md`.
- `chezmoi apply` en `niri` OK; `grep` confirma los cinco binds nuevos.
- `niri validate` (`niri`): `INFO niri: config is valid`.
- Scratch live: `clavis-scratch-toggle.sh ghostty` → `app=com.clavis.scratchpad title=Scratchpad floating=true`; segundo toggle cierra.
- `niri-test`: apply de targets OK; `niri validate -c` falla por include preexistente `clavis/outputs.kdl` (fuera de alcance); binds en disco correctos.
- code-review: 0 hard / 1 judgement (duplicación spawn kitty/ghostty).
- Commit sin `Co-authored-by` (reescrito con `commit-tree` tras inyección de Cursor).

## Worktree
- `/z/repos/rf-dotfiles/64-invert-ghostty-kitty-binds` (rama `issue/64-invert-ghostty-kitty-binds`) — a remover post-cierre.
