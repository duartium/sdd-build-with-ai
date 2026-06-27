# Validation — Scaffolding

> Rama: feat/2026-06-27-scaffolding

## Criterios de éxito

### Funcionales
- [ ] GET http://localhost:5000/health devuelve 200 OK con cuerpo `{ "status": "ok" }`
- [ ] La SPA carga en http://localhost:5173 sin errores en consola del browser

### Técnicos
- [ ] `dotnet build HendrixAccountant.sln` completa sin errores
- [ ] HendrixAccountant.Web.Api está registrado en HendrixAccountant.sln
- [ ] HendrixAccountant.Web.Api referencia HendrixAccountant.ApplicationCore
- [ ] appsettings.json contiene la sección ConnectionStrings sin valores reales comiteados
- [ ] `npm install` en hendrix-web completa sin errores

### De integración
- [ ] Los proyectos desktop existentes (HendrixAccountant, ApplicationCore, Data, etc.)
      siguen compilando sin cambios tras agregar los proyectos nuevos

## Pasos de validación manual
1. Desde la raíz del repo ejecutar: `dotnet run --project HendrixAccountant.Web.Api`
2. Abrir browser o curl: GET http://localhost:5000/health → esperar 200 con `{ "status": "ok" }`
3. En otra terminal, entrar a hendrix-web y ejecutar: `npm run dev`
4. Abrir browser en http://localhost:5173 → verificar que la SPA carga sin errores en consola (F12)
5. Detener ambos servidores

## Definición de Done
Esta fase puede mergearse cuando:
- Todos los criterios funcionales están marcados ✅
- Todos los criterios técnicos están marcados ✅
- El desarrollador ejecutó los pasos de validación manual
- No hay regresiones en el sistema existente
