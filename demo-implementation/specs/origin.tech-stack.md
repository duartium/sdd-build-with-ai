# Tech Stack — Estado Actual del Sistema

> Documento de línea base. Refleja las tecnologías encontradas en el codebase, no las deseadas.

## Proyectos de la solución

| Proyecto | Tipo | Responsabilidad |
|----------|------|-----------------|
| `HendrixAccountant.Application` | exe (WinForms) | App principal: todos los formularios de UI (POS, caja, inventario, clientes, reportes, configuración) |
| `HendrixAccountant.ApplicationCore` | lib | Núcleo de dominio: entidades, interfaces, DTOs, mappers manuales, `ProductService` |
| `HendrixAccountant.Infrastructure.Data` | lib | Repositorios, `AppDbContext` (EF6), `UnitOfWork`, servicios de datos (`SaleService`, `SaleReportService`, `BarCodeService`) |
| `HendrixAccountant.Infrastructure.Shared` | lib | Servicios externos: Google Drive, email via SendGrid, PDF via iText7 |
| `HendrixAccountant.Common` | lib | Utilidades compartidas: ADO.NET helper (`DataBase`), cifrado AES, exportación a Excel, controles base |
| `HendrixAccountant.UIControls` | lib (WinForms UserControls) | Controles personalizados reutilizables: `TextInput`, `NumericInput`, `DecimalInput`, `ItemMenu`, `ItemAside` |
| `HendrixAccountant.Database` | SQL project | Scripts de creación de base de datos y stored procedures |

## Arquitectura del sistema

El sistema aplica **Clean Architecture** en tres capas principales: `ApplicationCore` (dominio sin dependencias externas), `Infrastructure.Data` (implementación de repositorios y acceso a datos), y `Application` (UI WinForms que orquesta los casos de uso).

El acceso a datos es **híbrido**: los formularios de búsqueda y operaciones complejas usan stored procedures vía ADO.NET puro (clase `DataBase` → `ISqlServer.ExecuteProcedure()`). Las entidades simples usan EF6 (`AppDbContext` + `Repository<T>`). Los datos retornados por SPs se mapean manualmente a DTOs mediante clases `*Mapper` en `ApplicationCore/Map/`.

La UI no tiene capa de presentación formal (MVP/MVVM); la lógica de UI vive directamente en los formularios WinForms. Los formularios modales de búsqueda se comunican con los formularios padre mediante interfaces (`IFindElement`, `ISaleElement`).

## Tecnologías, frameworks y versiones

| Categoría | Tecnología | Versión |
|-----------|-----------|---------|
| Runtime | .NET Framework | 4.6.2 |
| ORM | Entity Framework | 6.4.4 |
| UI | Windows Forms (WinForms) | .NET 4.6.2 |
| Base de datos | SQL Server (LocalDB en dev) | — |
| Reportes | Microsoft RDLC (ReportViewer) | 140.340.80 |
| PDF | iText7 | 7.1.14 |
| Email | SendGrid (vía `SendgridMailService`) | — |
| Almacenamiento | Google Drive API (vía `GoogleService`) | — |
| Serialización | Newtonsoft.Json | 12.0.3 |
| Código de barras | BarcodeLib | 1.3.0.0 |
| Cifrado | Portable.BouncyCastle | 1.8.9 |

## Patrones de diseño identificados

- **Repository**: `IProductRepository → ProductoRepository`, `IClientRepository → ClienteRepository`, `ISupplierRepository → SupplierRepository`, `IKardexRepository → KardexRepository`, etc. Todos en `HendrixAccountant.Data/Repositories/`.
- **Generic Repository**: `IRepository<T>` / `Repository<T>` como base para repositorios EF6, provee `GetAll()`, `GetById()`, `Add()`, `Remove()`.
- **Unit of Work**: `IUnitOfWork` (ApplicationCore) → `UnitOfWork` (Data) que envuelve `AppDbContext` con lazy-init de repositorios (`Products`, `Colors`, `Brands`, `Categories`).
- **Mapper manual**: Clases `*Mapper` en `ApplicationCore/Map/` (`ProductMapper`, `ClientMapper`, `KardexMapper`, `SupplierMapper`, `CashMapper`, `CategoryMapper`) que convierten `DataSet → DTO/Entidad`.
- **Service Layer**: `IProductService → ProductService` en ApplicationCore; `ISales → SaleService` y `IReports → SaleReportService` en Infrastructure.Data.
- **DTO**: Objetos de transferencia separados (`ProductDTO`, `ClientDto`, `InvoiceDto`, `SupplierDto`) que desacoplan la entidad de dominio del contrato de UI.
- **Interfaz de comunicación entre formularios**: `IFindElement` y `ISaleElement` permiten que formularios modales de búsqueda notifiquen al padre sin acoplamiento directo.

## Esquema y estructura de base de datos

Base de datos: **HENDRIX** (SQL Server). Connection string: `HendrixContext`.

### DbSet en `AppDbContext`
| DbSet | Entidad | Tabla mapeada |
|-------|---------|--------------|
| `Products` | `Product` | `inventory.PRODUCTOS` |
| `ProductEntry` | `ProductEntry` | `inventory.ENTRADAS` |
| `Supplier` | `Supplier` | `inventory.PROVEEDORES` |

### Esquemas SQL identificados
| Esquema | Área | Tablas principales |
|---------|------|--------------------|
| `auth` | Seguridad | `USUARIOS`, `ROLES`, `ACCIONES`, `ROL_ACCION`, `MODULOS` |
| `management` | Clientes | `CLIENTES` |
| `inventory` | Inventario | `PRODUCTOS`, `PROVEEDORES`, `ENTRADAS`, `ENTRADAS_DETALLE`, `CATALOGOS`, `ITEM_CATALOGO` |
| `sale` | Ventas y Caja | `FACTURA`, `FACTURA_DETALLE`, `VENTAS`, `CAJA`, `MOVIMIENTOS_CAJA`, `SECUENCIAL`, `PARAMETROS` |

### Stored Procedures principales
`SP_CASH_FLOW`, `SP_CONSULTA_CLIENTES`, `SP_QRY_CLIENTS`, `SP_CONSULTA_PRODUCTOS`, `SP_GENERATE_SEQUENTIAL`, `SP_QRY_USERS`, `SP_REGISTER_SALE`, `SP_RPT_SALE`, `SP_USER_ATHENTICATION`

## Dependencias externas relevantes

| Paquete | Propósito |
|---------|-----------|
| `EntityFramework 6.4.4` | ORM para entidades simples (Product, Supplier, ProductEntry) |
| `itext7 7.1.14` | Generación de PDF (etiquetas, recibos) |
| `Microsoft.ReportingServices.ReportViewerControl.Winforms 140.340.80` | Visor de reportes RDLC integrado en la UI |
| `Newtonsoft.Json 12.0.3` | Serialización JSON (configuración, envío de datos) |
| `BarcodeLib 1.3.0.0` | Generación de imágenes de código de barras |
| `Portable.BouncyCastle 1.8.9` | Cifrado AES para contraseñas y datos sensibles |
| `System.Management 5.0.0` | Detección de hardware (serial de PC para identificar caja) |
| `SendGrid` | Envío de correos electrónicos |

## Línea gráfica

> Paleta, tipografía y estilo visual detectados en el codebase. Usar como referencia para mantener coherencia visual en nuevos formularios, componentes y assets.

### Paleta de colores

| Rol | Color (hex) | Dónde se usa |
|-----|-------------|--------------|
| Primario / fondo de sidebar | `#1B2E8C` | Panel lateral (`pnAside`), headers de formularios, botón Guardar, header de DataGridView, fondo de `ItemMenu` |
| Acento / hover y botón Buscar | `#FDB827` | Hover de `ItemMenu`, botón Buscar, botón Modificar, texto acento en login (`lblTitile2`) |
| Azul de campo activo | `#1E6BF7` | Indicador visual de inputs activos (`lblPnUsuario`, `lblPnContrasena`) |
| Secundario / limpiar | `#5BC0DE` | Botón Limpiar (acción secundaria no destructiva) |
| Peligro / eliminar | `#DC3545` | Botón Eliminar |
| Éxito / nuevo | `#28A745` | Botón Nuevo |
| Fondo de formulario | `#FFFFFF` | `BackColor` de formularios principales (SystemColors.Window) |
| Fondo de panel de campos | `SystemColors.Control` | Paneles de inputs (gris claro del sistema) |
| Texto principal | `SystemColors.WindowText` | Labels y texto de grilla |
| Texto secundario / hint | `SystemColors.GrayText` | Labels de campo (`label11`, `label3`, `label5`) |

### Tipografía

| Rol | Fuente | Tamaño | Estilo |
|-----|--------|--------|--------|
| Títulos de formulario | Arial | 12 pt | Bold |
| Botón de menú principal (`ItemMenu`) | Arial | 11 pt | Bold |
| Labels de campo y botones de acción | Arial | 9.75 pt | Regular |
| Hints / texto secundario | Arial | 9 pt | Regular |
| Campos de contraseña | Arial | 15.75 pt | Regular |
| Header de DataGridView | Microsoft Sans Serif | 8.25 pt | Regular |

### Estilo de controles

- **Botones primarios (Guardar):** Fondo `#1B2E8C`, texto blanco, `FlatStyle.Flat`, sin borde visible
- **Botones acento (Buscar, Modificar):** Fondo `#FDB827`, texto `#1B2E8C`, `FlatStyle.Flat`
- **Botones peligro (Eliminar):** Fondo `#DC3545`, texto blanco, `FlatStyle.Flat`
- **Botones éxito (Nuevo):** Fondo `#28A745`, texto blanco, `FlatStyle.Flat`
- **Botón de menú (`ItemMenu`):** Fondo `#1B2E8C`, texto blanco, 200×44 px, sin borde, hover `#FDB827`, cursor Hand
- **Campos de texto:** Fondo `SystemColors.Control`, sin borde personalizado
- **Grillas / DataGridView:** Header azul `#1B2E8C` con texto blanco; selección `SystemColors.Highlight`
- **Menú / navegación lateral:** Fondo `#1B2E8C`, hover `#FDB827`, texto blanco en bold
- **Formularios / ventanas:** Barra de título estándar Windows, sin personalización de chrome

### Logo e íconos

- **Icono de aplicación:** `HendrixIcon.ico` en `HendrixAccountant/`
- **Logo de empresa:** Cargado dinámicamente como `PictureBox` (`pbxLogoCompany`) en el formulario de login, imagen parametrizable por cliente
- **Librería de íconos:** No hay librería de íconos externa; íconos embebidos en recursos `.resx` por formulario

---
*Generado: 2026-06-27 | Rama: build-with-ai-demo*
