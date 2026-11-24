# OS — robert-flo/HR-CLAVIS#186

- **PR:** https://github.com/robert-flo/pj-template/pull/100
- **Head:** `d3de3dea0d8b16558b1a4a0059da7dba30ef9390`
- **Rama:** `issue/186-sumar-hr-kodigo` → `main`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/pj-template/186-hr-kodigo-family`
- **Merge:** no. Queda para Roberto.

## Qué quedó

`family.yml` suma `robert-flo/HR-KODIGO` al final de `hijos:`. No corrí `make nuevo-hijo`, no creé repos y no toqué secretos. No cierra `robert-flo/pj-template#35`.

## Pruebas

Antes, `family.yml` no tenía `HR-KODIGO` (`yaml.safe_load`: 7 hijos, `ausente`). Después, el chequeo de `scripts/check.sh` (`yaml.safe_load` de workflows, `family.yml` y `proyecto.yml`) imprimió `YAML válido`; el ítem está una vez, al final, y la cuenta es 8. CI `check` del PR: SUCCESS. `mergeable`: MERGEABLE. `mergeStateStatus`: CLEAN.

## code-review

Spec: el diff cumple la línea pedida; no hay alcance de más. Standards: el runbook de secretos dice que no se publica un home-repo privado en este repo público. El issue #186 es la excepción explícita de Roberto, así que no revertí la línea. Sin olores de código.

## Por qué no hay auto-merge

`## Merge Danger` es `Door: one-way` y `Blast Radius: Low`. El nombre de un repo privado entra en un repo público. El Merge Gate no auto-mergea. Etiqueta `ready-to-merge` en el PR. `needs-roberto` vive en `status.json` (no está en `schema/etiquetas.tsv`).
