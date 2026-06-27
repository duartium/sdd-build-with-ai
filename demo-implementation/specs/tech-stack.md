# Tech Stack

> Decisiones tomadas al 2026-06-27
> Toda decisión fuera de este documento requiere
> actualizar este archivo primero — antes de implementar

## Principio guía
Reutilizar al máximo la lógica existente (ApplicationCore, SPs, esquemas de BD)
— zero-change sobre el sistema desktop en v1.

## Stack

### Backend — HendrixAccountant.Web.Api

| Área | Decisión | Justificación |
|---|---|---|
| Framework | ASP.NET Core 10 Web API | Mismo ecosistema .NET del proyecto desktop; minimal APIs modernas |
| Acceso a datos | ApplicationCore existente + Stored Procedures | Reutiliza toda la lógica de negocio validada; zero-rewrite en v1 |
| Base de datos | SQL Server (sin cambios de esquema) | Esquemas auth, dbo, inventory, sale intactos; sin migraciones en v1 |
| Autenticación | JWT emitido vía `SP_USER_AUTHENTICATION` | Sin reescribir lógica de auth; el SP existente valida credenciales |
| Serialización | System.Text.Json | Incluido en .NET 10; sin dependencias adicionales |

### Frontend — hendrix-web

| Área | Decisión | Justificación |
|---|---|---|
| Framework | React 18 + Vite | Desacoplado del backend; mobile-first; ecosistema JS ampliamente adoptado |
| Estilos | Por definir antes de Fase 2 | — |
| Estado / Data fetching | Por definir antes de Fase 2 | — |
| HTTP Client | Por definir antes de Fase 2 | — |

## Fuera del stack — justificación explícita

| Tecnología | Por qué excluida |
|---|---|
| Blazor | Frontend React+Vite ya decidido; Blazor sería capa innecesaria |
| Entity Framework (nuevos endpoints) | Los SPs existentes cubren el acceso; EF introduciría deuda sin beneficio en v1 |
| Next.js / SSR | Dashboard interno sin requisito de SEO; Vite SPA es suficiente |
| SignalR | Alertas en v1 se implementan por polling; SignalR se evalúa en v2 |

## Arquitectura

```
[Browser / Mobile]
        │
        │ HTTPS :443
        ▼
[React + Vite SPA]             hendrix-web   (dev: 5173 │ prod: Nginx)
        │
        │ REST / JSON :5000
        ▼
[ASP.NET Core 10 API]          HendrixAccountant.Web.Api
        │
        │ ADO.NET / Stored Procedures
        ▼
[SQL Server]                   Esquemas: auth · dbo · inventory · sale
        ▲
        │ Escribe datos
[WinForms POS]                 Sistema desktop existente (sin cambios)
```

## Hosting

| Entorno     | Componente | Tecnología                        |
|-------------|------------|-----------------------------------|
| Desarrollo  | API        | `dotnet run` local (puerto 5000)  |
| Desarrollo  | Frontend   | Vite dev server (puerto 5173)     |
| Producción  | Ambos      | Por definir antes del primer deploy |
