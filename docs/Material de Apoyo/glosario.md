---
title: Glosario
excerpt: >-
  Términos técnicos de integración y conceptos financieros utilizados en la API
  de Voultech.
deprecated: false
hidden: false
metadata:
  robots: index
---
Definiciones de términos técnicos de integración y conceptos financieros utilizados en la API de Voultech.

## Términos técnicos de integración

| Término | Definición |
|---|---|
| **Asesor** | Código que identifica a la fintech dentro del sistema. Debe estar presente en todas las operaciones |
| **Cliente** | Persona natural o jurídica enrolada en la plataforma. Asociado a cuentas, documentos y operaciones |
| **Cuenta** | Estructura de inversión ligada a un cliente y a un asesor. Puede tener múltiples cajas/divisas |
| **Caja** | Representa los fondos disponibles por divisa en una cuenta |
| **Cuenta Corriente** | Cuenta bancaria del cliente donde se reciben abonos o se ejecutan retiros. Solo el titular puede usarla |
| **Movimiento** | Flujo de dinero en una caja: abonos (aportaciones) o retiros |
| **Orden** | Instrucción de compra o venta de un instrumento financiero |
| **Asignación** | Resultado de la ejecución de una orden en el mercado |
| **Evento** | Mensaje asincrónico que notifica cambios en estados (abonos, KYC, retiros, precios) |
| **Uuid** | Identificador único para operaciones POST, evita duplicaciones si se reintenta el envío |
| **Shinkansen** | Proceso de retiro bancario inmediato disponible para montos bajos (CLP) |
| **KYC** | Proceso de validación y conocimiento del cliente |
| **Mandato** | Documento obligatorio que autoriza a operar en nombre del cliente |
| **Contrato** | Documento legal que respalda el acuerdo entre cliente y la plataforma |
| **ciFrontal / ciReverso** | Imágenes del carnet de identidad en base64 |
| **codTipo** | Código que indica el tipo de documento al ser cargado (ej: `ciFrontal`) |
| **codMoneda** | Código ISO de la divisa usada (`CLP`, `USD`, `EUR`) |
| **Perfil de Riesgo** | Clasificación del cliente según tolerancia al riesgo: CONSERVADOR, MODERADO, AGRESIVO, etc. |
| **Operación Spot** | Compra o venta inmediata de divisas con liquidación casi instantánea |
| **Webhook** | Sistema de notificación push usado para recibir eventos |
| **FechaMovimiento** | Fecha en la que se solicita o registra un abono/retiro |
| **FechaLiquidacion** | Fecha efectiva en la que se hace firme una operación |
| **Cartera** | Conjunto de instrumentos financieros que posee una cuenta |
| **Cartola** | Reporte PDF de saldos y movimientos de una cuenta |
| **Datos en Proceso** | Indica si hay movimientos/saldos en actualización que podrían afectar la información mostrada |
| **CodEstadoCartera** | Tipo de posición en cartera: `VIG` (vigente), `PRE` (preasignada), `GTA` (guardada temporalmente) |
| **Identificador** | Número único del cliente, puede ser RUT o pasaporte |
| **SignIn / RefreshToken** | Endpoints para iniciar sesión y renovar el token de autenticación |
| **Paginación** | Manejo de listas de datos: se usan `PageSize`, `PageNumber`, y headers `X-Pagination` |

<br />

## Términos financieros

| Término | Definición |
|---|---|
| **Activo financiero** | Instrumento con valor monetario que puede ser negociado, como acciones, bonos, ETFs, etc. |
| **Asignación de activos** | Distribución estratégica de inversiones entre distintos instrumentos para diversificar riesgo |
| **Base monetaria** | Total de dinero emitido por un banco central, incluyendo efectivo y reservas bancarias |
| **Clearing** | Proceso de validación y compensación previo a la liquidación de operaciones |
| **CLP** | Código ISO 4217 del peso chileno, moneda nacional de Chile |
| **Cuenta omnibus** | Cuenta que agrupa las inversiones de varios clientes bajo un solo titular operativo |
| **Divisa** | Moneda extranjera usada para operaciones de cambio (ej: USD, EUR) |
| **Dólar Spot** | Compra/venta inmediata de dólares a precio de mercado |
| **ETF** | Fondo indexado que cotiza en bolsa como una acción |
| **FATCA** | Ley estadounidense que exige a instituciones financieras identificar clientes con nacionalidad o residencia fiscal en EE.UU. |
| **Forward** | Contrato para realizar una operación futura a un precio pactado hoy |
| **Instrumento financiero** | Cualquier contrato o título con valor económico que puede ser negociado |
| **Liquidez** | Facilidad para convertir un activo en efectivo sin perder valor |
| **Mandato** | Documento firmado que otorga a la fintech o broker el poder de operar por cuenta del cliente |
| **Mercado secundario** | Donde se negocian instrumentos ya emitidos (ej: acciones en bolsa) |
| **Nemotécnico (nemo)** | Código o símbolo que representa un instrumento en el mercado |
| **PEP** | Persona con exposición política. Requiere mayor supervisión debido a su rol público |
| **Perfil de riesgo** | Clasificación del inversor según su tolerancia a pérdidas |
| **Plazo de liquidación** | Días entre la ejecución de una operación y la entrega de fondos o títulos |
| **Riesgo de contraparte** | Riesgo de que la otra parte no cumpla con su obligación contractual |
| **Spread** | Diferencia entre precio de compra (bid) y venta (ask) de un activo |
| **Subcustodio** | Entidad que resguarda activos financieros en nombre de un custodio principal |
| **Tasa de cambio** | Precio relativo entre dos divisas |
| **Valor liquidativo** | Valor de una participación en un fondo en un momento determinado |
| **Volatilidad** | Medida de la variación del precio de un activo en un período determinado |