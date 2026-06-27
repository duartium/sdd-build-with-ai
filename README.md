# The Engineering Leaders Playbook for Spec-Driven Development

Recursos de la charla **"The Engineering Leaders Playbook for Spec-Driven Development"**.

Este repositorio contiene el setup completo para aplicar **SDD (Spec-Driven Development)** sobre proyectos **heredados** — sistemas en producción que no fueron diseñados con specs desde el inicio, y que ahora necesitan incorporar documentación viva, control de cambios y un flujo de ingeniería sostenible.

---

## ¿Qué problema resuelve?

Los proyectos heredados acumulan deuda de conocimiento: nadie sabe exactamente qué hace cada parte del sistema, los cambios se hacen a ciegas y cada nueva feature introduce riesgo. SDD convierte ese caos en un flujo documentado, paso a paso, manteniendo el control en manos del desarrollador.

---

## Estructura del repositorio

```
sdd-claude-setup/
  agents/          → Sub-agentes especializados de Claude Code
  commands/        → Slash commands del flujo SDD

demo-implementation/
  CLAUDE.md        → Ejemplo de configuración de proyecto
  specs/           → Ejemplo de constitución y feature specs generados
```

---

## Setup

### 1. Requisitos previos

- **Claude Code** instalado y configurado
- **GitHub CLI** (`gh`) instalado y autenticado
- Repositorio Git con remote `origin` configurado

### 2. Copiar agentes y commands

Los agentes y commands deben copiarse al directorio de configuración de Claude Code en tu máquina:

```
~/.claude/
  agents/          ← copia aquí el contenido de sdd-claude-setup/agents/
  commands/        ← copia aquí el contenido de sdd-claude-setup/commands/
```

**En Windows:**
```powershell
Copy-Item -Recurse sdd-claude-setup\agents\* "$env:USERPROFILE\.claude\agents\"
Copy-Item -Recurse sdd-claude-setup\commands\* "$env:USERPROFILE\.claude\commands\"
```

**En macOS / Linux:**
```bash
cp -r sdd-claude-setup/agents/* ~/.claude/agents/
cp -r sdd-claude-setup/commands/* ~/.claude/commands/
```

### 3. Configurar el MCP de Repomix

El comando `/codebase-map` depende del servidor MCP de **Repomix** para empaquetar y leer el codebase completo en una sola pasada.

Repositorio oficial: [github.com/yamadashy/repomix](https://github.com/yamadashy/repomix)

Agrega el servidor MCP a tu configuración de Claude Code:

```json
{
  "mcpServers": {
    "repomix": {
      "command": "npx",
      "args": ["-y", "repomix", "--mcp"]
    }
  }
}
```

> El comando `/codebase-map` usa las herramientas `mcp__repomix__pack_codebase` y `mcp__repomix__read_repomix_output` para analizar el proyecto sin escribir archivos al disco.

### 4. Agregar CLAUDE.md a tu proyecto

Copia el archivo `demo-implementation/CLAUDE.md` a la raíz de tu proyecto heredado y ajústalo. Este archivo le da a Claude las reglas de comportamiento específicas del proyecto: convención de commits, cuándo pedir confirmación, idioma, y que debe leer `specs/` antes de actuar.

---

## Flujo de trabajo paso a paso

### FASE 0 — Exploración guiada del codebase

```
/codebase-map
```

Usa Repomix para empaquetar el proyecto completo y producir un **mapa estructurado** del sistema: arquitectura, proyectos de la solución, entidades de dominio, stored procedures, patrones de diseño y tablas de base de datos.

No escribe archivos. Solo analiza y reporta. El objetivo es entender qué tienes antes de tomar cualquier decisión.

---

### FASE 1 — Constitución de origen

```
/sdd-origin
```

Captura el **estado actual del sistema como línea base versionada**. Analiza el codebase en silencio, te hace 2-3 preguntas clave sobre la misión actual y los usuarios, y genera:

- `specs/origin.mission.md` — qué hace el sistema hoy, quién lo usa, qué problemas resuelve
- `specs/origin.tech-stack.md` — stack tecnológico, arquitectura, patrones, paleta de colores y tipografía detectados

> Esta es la fotografía del "antes". Documenta la realidad, no las aspiraciones.

---

### FASE 2 — Constitución de destino

```
/sdd-constitution
```

A través de una entrevista guiada, define **hacia dónde va el proyecto**: misión, usuarios objetivo, alcance de la v1, fuera de alcance y roadmap de fases priorizadas. Genera:

- `specs/mission.md`
- `specs/tech-stack.md`
- `specs/roadmap.md`

Cada archivo se presenta para tu aprobación antes de escribirse. Una constitución con vacíos es peor que ninguna constitución.

#### Confirmar la constitución

```
/commit-constitution-pr
```

Commitea los archivos de `specs/` en la rama `constitution`, hace push y crea (o mergea) automáticamente el PR hacia tu rama base. El historial queda versionado desde el primer momento.

---

### FASE 3 — Flujo de implementación de features

```
/sdd-next-feat-spec
```

Lee la constitución, detecta la **siguiente fase pendiente del roadmap** y crea la rama de feature. A través de 3 preguntas sobre scope, decisiones técnicas y criterio de validación, genera:

- `specs/feat/YYYY-MM-DD-NOMBRE/plan.md` — grupos de tareas ordenados por dependencia
- `specs/feat/YYYY-MM-DD-NOMBRE/requirements.md` — alcance, fuera de alcance, decisiones técnicas, dependencias
- `specs/feat/YYYY-MM-DD-NOMBRE/validation.md` — criterios funcionales, técnicos, de integración y definición de Done

Luego, para versionar el spec antes de implementar:

```
/commit-feature-spec
```

Una vez implementada y validada la feature:

```
/commit-feature-impl --pr <rama-destino>
```

Commitea la implementación y genera el link del PR listo para abrir.

---

### FASE 4 — Replanning

Desde que incorporaste SDD a tu proyecto heredado, es probable que encuentres muchas cosas que ajustar: el roadmap cambia, aparecen dependencias no previstas, el alcance de una fase resulta ser mayor de lo esperado.

Eso es normal. Actualiza `specs/roadmap.md` y vuelve a ejecutar `/commit-constitution-pr` para versionar los cambios. Las specs son documentos vivos, no contratos rígidos.

---

## Resultado

Ahora tienes un proyecto heredado incorporado a SDD: un flujo documentado, trabajado en pasos usando ingeniería y con el control en tus manos.

**Las specs son la memoria del proyecto.** Cuando un nuevo desarrollador se incorpore, cuando retomes el proyecto después de meses, o cuando un agente de IA necesite contexto, `specs/` es la fuente de verdad.

---

## Recomendaciones

- **Empieza por el mapa.** `/codebase-map` antes de cualquier otra cosa. No puedes documentar lo que no entiendes.
- **La constitución de origen es obligatoria.** Sin `origin.*`, no tienes línea base y no puedes medir el progreso.
- **Una fase a la vez.** No implementes dos fases en paralelo. El valor de SDD está en el control incremental.
- **Actualiza las specs cuando cambia la realidad.** Una spec desactualizada es deuda de conocimiento disfrazada de documentación.
- **El `CLAUDE.md` es el contrato con el agente.** Dedica tiempo a ajustarlo: define el idioma, las reglas de git y cuándo debe pedir confirmación.
- **Construye tu propio flujo.** Los commands son puntos de partida. Puedes modificarlos, extenderlos y agregar los tuyos según las necesidades de tu equipo.
