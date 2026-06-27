# Mission — Estado Actual del Sistema

> Documento de línea base. Captura lo que existe hoy, no lo que se planea construir.

## ¿Qué hace este sistema en producción?

HendrixAccountant es un sistema de punto de venta (POS) y gestión comercial para una boutique de ropa. Está en producción activa y cubre el ciclo completo de una venta: desde la búsqueda y selección de productos en el POS, pasando por el cálculo de subtotales, IVA y descuentos, hasta la emisión del comprobante (factura impresa o ticket térmico).

El sistema también gestiona el ciclo de caja: apertura de caja al inicio del día, registro de gastos durante la jornada y cierre de caja con resumen de movimientos. Las ventas se registran con trazabilidad completa hacia el inventario mediante kardex automático, y es posible anular comprobantes emitidos.

La aplicación centraliza además la gestión del catálogo de productos (altas, bajas, categorías, tallas, marcas, colores), el registro de proveedores, las entradas de mercadería con ajuste de stock, y el historial de clientes incluyendo datos de cumpleañeros. Genera reportes de ventas con filtros por fecha, cliente y usuario.

## ¿Quién lo usa y para qué?

**Cajero/a:** Utiliza el módulo de Punto de Venta (POS) para registrar ventas, buscar clientes por identificación, seleccionar productos, procesar pagos en efectivo o tarjeta, e imprimir tickets o facturas. También realiza la apertura y cierre de caja diario.

**Administrador de tienda:** Gestiona el catálogo de productos (altas, modificaciones, bajas), registra entradas de mercadería vinculadas a proveedores, administra clientes y usuarios del sistema, revisa el kardex de inventario y consulta reportes de ventas.

## ¿Qué problemas resuelve actualmente?

- Registro y facturación de ventas al por menor con cálculo automático de IVA y descuentos
- Control de caja diaria: apertura, gastos y cierre con saldo final
- Gestión de inventario con kardex de entradas y salidas por venta
- Administración de catálogo de productos con atributos propios de moda (talla, color, marca)
- Trazabilidad de clientes con historial de compras y filtro de cumpleañeros
- Emisión de comprobantes impresos (factura A4 y ticket térmico personalizado)
- Anulación de comprobantes emitidos
- Reimpresión de tickets de venta
- Consulta de reportes de ventas con múltiples filtros

## ¿Qué está explícitamente fuera de su alcance actual?

- No tiene módulo de contabilidad formal (asientos contables, balances, estado de resultados)
- No tiene facturación electrónica conectada al SRI (el XML se genera pero no hay integración con el webservice del SRI)
- No tiene interfaz web ni aplicación móvil; es exclusivamente una aplicación de escritorio Windows
- No tiene soporte multi-sucursal ni multi-empresa
- No tiene gestión de devoluciones o notas de crédito
- No tiene módulo de nómina ni recursos humanos
- No tiene integración con pasarelas de pago electrónicas (solo efectivo y tarjeta registradas manualmente)
- No tiene módulo de reportes de inventario (solo ventas)

---
*Generado: 2026-06-27 | Rama: build-with-ai-demo*
