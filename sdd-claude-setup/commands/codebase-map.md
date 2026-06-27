---
description: Analiza el codebase completo con repomix y genera un mapa estructurado de arquitectura, entidades, SPs, patrones y tablas DB
allowed-tools: mcp__repomix__pack_codebase, mcp__repomix__pack_remote_repository, mcp__repomix__read_repomix_output, mcp__repomix__file_system_read_file, mcp__repomix__file_system_read_directory
---

## Objetivo

Leer el codebase completo usando la herramienta repomix MCP y producir un mapa estructurado del sistema. No escribas archivos. Solo analiza y reporta.

---

## Paso 1 — Leer el codebase con repomix

Usa la herramienta repomix MCP para empaquetar y leer el codebase completo del directorio actual.

---

## Paso 2 — Analizar en silencio

Sin escribir nada todavía, extrae la siguiente información del código:

**Arquitectura:**
- Proyectos de la solución (archivos `.sln`, `.csproj`, `package.json`, etc.)
- Responsabilidad de cada proyecto (UI, dominio, datos, infraestructura, tests)
- Cómo se referencian entre sí (dependencias entre proyectos)

**Entidades:**
- Clases que representan entidades de dominio o modelos de base de datos
- Campos clave de cada una (PK, FKs, campos de negocio relevantes)
- Omite entidades vacías, stubs o con menos de 3 propiedades

**Stored Procedures:**
- Nombre de cada SP, su propósito funcional y parámetros más relevantes
- Busca en archivos `.sql`, carpetas `database/`, llamadas a `ExecuteSqlCommand`, `FromSqlRaw`, o `sp_executesql`

**Patrones de diseño:**
- Repository, Unit of Work, Mapper, Factory, Service Layer, CQRS, etc.
- Para cada patrón: nombre del patrón + ejemplo concreto (clase o archivo donde se aplica)

**Base de datos:**
- Esquemas utilizados (`dbo`, `sales`, `inventory`, etc.)
- Tabla principal por área funcional (ventas, inventario, contabilidad, usuarios, etc.)
- Detecta desde DbSet, scripts SQL, o migraciones

---

## Paso 3 — Generar el reporte

Responde con este reporte estructurado. Usa tablas donde mejoren la lectura. Sé conciso: omite detalles obvios o repetitivos.

```
# Codebase Map — [nombre del proyecto]

## 1. Arquitectura General

| Proyecto | Tipo | Responsabilidad |
|----------|------|-----------------|
| [nombre] | [UI/Domain/Data/Infra/Test] | [qué hace] |

**Relaciones:**
[Describir dependencias entre proyectos en 3-5 líneas]

---

## 2. Entidades Principales

| Entidad | Propósito | Campos Clave |
|---------|-----------|--------------|
| [Nombre] | [qué representa] | [campo1, campo2, FK: campo3] |

---

## 3. Stored Procedures

| SP | Propósito | Parámetros Relevantes |
|----|-----------|----------------------|
| [sp_nombre] | [qué hace] | [@param1 tipo, @param2 tipo] |

*(Omitido si no se encuentran SPs en el codebase)*

---

## 4. Patrones de Diseño

| Patrón | Ejemplo Concreto |
|--------|-----------------|
| [Repository] | [IProductRepository → ProductRepository en Data/] |
| [Unit of Work] | [IUnitOfWork en ApplicationCore/, impl en Data/] |

---

## 5. Tablas de Base de Datos

| Esquema | Área Funcional | Tabla Principal |
|---------|---------------|-----------------|
| dbo | [Ventas] | [Facturas] |
| dbo | [Inventario] | [Productos] |

*(Incluye solo las tablas identificadas desde el código — no inventes esquemas)*
```

---

## Reglas de salida

- No escribas ningún archivo al disco
- No hagas preguntas al usuario
- No propongas mejoras ni siguientes pasos
- Si no encuentras información para una sección, escribe `*(No identificado en el codebase)*`
- Detente después del reporte
