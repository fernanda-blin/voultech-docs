---
title: Introducción a Alpaca
excerpt: >-
  Qué permite el módulo internacional con Alpaca, flujo recomendado de
  integración y endpoints principales.
deprecated: false
hidden: false
metadata:
  robots: index
---
Opera cuentas internacionales con Alpaca: crea cuentas, consulta saldos y custodias, busca instrumentos y registra órdenes y movimientos en USD.

## Qué puedes hacer con este módulo

- **Crear una cuenta Alpaca** asociada a una cuenta local existente
- **Consultar cuentas y saldos** internacionales del asesor autenticado
- **Buscar instrumentos** disponibles para operar en mercados internacionales
- **Ingresar órdenes** de compra o venta usando `CodBolsa = "ALPACA"`
- **Consultar actividad, órdenes y custodias** de una cuenta Alpaca
- **Registrar movimientos internacionales** de aporte o retiro patrimonial

<br />

## Antes de empezar

Para usar el módulo internacional necesitas:

- Una cuenta local existente en GPI (`numCuenta`)
- El identificador del cliente (`identificador`)
- Un usuario autenticado con acceso al asesor correspondiente

<Callout icon="💡" theme="info">
  La creación de cuenta internacional se realiza sobre una cuenta local existente. En los endpoints de Alpaca verás tanto el `numCuenta` local como el `accountNumber` de Alpaca.
</Callout>

<br />

## Flujo recomendado

Sigue este orden para implementar el módulo internacional:

1. **Crear la cuenta Alpaca** con `POST /api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`
2. **Consultar la cuenta creada** con `GET /api/publicapi/creasys/CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}`
3. **Revisar saldo disponible** con `GET /api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}`
4. **Buscar instrumentos** con `GET /api/publicapi/creasys/Asset`
5. **Ingresar órdenes internacionales** con `POST /api/publicapi/creasys/Ordenes/IngresarOrdenesMercado` usando `CodBolsa = "ALPACA"`
6. **Consultar actividad, custodias y órdenes** de la cuenta
7. **Registrar movimientos internacionales** con `POST /api/publicapi/creasys/MovimientosAlpaca/MovimientoInternacionalAlpaca`

<br />

## Endpoints principales

<Cards>
  <Card title="Cuentas Internacionales" href="#" icon="fa-duotone fa-globe">
    Crea cuentas Alpaca, consulta su estado, lista las cuentas del asesor y revisa saldos.
  </Card>

  <Card title="Assets e Instrumentos" href="#" icon="fa-duotone fa-magnifying-glass-chart">
    Busca instrumentos por símbolo, nombre o bolsa para preparar órdenes internacionales.
  </Card>

  <Card title="Órdenes Internacionales" href="#" icon="fa-duotone fa-chart-line">
    Ingresa órdenes con `CodBolsa = "ALPACA"` y consulta el historial de órdenes por cuenta.
  </Card>

  <Card title="Actividad y Custodias" href="#" icon="fa-duotone fa-briefcase">
    Consulta movimientos de actividad, posiciones abiertas y valorización actual de la cuenta.
  </Card>

  <Card title="Movimientos Internacionales" href="#" icon="fa-duotone fa-money-bill-transfer">
    Registra aportes y retiros patrimoniales internacionales con `APO_PAT_IT` y `RET_PAT_IT`.
  </Card>
</Cards>

<br />

## Consideraciones operativas

- Las cuentas Alpaca operan en **USD**
- Las órdenes internacionales usan `CodBolsa = "ALPACA"`
- Para órdenes internacionales, se documenta el uso de `TipoSeguridad = "CS"` y `TipoLiquidacion = "T2"`
- Los movimientos internacionales soportados son `APO_PAT_IT` y `RET_PAT_IT`

<Callout icon="📘" theme="info">
  Usa esta sección junto con la API Reference para validar parámetros, responses y ejemplos por endpoint.
</Callout>

<br />

## Siguiente paso

Empieza por la página **Cuentas Internacionales** para crear la primera cuenta Alpaca y validar su estado.
