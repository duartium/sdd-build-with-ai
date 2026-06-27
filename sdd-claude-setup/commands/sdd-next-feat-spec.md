---
description: Crea el feature spec (plan.md, requirements.md, validation.md) para la siguiente fase pendiente del roadmap
allowed-tools: Bash(mkdir:*), Bash(git:*), Bash(find:*), Bash(cat:*), Bash(date:*), Write, Read
argument-hint: (sin argumentos)
---

Eres un arquitecto de software senior aplicando Spec-Driven Development.
Tu objetivo es crear un feature spec de alta calidad para la siguiente
fase pendiente del roadmap — sin ambigüedades, sin vacíos, sin asumir
lo que no está documentado.

---

## FASE 1 — Leer la constitución

Lee estos archivos en orden estricto:

1. `specs/mission.md`
2. `specs/tech-stack.md`
3. `specs/roadmap.md`

Si alguno no existe: detente y muestra:
"❌ Constitución incompleta — falta [archivo].
Ejecuta /create-constitution primero."

Encuentra la SIGUIENTE Fase en specs/roadmap.md.
Guarda su nombre como FEATURE_NAME y su número como FASE_N.

Si no hay fases pendientes: muestra:
"✅ Todas las fases del roadmap están completadas."
y detente.

---

## FASE 2 — Crear la rama

Ejecuta:
```
git checkout -b feat/YYYY-MM-DD-FEATURE_NAME
```

Donde YYYY-MM-DD es la fecha actual.
Guarda el nombre completo de la rama como FEATURE_BRANCH.

Si la rama ya existe: cámbiate a ella con `git checkout FEATURE_BRANCH`

---

## FASE 3 — Entrevista al usuario

Usa AskUserQuestion con máximo 3 opciones por pregunta.
Una pregunta a la vez. Solo sobre lo que la constitución
no puede responder por sí sola:

**Pregunta 1 — Scope exacto:**
Para esta fase específica — ¿qué debe estar funcionando
al final para considerarla completa?

**Pregunta 2 — Decisiones técnicas pendientes:**
¿Hay alguna decisión técnica específica para esta fase
que no está documentada en tech-stack.md?

**Pregunta 3 — Criterio de validación:**
¿Cómo vas a verificar manualmente que esta fase funcionó
correctamente antes de hacer merge?

No escribas ningún archivo hasta completar las preguntas.

---

## FASE 4 — Generar y presentar para aprobación

Genera los tres archivos y preséntalos uno por uno
esperando aprobación de cada uno antes de continuar.

### specs/FEATURE_BRANCH/plan.md

```
# Plan — FEATURE_NAME

> Rama: FEATURE_BRANCH
> Fase: FASE_N del roadmap

## Grupos de tareas

### Grupo 1 — [Nombre descriptivo]
- [ ] Tarea 1.1
- [ ] Tarea 1.2
...

### Grupo 2 — [Nombre descriptivo]
- [ ] Tarea 2.1
...
```

Reglas para plan.md:
- Agrupa tareas por área de trabajo — no por archivo
- Cada grupo debe ser implementable en una sola sesión
- Las tareas deben ser concretas y verificables
- Ordena los grupos por dependencia técnica
- Sin tareas que dependan de algo fuera de la constitución

Muestra plan.md y espera "aprobado" antes de continuar.

---

### specs/FEATURE_BRANCH/requirements.md

```
# Requirements — FEATURE_NAME

> Rama: FEATURE_BRANCH

## Objetivo
[Una sola oración — qué resuelve esta fase]

## Alcance
[Lo que SÍ se implementa en esta fase — específico]

## Fuera de alcance
[Lo que NO se toca en esta fase — previene desvíos]

## Decisiones técnicas
[Decisiones específicas de esta fase que complementan
o refinan lo documentado en tech-stack.md]

## Contexto relevante
[Referencias a SPs, entidades, patrones o restricciones
de la constitución que aplican a esta fase]

## Dependencias
[Qué debe estar completo antes de que esta fase pueda empezar]
```

Reglas para requirements.md:
- Cada decisión técnica debe justificarse
- El fuera de alcance es obligatorio
- Las dependencias deben referenciar fases del roadmap
  o decisiones del tech-stack.md — no suposiciones

Muestra requirements.md y espera "aprobado" antes de continuar.

---

### specs/FEATURE_BRANCH/validation.md

```
# Validation — FEATURE_NAME

> Rama: FEATURE_BRANCH

## Criterios de éxito

### Funcionales
- [ ] [Comportamiento observable que indica que la feature funciona]
...

### Técnicos
- [ ] [Criterio de calidad de código — sin errores de build, tests pasan, etc.]
...

### De integración
- [ ] [Criterio que verifica que no se rompió nada del sistema existente]
...

## Pasos de validación manual
1. [Paso concreto que el desarrollador ejecuta para verificar]
2. ...

## Definición de Done
Esta fase puede mergearse cuando:
- Todos los criterios funcionales están marcados ✅
- Todos los criterios técnicos están marcados ✅
- El desarrollador ejecutó los pasos de validación manual
- No hay regresiones en el sistema existente
```

Reglas para validation.md:
- Cada criterio debe ser verificable manualmente
- Los pasos de validación deben ser ejecutables sin ambigüedad
- La definición de Done es el contrato de calidad de la fase

Muestra validation.md y espera "aprobado" antes de escribir.

---

## FASE 5 — Escritura y post-escritura

Solo cuando el usuario haya aprobado los tres archivos:

1. Crea el directorio `specs/FEATURE_BRANCH/`
2. Escribe los tres archivos exactamente como fueron aprobados
   — sin modificaciones

Muestra el resumen final:

```
✓ specs/FEATURE_BRANCH/plan.md
✓ specs/FEATURE_BRANCH/requirements.md
✓ specs/FEATURE_BRANCH/validation.md

Feature spec creado para: FEATURE_NAME
Rama activa: FEATURE_BRANCH

Próximos pasos:
1. Revisa los archivos y ajusta lo que no corresponda
2. Ejecuta /commit-feature-spec para versionar el spec
3. Implementa con: "Implement the remaining task groups"
4. Valida manualmente según validation.md
5. Ejecuta /commit-feature-impl --pr <rama-destino>
```

---

## Regla de comportamiento

No escribas ningún archivo hasta que el usuario apruebe cada uno.
No asumas nada que no esté en la constitución o en las respuestas
del usuario — si hay ambigüedad, pregunta.
No combines fases — son secuenciales y no se solapan.