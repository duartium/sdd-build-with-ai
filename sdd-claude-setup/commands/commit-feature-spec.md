---
description: Commitea el feature spec (plan, requirements, validation) en la rama actual. Pasa --pr <rama> solo si quieres generar el link de PR.
allowed-tools: Bash(git status:*), Bash(git branch:*), Bash(git add:*), Bash(git commit:*), Bash(git push:*), Bash(git remote:*), Bash(git diff:*), Bash(git rev-parse:*), Bash(find:*)
argument-hint: [--pr <rama-destino>]
---

Ejecuta los siguientes pasos en orden estricto.
No hagas preguntas. No evalúes otros PRs o ramas. Ejecuta y reporta.

## Paso 1 — Parsear argumentos

Analiza $ARGUMENTS:
- Si $ARGUMENTS contiene `--pr <rama>`: activa GENERAR_PR=true y guarda <rama> como RAMA_DESTINO
- Si $ARGUMENTS está vacío o no contiene `--pr`: GENERAR_PR=false

## Paso 2 — Capturar la rama actual

- Ejecuta: `git rev-parse --abbrev-ref HEAD`
- Guarda como RAMA_ACTUAL
- NUNCA uses main, master o constitution como rama destino por defecto

## Paso 3 — Validar feature spec en specs/

- Busca directorios: `find specs/ -maxdepth 1 -type d -name "????-??-??-*"`
- Si no existe ninguno: detente → "❌ No se encontró ningún feature spec en specs/"
- Si existe más de uno: usa el más reciente por fecha
- Guarda como FEATURE_SPEC_DIR

## Paso 4 — Validar los tres archivos del spec

Verifica dentro de FEATURE_SPEC_DIR:
- plan.md
- requirements.md  
- validation.md

Si falta alguno: detente → "❌ Spec incompleto en FEATURE_SPEC_DIR — faltan: [lista]"

## Paso 5 — Validar remote origin

- Ejecuta: `git remote get-url origin`
- Si falla: detente → "❌ No hay remote origin configurado"

## Paso 6 — Staging

- Ejecuta: `git add FEATURE_SPEC_DIR/`

## Paso 7 — Verificar cambios staged

- Ejecuta: `git diff --cached --name-only`
- Si está vacío: detente → "⚠️ No hay cambios nuevos en el feature spec"

## Paso 8 — Extraer nombre del feature

- Toma el nombre de FEATURE_SPEC_DIR, elimina el prefijo YYYY-MM-DD-
- Guarda como FEATURE_NAME
- Ejemplo: `2026-06-23-phase-0-scaffolding` → `phase-0-scaffolding`

## Paso 9 — Commit

- Ejecuta: `git commit -m "feat(spec): add feature spec for FEATURE_NAME"`

## Paso 10 — Push

- Ejecuta: `git push origin RAMA_ACTUAL`

## Paso 11 — Mensaje final

### Si GENERAR_PR=false (caso normal):

```
✓ Feature spec commiteado: FEATURE_SPEC_DIR
✓ Rama: RAMA_ACTUAL
✓ Push realizado a origin

Spec confirmado. Continúa con la implementación.
Cuando termines la validación usa: /commit-feature-spec --pr <rama-destino>
```

### Si GENERAR_PR=true:

- Obtén URL del remote y transfórmala al formato web GitHub
- Construye: `{url_base}/compare/RAMA_DESTINO...RAMA_ACTUAL`

```
✓ Feature spec commiteado: FEATURE_SPEC_DIR
✓ Rama: RAMA_ACTUAL
✓ Push realizado a origin

Abre este link para crear tu PR hacia 'RAMA_DESTINO':
{link_pr}
```

## Regla de comportamiento

No cambia de rama. No sugiere PRs a main o master por defecto.
El PR solo se genera si el usuario pasa explícitamente --pr <rama>.