# Mission

> Línea base versionada · Estado al 2026-06-27

## ¿Qué es este sistema?
HendrixAccountant Web es una aplicación web de solo lectura que expone en
tiempo real los datos operativos del sistema POS de escritorio (ventas,
inventario, caja) a gerentes y supervisores de turno desde cualquier
dispositivo con navegador, sin necesidad de estar físicamente en el local.

## Usuarios

| Usuario              | Tarea específica                              | Necesidad principal                            |
|----------------------|-----------------------------------------------|------------------------------------------------|
| Gerente              | Monitorear ventas del día y por turno         | Ver totales en tiempo real desde cualquier lugar |
| Supervisor de turno  | Verificar cierres de caja y stock de inventario | Confirmar operaciones del turno sin acceder al POS |

## Problema que resuelve
- Los gerentes y supervisores no pueden consultar ventas, inventario ni caja en
  tiempo real sin estar físicamente frente al POS de escritorio.
- La información operativa queda bloqueada en una sola máquina, limitando la
  capacidad de supervisión remota.
- Las decisiones de turno (validación de cierres, detección de faltantes)
  se retrasan por falta de visibilidad.

## Alcance v1
- Dashboard de ventas del día: totales, desglose por turno y por cajero, accesible sin login (modo kiosco)
- Autenticación JWT integrada con el stored procedure existente `SP_USER_AUTHENTICATION`
- Resumen de caja: aperturas, cierres y totales del día (vista autenticada)
- Consulta de inventario: stock actual de productos (vista autenticada)
- Alertas y notificaciones: faltantes de inventario, cierres pendientes y umbrales superados

## Fuera de alcance — v1
- Crear, editar o eliminar datos desde la web (la aplicación es estrictamente de solo lectura)
- Portal o acceso para clientes externos
- Integración con proveedores o sistemas ERP externos
- Reportes históricos, exportación de datos o impresión desde web
- Gestión de usuarios desde la web
