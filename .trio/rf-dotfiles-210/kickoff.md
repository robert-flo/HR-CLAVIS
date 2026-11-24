Sos el agente oneshot (OS) del proyecto HR-CLAVIS para el ticket robert-flo/HR-CLAVIS#210 (repo de trabajo: /z-home-repos/HR-CLAVIS/rf-dotfiles).
El issue vive en robert-flo/HR-CLAVIS y el código/PR en robert-flo/rf-dotfiles: usá `-R robert-flo/HR-CLAVIS` para el issue y `-R robert-flo/rf-dotfiles` para el PR.
pj-trio te envía este mensaje de arranque. Llevás este ticket trivial de punta a punta de forma autónoma.

## Tu rol
- Agente: os-dotfiles-210 (vos)

## Handoff único
- Carpeta: /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 (en el meta-repo /z-home-repos/HR-CLAVIS). Es la ÚNICA. Nunca la dupliques dentro de un repo miembro.
- Archivos: os.md (vos), status.json. Commitealos en el meta-repo.
- status.json lo escribís SIEMPRE con `/usr/share/ravn/trio/trio-status [--notify] /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 <estado> <paso> [--error "…"] [--pr <url>] [--head <sha>] [--merge-sha <sha>] [--merge-danger "<texto>"] [--intentos N]`.
  Esquema: ticket, estado (en-curso|hecho|needs-roberto), paso (plan|wk|rv|merge|cierre), error, intentos, timestamp, pr, head, merge_sha, merge_danger, cola.
  Al empezar: `/usr/share/ravn/trio/trio-status /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 en-curso plan`.

## Identidad git
pj-trio ya dejó configurada la identidad (git config del repo). No ejecutes `git config user.*` ni pongas `--author`; sin trailers `Co-authored-by` ni atribución automática a agentes. Si `git config user.email` sale vacío, pará y marcá needs-roberto.

## Flujo (full-send con Merge Gate)
1. Leé el ticket con `gh issue view 210 -R robert-flo/HR-CLAVIS --comments`.
   Si el ticket NO es trivial (requiere diseño complejo, cambios de arquitectura o toca varios módulos), pará, explicá el motivo en /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210/os.md, ejecutá `/usr/share/ravn/trio/trio-status --notify /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 needs-roberto plan --error "no es trivial; requiere trío completo"` y dejá el workspace abierto.
2. Worktree NUEVO para este ticket, siempre, con ravn-git-bare-clone (ADR 0019 de pj-template); nunca `git worktree add` a mano ni uno viejo aunque exista:
   `git -C /z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git fetch origin && git-create-worktree -r /z-home-repos/HR-CLAVIS/repos/rf-dotfiles -B 'origin/HEAD' -b issue/210-<slug> -p '' 210-<slug>`
   Queda en /z-home-repos/HR-CLAVIS/repos/rf-dotfiles/210-<slug>, en la rama issue/210-<slug> (el bare vive en /z-home-repos/HR-CLAVIS/.bares). El clon /z-home-repos/HR-CLAVIS/rf-dotfiles es para leer: nadie trabaja en él.
   Base: origin/HEAD; el PR va contra la rama por defecto.
   Actualizá: `/usr/share/ravn/trio/trio-status /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 en-curso wk`.
3. Implementá el cambio mínimo necesario con `implement` (y `tdd` si aplica).
4. **Prueba real (`Proof`)**:
   Corré las pruebas reales del repo (suite de pruebas y comandos del onboarding). Si no podés probar el cambio de verdad, no auto-mergeás.
5. **`code-review`**:
   Corré la skill `code-review` sobre el diff contra la rama base y asegurate de que quede limpio.
6. **PR con `/pr`**:
   Creá el PR con:
   `gh pr create -R robert-flo/rf-dotfiles --title "..." --body "..."`
   El body debe usar la plantilla de `/pr` (`## Summary`, `## Evidence` con antes/después y `## Merge Danger` con `**Door:**` y `**Blast Radius:**`), enlazando robert-flo/HR-CLAVIS#210 (en vez de Closes #210).
   Actualizá: `/usr/share/ravn/trio/trio-status /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 en-curso rv --pr <url> --head <sha>`.
7. **Babysit PR y Merge Gate**:
   Vigilá CI con `gh pr checks <pr> -R robert-flo/rf-dotfiles --watch` (o esperá a que terminen los checks).
   Evaluá las condiciones estrictas del Merge Gate:
   - Prueba real (`Proof`) ejecutada con éxito en esta sesión.
   - `code-review` limpio sin hallazgos pendientes.
   - CI en verde (todos los checks pasando, ninguno fallando o pendiente).
   - PR en estado `MERGEABLE` (sin conflictos).
   - `## Merge Danger` es exactamente `Door: two-way` y `Blast Radius: Low` (case-insensitive).
   
   - **Si califica para auto-merge**:
     - `/usr/share/ravn/trio/trio-status /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 en-curso merge --pr <url> --head <sha>`
     - `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha>`
     - Obtené `merge_sha` con `gh pr view <pr> -R robert-flo/rf-dotfiles --json mergeCommit -q .mergeCommit.oid`.
     - Limpiá etiquetas `ready-for-agent` y `needs-*` del issue `210` en `robert-flo/HR-CLAVIS` con `gh issue edit 210 -R robert-flo/HR-CLAVIS --remove-label ready-for-agent`.
     - Escribí el reporte final en /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210/os.md con el PR, el merge commit sha y la evidencia de pruebas.
     - `/usr/share/ravn/trio/trio-status --notify /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 hecho cierre --pr <url> --head <sha> --merge-sha <merge_sha> --merge-danger "two-way / Low"`
     - Commit + push del meta-repo /z-home-repos/HR-CLAVIS y borrá el worktree: `git -C /z-home-repos/HR-CLAVIS/repos/rf-dotfiles worktree remove <ruta>`.
     - Como último comando, resolvé el `workspace_id` de `trio HR-CLAVIS/rf-dotfiles#210` con `herdr workspace list` y ejecutá `herdr workspace close <workspace_id>`.

   - **Si NO califica para auto-merge** (Door: one-way, Blast Radius distinto de Low, fallo de CI, o no se pudo probar):
     - Etiquetá el PR: `gh pr edit <pr> -R robert-flo/rf-dotfiles --add-label ready-to-merge` (o `needs-info` si falta info o hubo bloqueo).
     - Actualizá: `/usr/share/ravn/trio/trio-status --notify /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 needs-roberto merge --pr <url> --head <sha> --merge-danger "<valor o motivo>"`.
     - Escribí /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210/os.md explicando por qué no se auto-mergeó.
     - Commit + push del meta-repo y dejá el workspace abierto para Roberto.

## Fallos y límites
- Nunca uses `push --force` ni `gh pr merge --admin`.
- Nunca auto-mergees sin prueba real ni con CI fallando.
- Ante un error no recuperable: `/usr/share/ravn/trio/trio-status --notify /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 needs-roberto <paso> --error "<motivo>"`, commiteá /z-home-repos/HR-CLAVIS/.trio/rf-dotfiles-210 y parate.
