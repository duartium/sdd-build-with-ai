---
description: Commitea specs/ en la rama constitution y genera el link de PR hacia la rama destino (argumento) o la rama actual (sin argumento)
allowed-tools: Bash(git status:*), Bash(git branch:*), Bash(git checkout:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git remote:*), Bash(git diff:*), Bash(git rev-parse:*), Bash(gh pr:*)
argument-hint: [rama-destino]
---

Ejecuta los siguientes pasos en orden estricto.
No hagas preguntas. No evalúes otros PRs o ramas. Ejecuta y reporta.

## Paso 1 — Capturar la rama actual

- Ejecuta: `git rev-parse --abbrev-ref HEAD`
- Guarda este valor como RAMA_ORIGEN
- Si $ARGUMENTS está vacío, la rama destino del PR será RAMA_ORIGEN
- Si $ARGUMENTS tiene valor, la rama destino del PR será $ARGUMENTS

## Paso 2 — Validar specs/

- Verifica que existe el directorio `specs/` con al menos un archivo `.md`
  - Si no existe: detente y muestra "❌ No existe specs/ — crea la constitución primero"
  - Si está vacío: detente y muestra "❌ specs/ está vacío — no hay nada que commitear"

## Paso 3 — Validar remote origin

- Ejecuta: `git remote get-url origin`
  - Si falla: detente y muestra "❌ No hay remote origin configurado"

## Paso 4 — Cambiar a la rama constitution

- Ejecuta: `git branch --list constitution`
- Si la rama ya existe: `git checkout constitution`
- Si no existe: `git checkout -b constitution`

## Paso 5 — Agregar solo los cambios de specs/

- Ejecuta: `git add specs/`

## Paso 6 — Verificar que hay cambios staged

- Ejecuta: `git diff --cached --name-only`
- Si la lista está vacía: muestra "⚠️ No hay cambios nuevos en specs/ — nada que commitear" y detente
- No hagas preguntas. No menciones otros PRs ni otras ramas.

## Paso 7 — Commit

- Ejecuta: `git commit -m "docs(constitution): update project constitution baseline"`

## Paso 8 — Push

- Ejecuta: `git push origin constitution`

## Paso 9 — Crear y mergear el PR automáticamente

- RAMA_DESTINO = $ARGUMENTS si tiene valor, sino RAMA_ORIGEN capturada en Paso 1
- Verifica si ya existe un PR abierto desde `constitution` hacia `RAMA_DESTINO`:
  - Ejecuta: `gh pr list --head constitution --base RAMA_DESTINO --state open --json number --jq '.[0].number'`
  - Si devuelve un número: guárdalo como PR_NUMBER y salta la creación
  - Si está vacío: crea el PR con:
    ```
    gh pr create --title "docs(constitution): project constitution baseline" --body "Constitución SDD generada automáticamente con /sdd-origin" --head constitution --base RAMA_DESTINO
    ```
    Luego obtén el número: `gh pr list --head constitution --base RAMA_DESTINO --state open --json number --jq '.[0].number'`
- Mergea sin esperar aprobación:
  - Ejecuta: `gh pr merge PR_NUMBER --merge --delete-branch=false`
  - Si falla porque el repo requiere aprobación o protección de rama, muestra el error y continúa al Paso 10 con el PR_NUMBER

## Paso 10 — Mensaje final

Muestra exactamente esto:

```
✓ Constitución commiteada en rama: constitution
✓ Push realizado a origin
✓ PR #PR_NUMBER mergeado hacia 'RAMA_DESTINO'
```

Si el merge falló por protección de rama, muestra en su lugar:

```
✓ Constitución commiteada en rama: constitution
✓ Push realizado a origin
⚠️  Merge automático bloqueado (protección de rama) — PR #PR_NUMBER abierto hacia 'RAMA_DESTINO'
```

## Regla de comportamiento

Este comando no hace preguntas ni evalúa el trabajo de otras ramas o PRs.
Ejecuta los pasos en orden estricto y termina con el mensaje final o con el error correspondiente.
No vuelvas a RAMA_ORIGEN automáticamente — deja al usuario en la rama constitution al terminar.