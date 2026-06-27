# Requirements — Scaffolding

> Rama: feat/2026-06-27-scaffolding

## Objetivo
Crear la estructura base de los dos proyectos nuevos (API y SPA) de forma que ambos
arranquen sin errores y la API exponga un endpoint de health check.

## Alcance
- Crear proyecto ASP.NET Core 10 Minimal API: HendrixAccountant.Web.Api
- Registrar HendrixAccountant.Web.Api en HendrixAccountant.sln
- Referenciar HendrixAccountant.ApplicationCore desde la API
- Configurar cadena de conexión a SQL Server existente en appsettings.json
- Implementar GET /health → 200 OK
- Crear proyecto React 18 + Vite: hendrix-web
- Verificar que ambos proyectos arrancan correctamente en local

## Fuera de alcance
- Ningún endpoint de negocio (ventas, caja, inventario)
- Autenticación JWT
- Queries a la base de datos (la conexión se configura pero no se usa en Fase 0)
- Estilos, librerías de estado, HTTP client del frontend (se decide antes de Fase 2)
- Configuración de producción u hosting
- Tests automatizados

## Decisiones técnicas
- Framework API: ASP.NET Core 10 Minimal APIs — mismo ecosistema .NET del proyecto desktop
- Framework SPA: React 18 + Vite con template react (JavaScript; TypeScript se decide antes de Fase 1)
- Cadena de conexión: appsettings.json local; los valores reales no se comitean
- Puerto API: 5000 (desarrollo local)
- Puerto SPA: 5173 (Vite dev server por defecto)

## Contexto relevante
- HendrixAccountant.ApplicationCore contiene la lógica de negocio validada que la API reutilizará
- El stack base (ASP.NET Core 10, React + Vite, SQL Server) está definido en specs/tech-stack.md
- Principio guía del tech-stack: zero-change sobre el sistema desktop existente
- Esquemas de BD existentes: auth · dbo · inventory · sale (sin migraciones en v1)

## Dependencias
- Ninguna fase previa del roadmap (Fase 0 es el punto de partida)
- .NET 10 SDK instalado localmente
- Node.js con npm instalado localmente
- Acceso al SQL Server existente para configurar la cadena de conexión
