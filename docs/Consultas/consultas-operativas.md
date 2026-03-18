---
title: Consultas Operativas
excerpt: >-
  Consulta cajas, saldos, cartera, movimientos, órdenes, asignaciones y precios
  del sistema.
deprecated: false
hidden: false
metadata:
  robots: index
---
Consulta información dinámica del sistema: cajas, saldos, movimientos, órdenes, carteras y operaciones de clientes.

## Cajas con saldo online

**→ GET** `/api/publicapi/creasys/Cajas/ConSaldoOnline`

Devuelve las cajas registradas de una cuenta según la fecha consultada. Incluye únicamente **parámetros operativos** (sin información del cliente).

<br />

## Cajas

**→ GET** `/api/publicapi/creasys/Cajas`

Devuelve **todas las cajas registradas** por fecha consultada. Incluye tanto parámetros operativos como los **datos del cliente propietario**.

<br />

## Operaciones

**→ GET** `/api/publicapi/creasys/Operaciones`

Entrega información detallada de las operaciones de un cliente dentro de un **periodo determinado**.

<br />

## Cartera

**→ GET** `/api/publicapi/creasys/Cartera`

Entrega la cartera de inversiones de un cliente, incluyendo posiciones, instrumentos y valorizaciones vigentes.

**→ GET** `/api/publicapi/creasys/Cartera/CarteraResumida`

Versión **resumida** de la cartera, con menor nivel de detalle que la consulta principal.

**→ GET** `/api/publicapi/creasys/Cartera/CarteraDetallada`

Versión **ligera y simplificada**, priorizando la velocidad de respuesta sobre la cantidad de datos entregados.

<Callout icon="💡" theme="info">
  `CarteraDetallada` es ideal para vistas rápidas y frecuentes, o consultas de alto rendimiento y masivas.
</Callout>

<br />

## Movimientos

**→ GET** `/api/publicapi/creasys/Movimientos`

Entrega los movimientos de una cuenta o cliente, filtrables por rango de fechas o tipo de operación.

<br />

## Órdenes

**→ GET** `/api/publicapi/creasys/Ordenes`

Consulta las órdenes generales registradas en el sistema dentro de un rango de fechas.

<br />

## Asignaciones

**→ GET** `/api/publicapi/creasys/Asignaciones`

Devuelve las asignaciones de órdenes o instrumentos asociadas a un cliente o portafolio específico.

<br />

## Publicador de precios

**→ GET** `/api/publicapi/creasys/PublicadorPrecio`

Entrega los precios publicados de instrumentos financieros disponibles en la corredora.

<br />

## Movimientos Shinkansen

**→ GET** `/api/publicapi/creasys/Movimientos/MovimientosShinkansen`

Consulta los movimientos procesados a través del sistema Shinkansen, utilizados para conciliaciones o trazabilidad de aportes y retiros automáticos.

<br />

## Movimientos Banco Security

**→ GET** `/api/publicapi/creasys/Movimientos/MovimientosBancoSecurity`

Devuelve los movimientos bancarios sincronizados con **Banco Security**, incluyendo abonos y débitos relevantes para las cuentas operativas.

<br />

## Movimientos Banco BICE

**→ GET** `/api/publicapi/creasys/Movimientos/MovimientosBancoBice`

Devuelve los movimientos bancarios sincronizados con **Banco BICE**, en formato estándar de conciliación con el sistema Creasys.