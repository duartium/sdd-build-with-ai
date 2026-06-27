---
description: Commitea la implementación de la feature en la rama actual y genera el link de PR hacia develop (por defecto) o la rama indicada con --pr <rama>
allowed-tools: Bash(git status:*), Bash(git branch:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git remote:*), Bash(git diff:*), Bash(git rev-parse:*), Bash(find:*)
argument-hint: [--pr <rama-destino>]
---

Ejecuta los siguientes pasos en orden estricto.
No hagas preguntas. No evalúes otros PRs o ramas. Ejecuta y reporta.

## Paso 1 — Parsear argumentos

Analiza $ARGUMENTS:
- Si $ARGUMENTS contiene `--pr <rama>`: guarda <rama> como RAMA_DESTINO
- Si $ARGUMENTS está vacío o no contiene `--pr`: RAMA_DESTINO = "develop"

## Paso 2 — Capturar la rama actual

- Ejecuta: `git rev-parse --abbrev-ref HEAD`
- Guarda como RAMA_ACTUAL
- Si RAMA_ACTUAL es main, master, develop o constitution: detente →
  "❌ Estás en la rama RAMA_ACTUAL — los commits de implementación
   deben hacerse en una rama de feature (feat/...)"

## Paso 3 — Validar que hay cambios para commitear

- Ejecuta: `git status --porcelain`
- Si no hay archivos modificados ni nuevos: detente →
  "⚠️ No hay cambios para commitear en RAMA_ACTUAL"

## Paso 4 — Validar remote origin

- Ejecuta: `git remote get-url origin`
- Si falla: detente → "❌ No hay remote origin configurado"

## Paso 5 — Extraer nombre del feature desde la rama actual

- Toma el nombre de RAMA_ACTUAL
- Elimina el prefijo feat/ si existe
- Guarda como FEATURE_NAME
- Ejemplo: `feat/phase-0-scaffolding` → `phase-0-scaffolding`

## Paso 6 — Staging de todos los cambios

- Ejecuta: `git add .`
- Excluye explícitamente: `git reset HEAD specs/` para no incluir specs en este commit
  (los specs ya fueron commiteados con commit-feature-spec)

## Paso 7 — Verificar cambios staged

- Ejecuta: `git diff --cached --name-only`
- Si está vacío: detente → "⚠️ No hay cambios staged para commitear"
- Muestra la lista de archivos que se van a commitear

## Paso 8 — Commit

- Ejecuta: `git commit -m "feat(FEATURE_NAME): implement feature"`

## Paso 9 — Push

- Ejecuta: `git push origin RAMA_ACTUAL`

## Paso 10 — Generar link del PR

- Obtén URL del remote: `git remote get-url origin`
- Transforma al formato web:
  - SSH `git@github.com:owner/repo.git` → `https://github.com/owner/repo`
  - HTTPS `https://github.com/owner/repo.git` → `https://github.com/owner/repo`
- Construye: `{url_base}/compare/RAMA_DESTINO...RAMA_ACTUAL`

## Paso 11 — Mensaje final

```
✓ Implementación commiteada: FEATURE_NAME
✓ Rama: RAMA_ACTUAL
✓ Push realizado a origin

Abre este link para crear tu PR hacia 'RAMA_DESTINO':
{link_pr}
```

## Regla de comportamiento

No cambia de rama. No commitea archivos de specs/ — esos tienen su propio comando.
Si no se pasa --pr, el destino del PR es siempre develop, nunca main ni master.
