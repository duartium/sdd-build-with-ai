---
description: Crea la constitución SDD del nuevo proyecto (mission.md, tech-stack.md, roadmap.md) a partir de una entrevista al usuario — sin leer archivos previos
allowed-tools: Bash(mkdir:*), Bash(git:*), Bash(find:*), Write
argument-hint: (sin argumentos)
---

Eres un arquitecto de software senior aplicando Spec-Driven Development.
Tu objetivo es crear una constitución de alta calidad que sirva como
contrato entre el equipo, el negocio y los agentes futuros.

Una constitución con vacíos genera desvíos del objetivo.
Una constitución precisa previene decisiones contradictorias
antes de que ocurran.

No leas ningún archivo del proyecto. Todo el contexto
viene de la entrevista al usuario.

---

## FASE 1 — Entrevista

Usa AskUserQuestion con máximo 3 opciones por pregunta.
Una pregunta a la vez. En este orden estricto:

**Pregunta 1 — Propósito del nuevo sistema:**
¿Cuál es el objetivo principal de lo que vas a construir?

**Pregunta 2 — Usuarios:**
¿Quiénes van a usar este sistema y para qué tarea específica?

**Pregunta 3 — Problema que resuelve:**
¿Qué problema de negocio concreto resuelve esta solución?

**Pregunta 4 — Alcance v1:**
¿Qué capacidades debe tener la primera versión funcional?

**Pregunta 5 — Fuera de alcance:**
¿Qué está explícitamente fuera de esta primera versión?

**Pregunta 6 — Stack del nuevo sistema:**
¿Qué tecnologías has decidido usar para construir esto?

**Pregunta 7 — Prioridad del roadmap:**
¿Qué debe estar funcionando primero para que el negocio
sienta valor inmediato?

**Pregunta 8 — Criterio de ordenamiento:**
¿El roadmap se ordena por valor de negocio (lo más útil primero)
o por dependencia técnica (lo más simple primero)?

No escribas ningún archivo hasta completar todas las preguntas.

---

## FASE 2 — Generar y presentar para aprobación

Con las respuestas del usuario, genera los tres archivos
y preséntalos como texto en el chat — uno por uno —
esperando aprobación de cada uno antes de continuar
con el siguiente.

### specs/mission.md

Estructura exacta:

```
# Mission

> Línea base versionada · Estado al [fecha actual]

## ¿Qué es este sistema?
[Descripción concisa del nuevo sistema — qué hace y para qué existe]

## Usuarios
[Tabla: Usuario | Tarea específica | Necesidad principal]

## Problema que resuelve
[Lista concreta de los problemas de negocio que esta solución ataca]

## Alcance v1
[Lo que SÍ se construye en esta primera versión — específico y medible]

## Fuera de alcance — v1
[Lo que explícitamente NO se construye — previene scope creep]
```

Reglas:
- Sin aspiraciones vagas — cada punto debe ser concreto y verificable
- El fuera de alcance es tan importante como el alcance
- Si algo no quedó claro en la entrevista, pregunta antes de asumir

Muestra `mission.md` al usuario y espera "aprobado" antes de continuar.

---

### specs/tech-stack.md

Estructura exacta:

```
# Tech Stack

> Decisiones tomadas al [fecha actual]
> Toda decisión fuera de este documento requiere
> actualizar este archivo primero — antes de implementar

## Principio guía
[Una frase que define el criterio de todas las decisiones de stack]

## Stack

### [Nombre del componente backend]

| Área | Decisión | Justificación |
[Cada fila justifica por qué — no solo qué tecnología]

### [Nombre del componente frontend — si aplica]

| Área | Decisión | Justificación |

## Fuera del stack — justificación explícita
[Tecnologías deliberadamente excluidas y por qué]
[Esto previene que el agente las proponga en fases futuras]

## Arquitectura
[Diagrama ASCII mostrando cómo se comunican los componentes]

## Hosting
[Tabla: Entorno | Componente | Tecnología]
```

Reglas:
- Cada decisión tecnológica DEBE tener justificación
- El "fuera del stack" es obligatorio — sin él el agente inventará alternativas
- El diagrama ASCII debe mostrar puertos y dirección del flujo
- Si el stack no fue completamente definido en la entrevista, pregunta

Muestra `tech-stack.md` al usuario y espera "aprobado" antes de continuar.

---

### specs/roadmap.md

Estructura exacta:

```
# Roadmap

> Línea base versionada · Estado al [fecha actual]
> Ordenado por [criterio confirmado con el usuario]

[ ] Fase 0 — Scaffolding: estructura base compilando
    y conectada a la infraestructura existente
[ ] Fase N — Nombre: objetivo en una sola oración
...

## Criterios de completitud por fase
Una fase se considera Done cuando:
1. La funcionalidad corre sin errores
2. Las reglas de negocio del alcance están cubiertas
3. El código fue revisado y aprobado por el desarrollador
```

Reglas estrictas:
- Fase 0 es SIEMPRE Scaffolding — sin excepción
- Una línea por fase — sin detalle de tareas
- Cada fase implementable de forma independiente
- Ninguna fase depende de algo no documentado en mission.md
  o tech-stack.md
- El fuera de alcance de mission.md no aparece en ninguna fase
- Máximo 8 fases — si hay más están demasiado granuladas

Muestra `roadmap.md` al usuario y espera "aprobado" antes de escribir.

---

## FASE 3 — Escritura y post-escritura

Solo cuando el usuario haya aprobado los tres archivos:

1. Crea el directorio `specs/` si no existe
2. Escribe los tres archivos exactamente como fueron aprobados
   — sin modificaciones
3. Si existe un archivo `.sln` en la raíz del proyecto,
   registra los tres archivos como Solution Items en el `.sln`
   para que sean visibles en Visual Studio

Muestra el resumen final:

```
✓ specs/mission.md
✓ specs/tech-stack.md
✓ specs/roadmap.md

Constitución creada. Próximos pasos:
1. Revisa los archivos y ajusta lo que no corresponda
2. Ejecuta /commit-constitution <rama-destino> para versionar
3. Una vez mergeada usa /create-feature-spec para la Fase 0
```

---

## Regla de comportamiento

No escribas ningún archivo hasta que el usuario apruebe cada uno.
No combines fases — entrevista, presentación y escritura
son secuenciales y no se solapan.
Si hay ambigüedad en cualquier punto, pregunta.
Una constitución con vacíos es peor que ninguna constitución.