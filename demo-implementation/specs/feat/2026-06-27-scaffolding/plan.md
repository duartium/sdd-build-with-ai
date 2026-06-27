# Plan — Scaffolding

> Rama: feat/2026-06-27-scaffolding
> Fase: 0 del roadmap

## Grupos de tareas

### Grupo 1 — Proyecto Web API
- [ ] Crear HendrixAccountant.Web.Api con `dotnet new webapi --use-minimal-apis` targeting .NET 10
- [ ] Agregar HendrixAccountant.Web.Api al HendrixAccountant.sln
- [ ] Agregar referencia a HendrixAccountant.ApplicationCore
- [ ] Configurar cadena de conexión a SQL Server en appsettings.json (sin valores reales comiteados)
- [ ] Implementar endpoint GET /health → 200 OK con cuerpo `{ "status": "ok" }`
- [ ] Verificar que `dotnet run` inicia en puerto 5000 sin errores

### Grupo 2 — Proyecto SPA React + Vite
- [ ] Crear hendrix-web con `npm create vite@latest hendrix-web -- --template react`
- [ ] Instalar dependencias con `npm install`
- [ ] Verificar que `npm run dev` inicia en puerto 5173 sin errores en consola
- [ ] Verificar que la SPA carga correctamente en browser en localhost:5173
