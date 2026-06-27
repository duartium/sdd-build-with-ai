---
description: Crea la constitución SDD del proyecto en specs/ capturando el estado actual como línea base versionada (origin.mission.md + origin.tech-stack.md)
allowed-tools: Read, Glob, Grep, Bash(git rev-parse:*), Bash(git remote:*), Bash(ls:*), Bash(dir:*), AskUserQuestion
---

## Objetivo

Crear la constitución SDD del proyecto en `specs/` capturando el ESTADO ACTUAL como línea base versionada.
Solo se crean dos archivos: `specs/origin.mission.md` y `specs/origin.tech-stack.md`.

---

## Paso 1 — Analizar el codebase en silencio

Sin escribir nada al disco ni preguntar nada todavía, analiza el proyecto:

- Identifica archivos `.sln`, `.csproj`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, o equivalentes
- Lee los archivos de proyecto principales para detectar dependencias, frameworks y versiones
- Explora la estructura de directorios top-level
- Identifica patrones de arquitectura (capas, namespaces, módulos)
- Detecta esquemas de base de datos si hay migraciones, contextos EF, modelos ORM, o scripts SQL
- Lee el README si existe
- Revisa los commits recientes con `git log --oneline -20` para entender el historial de trabajo

**Detección de línea gráfica (analiza siempre, independiente del tipo de proyecto):**

Para proyectos WinForms / WPF / Avalonia:
- Lee archivos `.Designer.cs` o `.xaml` buscando `BackColor`, `ForeColor`, `Font`, `FlatStyle`, `BackgroundColor`, colores en formato `Color.FromArgb(...)` o `#RRGGBB`
- Busca archivos de recursos (`.resx`) que contengan imágenes, íconos o logos
- Identifica si hay clases de tema, constantes de color, o archivos tipo `AppColors.cs`, `Theme.cs`, `StyleHelper.cs`
- Detecta fuentes predominantes (nombre, tamaño, bold/italic)
- Identifica el estilo de botones (flat, standard, custom) y si hay bordes redondeados o sombras
- Detecta si hay un color de acento principal recurrente en controles activos, headers o botones primarios
- Busca uso de librerías de UI como `MaterialSkin`, `AeroSuite`, `Guna.UI`, `DevExpress`, `Telerik`, u otras

Para proyectos Web (HTML/CSS/JS/TS):
- Lee archivos CSS/SCSS/LESS buscando variables de color (`--color-primary`, `$primary`, etc.)
- Lee `tailwind.config.js` si existe (sección `theme.colors`)
- Detecta la paleta de colores definida, tipografías (`font-family`) y tamaños base
- Identifica si hay un design system o librería de componentes (Bootstrap, Shadcn, MUI, Ant Design, etc.)

Para cualquier proyecto:
- Busca archivos de logo (`logo.png`, `logo.svg`, `icon.ico`, `favicon.*`) e infiere colores del nombre o ubicación
- Detecta si hay un tema claro/oscuro configurado

Guarda mentalmente todo lo que encuentres. No escribas nada aún.

---

## Paso 2 — Entrevistar al usuario (3 preguntas agrupadas)

Usa la herramienta `AskUserQuestion` con 3 preguntas en una sola llamada, ente ellas:

**Pregunta 1 — Misión actual del sistema:**
Opciones a ofrecer basadas en lo que dedujiste del codebase. Construye 3 opciones concretas que describan lo que el sistema hace HOY según el código que leíste, más una opción abierta. Ejemplo de opciones si es un POS contable: "Sistema POS + contabilidad para PYMES", "Facturación electrónica y cierre de caja para retail", "Gestión de inventario y ventas con reportes".

**Pregunta 2 — Usuarios actuales:**
Opciones a ofrecer según lo que inferiste. Ejemplo: "Cajeros y administradores de tienda", "Contadores y dueños de negocio", "Equipo interno de la empresa". Más opción abierta.

IMPORTANTE: Agrupa las 3 preguntas en UNA SOLA llamada a `AskUserQuestion`. No hagas preguntas adicionales. No preguntes sobre roadmap futuro, evolución ni tecnologías nuevas.

---

## Paso 3 — Generar specs/origin.mission.md

Con las respuestas del usuario y tu análisis, escribe `specs/origin.mission.md` con esta estructura exacta:

```markdown
# Mission — Estado Actual del Sistema

> Documento de línea base. Captura lo que existe hoy, no lo que se planea construir.

## ¿Qué hace este sistema en producción?

[Descripción concreta basada en el código y las respuestas del usuario. 2-4 párrafos.]

## ¿Quién lo usa y para qué?

[Usuarios actuales identificados. Casos de uso concretos observados en el código.]

## ¿Qué problemas resuelve actualmente?

[Lista de problemas que el sistema resuelve HOY según la funcionalidad implementada.]

## ¿Qué está explícitamente fuera de su alcance actual?

[Lo que el código claramente NO hace. Límites observados en la implementación actual.]

---
*Generado: YYYY-MM-DD | Rama: [rama actual]*
```

---

## Paso 4 — Generar specs/origin.tech-stack.md

Escribe `specs/origin.tech-stack.md` con esta estructura exacta:

```markdown
# Tech Stack — Estado Actual del Sistema

> Documento de línea base. Refleja las tecnologías encontradas en el codebase, no las deseadas.

## Proyectos de la solución

| Proyecto | Tipo | Responsabilidad |
|----------|------|-----------------|
| [nombre] | [lib/exe/test] | [qué hace] |

## Arquitectura del sistema

[Descripción de la arquitectura identificada. Capas, flujo de datos, separación de responsabilidades.]

## Tecnologías, frameworks y versiones

| Categoría | Tecnología | Versión |
|-----------|-----------|---------|
| Runtime | [ej. .NET 4.8] | [versión exacta del proyecto] |
| ORM | [ej. Entity Framework 6] | [versión] |
| UI | [ej. WinForms] | [versión] |
| Base de datos | [ej. SQL Server] | [versión si se conoce] |
| ... | ... | ... |

## Patrones de diseño identificados

[Lista de patrones con ejemplos concretos: clases, archivos o namespaces donde se aplican.]

## Esquema y estructura de base de datos

[Tablas, entidades o migraciones encontradas. Si hay DbContext, lista los DbSet. Si hay migraciones, lista las más recientes.]

## Dependencias externas relevantes

[NuGet packages, npm packages, o librerías externas que sean parte del núcleo del sistema.]

## Línea gráfica

> Paleta, tipografía y estilo visual detectados en el codebase. Usar como referencia para mantener coherencia visual en nuevos formularios, componentes y assets.

### Paleta de colores

| Rol | Color (hex o nombre) | Dónde se usa |
|-----|---------------------|--------------|
| Primario | `#______` | [botones principales, headers...] |
| Secundario | `#______` | [botones secundarios, bordes...] |
| Fondo principal | `#______` | [BackColor de formularios...] |
| Fondo de panel | `#______` | [paneles, sidebars...] |
| Texto principal | `#______` | [ForeColor de labels, etiquetas...] |
| Texto secundario | `#______` | [hints, subtítulos...] |
| Acento / estado activo | `#______` | [selección, hover, foco...] |
| Error / alerta | `#______` | [validaciones, mensajes de error...] |
| Éxito / confirmación | `#______` | [confirmaciones, badges...] |

### Tipografía

| Rol | Fuente | Tamaño | Estilo |
|-----|--------|--------|--------|
| Títulos de formulario | [nombre] | [pt/px] | [Bold/Regular] |
| Labels de campo | [nombre] | [pt/px] | [Bold/Regular] |
| Botones | [nombre] | [pt/px] | [Bold/Regular] |
| Texto de tabla / grilla | [nombre] | [pt/px] | [Bold/Regular] |
| Texto de estado / footer | [nombre] | [pt/px] | [Bold/Regular] |

### Estilo de controles

- **Botones primarios:** [color fondo, color texto, bordes, estilo flat/standard/custom]
- **Botones secundarios:** [color fondo, color texto, bordes]
- **Campos de texto:** [borde, fondo, color de placeholder si aplica]
- **Grillas / tablas:** [color de header, color de fila alterna, color de selección]
- **Menú / navegación:** [color de fondo, color activo, color hover]
- **Formularios / ventanas:** [color de barra de título, borde de ventana]

### Logo e íconos

- **Logo:** [nombre de archivo, ubicación, colores principales del logo si se puede inferir]
- **Icono de aplicación:** [nombre de archivo, ubicación]
- **Librería de íconos:** [Font Awesome, Material Icons, íconos propios en .resx, etc.]

---
*Generado: YYYY-MM-DD | Rama: [rama actual]*
```

---

## Paso 5 — Registrar en .sln (solo si es proyecto .NET con solución)

Si encontraste un archivo `.sln` en el proyecto, registra los dos archivos creados como Solution Items:

1. Busca si ya existe una sección `Project("{2150E333-8FDC-42A3-9474-1A3956D46DE8}")` llamada "Solution Items" en el `.sln`
2. Si existe: agrega las entradas de los dos archivos dentro de su bloque `ProjectSection(SolutionItems)`
3. Si no existe: agrega un nuevo Project de tipo carpeta de solución llamado "specs" con los dos archivos como items
4. El formato de entrada es: `specs\origin.mission.md = specs\origin.mission.md`

No modifiques ninguna otra sección del `.sln`.

---

## Paso 6 — Reporte final

Muestra al usuario:

```
✓ specs/origin.mission.md creado
✓ specs/origin.tech-stack.md creado
[✓ Registrado en [nombre].sln como Solution Items]  ← solo si aplica

Próximos pasos:
  /commit-constitution-pr   → commitear en rama constitution y abrir PR
```

No hagas commit. No hagas push. No propongas cambios adicionales. Detente aquí.
