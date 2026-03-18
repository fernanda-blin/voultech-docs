---
title: Movimientos
excerpt: >-
  Aportes, retiros, retiros Shinkansen, órdenes de compra/venta de instrumentos
  y operaciones de divisas (spot).
deprecated: false
hidden: false
metadata:
  robots: index
---
Gestiona el ciclo completo de operaciones financieras: aportes, retiros, compra/venta de instrumentos y operaciones de divisas.

## Flujo estándar

El flujo de movimientos depende del modelo de negocio de cada fintech. Un ejemplo típico:

1. El cliente realiza un **aporte**
2. La fintech recibe la notificación a través del **Sistema de Eventos**
3. El cliente realiza una **compra de divisas extranjeras**
4. Ejecuta una **orden de compra de instrumento financiero**
5. Al cerrar la inversión, **vende el instrumento** y **reconvierte la divisa**
6. Se realiza el **retiro de fondos**

<br />

## Aportes y retiros

Permite el ingreso manual de aportes o retiros en el sistema, sumando o restando caja a un cliente.

**→ POST** `/api/publicapi/creasys/Movimientos/IngresoAporteRetiro` — Aporte o retiro puntual

**→ POST** `/api/publicapi/creasys/Movimientos/IngresoAporteRetiroMasivo` — Aportes o retiros masivos

**Parámetros:**

| Parámetro | Tipo | Descripción |
|---|---|---|
| `Uuid` | string | Identificador único de idempotencia del movimiento |
| `CodTipoMovimiento` | string | `APO_PAT` (aporte) o `RET_PAT` (retiro) |
| `NumCuenta` | string | Número de la cuenta donde se aplica el movimiento |
| `DscMedioPagoCobro` | string | Medio de pago: `TRANSFERENCIA`, `EFECTIVO`, `CHEQUE`, etc. |
| `CodMoneda` | string | Moneda del movimiento: `CLP`, `USD`, `EUR` |
| `MtoMovimiento` | decimal | Monto del aporte o retiro |
| `ObsMovimiento` | string | Observación o comentario opcional |

```json title="Request Body (masivo)"
[
  {
    "Uuid": "123-123-123",
    "CodTipoMovimiento": "APO_PAT",
    "NumCuenta": "XXXXXXX/X",
    "ObsMovimiento": "Aporte Patrimonial",
    "FechaMovimiento": "2024-06-11",
    "Monto": "1000000",
    "CodMoneda": "CLP",
    "DscMedioPagoCobro": "TRANSFERENCIA",
    "FechaLiquidacion": "2024-06-11",
    "Banco": "banco itau",
    "NumeroCuenta": "XXXXXXXX",
    "TipoCuenta": "Cuenta Corriente"
  }
]
```

<Accordion title="Catálogo de errores — Aportes/Retiros" icon="fa-duotone fa-circle-exclamation">

| Código | Descripción |
|---|---|
| ARP-001 | No se encontró la cuenta `{numCuenta}` |
| ARP-002 | Falta ingresar NumeroCuenta de Banco |
| ARP-003 | No se encontró caja vigente `{codMoneda}` para la cuenta `{numCuenta}` |
| ARP-004 | Tipo Origen Mov Caja `{codTipoMovimiento}` no existe |
| ARP-005 | Falta ingresar TipoCuenta de Banco |
| ARP-006 | Falta ingresar banco |
| ARP-007 | Fecha Movimiento o Fecha Liquidacion NO es igual a la fecha actual (hoy) |
| ARP-008 | Cuando CodMoneda es CLP el valor del Monto no puede contener decimales |
| ARP-009 | La fecha operación debe ser igual a la fecha máxima de las operaciones ingresadas |
| ARP-010 | Monto Máximo Permitido para RET_PAT_BA 7.000.000 |
| ARP-011 | Saldo disponible insuficiente para ejecutar este movimiento |
| ARP-012 | UUID duplicado en la misma transacción |
| ARP-013 | UUID ya utilizado con anterioridad en otro movimiento de caja |
| ARP-014 | La hora actual fuera de horario permitido |
| ARP-015 | Excepción del sistema |

</Accordion>

<br />

## Retiros vía Shinkansen

**→ POST** `/api/publicapi/creasys/Movimientos/IngresoAporteRetiroMasivo`

Los retiros vía Shinkansen permiten transferencias automáticas y rápidas a cuentas bancarias del mismo cliente:

- **Montos ≤ 5.000.000 CLP** → automáticos e instantáneos
- **Montos hasta 7.000.000 CLP** → se procesan aproximadamente a las 16:00 hrs, requieren firma de apoderado

<Callout icon="🚨" theme="danger">
  Solo se permiten transferencias **a cuentas bancarias del mismo cliente**, nunca a terceros. El monto total no puede superar las 1.000 UF.
</Callout>

**Parámetros:**

| Parámetro | Descripción |
|---|---|
| `CodTipoMovimiento` | Retiro patrimonial banco: `RET_PAT_BA` |
| `NumCuenta` | Cuenta del cliente |
| `CodMoneda` | Por el momento solo `CLP` |
| `Banco` | Banco del cliente |
| `NumeroCuenta` | Número de cuenta bancaria del cliente |
| `TipoCuenta` | Tipo de cuenta bancaria |
| `Uuid` | Identificador único de idempotencia |

```json title="Request Body"
[
  {
    "Uuid": "xxx-xxxx-xxxx-xxxx-xxxx-xxxx",
    "CodTipoMovimiento": "RET_PAT_BA",
    "NumCuenta": "17931004/60",
    "ObsMovimiento": "RETIRO PATRIMONIAL SHINKANSEN",
    "FechaMovimiento": "2024-02-06T00:00:00",
    "Monto": 1000,
    "CodMoneda": "CLP",
    "DscMedioPagoCobro": "TRANSFERENCIA",
    "FechaLiquidacion": "2024-02-06T00:00:00",
    "Banco": "Banco BICE",
    "NumeroCuenta": "37684701",
    "TipoCuenta": "Cuenta Corriente"
  }
]
```

<Callout icon="💡" theme="info">
  Una vez ingresado el retiro, se retorna un `id` que puedes usar para consultar el estado vía `GET /MovimientosShinkansen`.
</Callout>

<br />

## Aporte/Retiro con cuenta remunerada

**→ POST** `/api/publicapi/creasys/Operaciones/IngresoOperacionCuentaRemunerada`

Ingresa una operación de inversión o rescate que se traduce automáticamente en una orden financiera y un movimiento de caja. El flujo incluye:

1. Creación de **movimiento de caja**
2. Creación de **orden de compra/venta de instrumento**
3. Impacto directo en **cartera y caja del cliente**

**Parámetros:**

| Parámetro | Descripción |
|---|---|
| `Uuid` | Identificador único para la orden (idempotencia) |
| `NumCuenta` | Número de cuenta del cliente |
| `CodTipoOperacion` | `INVERSION` o `RESCATE` |
| `Nemotecnico` | Código Bolsa del instrumento |
| `FechaOperacion` | Fecha de la operación (ISO 8601) |
| `Monto` | Monto total de la operación |

```json title="Request Body"
[
  {
    "idOperacion": 0,
    "uudi": "xxx-xxxx-xxxx-xxxx",
    "numCuenta": "12345678/80",
    "CodTipoOperacion": "INVERSION",
    "nemotecnico": "VECTOR-A",
    "fechaOperacion": "2024-08-27T20:55:41.260Z",
    "monto": 1000
  }
]
```

<Accordion title="Catálogo de errores — Cuenta Remunerada" icon="fa-duotone fa-circle-exclamation">

| Código | Descripción |
|---|---|
| OCR-001 | No existe cuenta disponible con el número `{NumCuenta}` |
| OCR-002 | No se pudo obtener el precio para la operación |
| OCR-003 | No existe operación concepto |
| OCR-004 | No existe instrumento con nemotécnico |
| OCR-005 | No se encontró caja vigente `{monedaTransaccion}` para `{NumCuenta}` |
| OCR-006 | El UUID ya ha sido procesado previamente |
| OCR-007 | Error en la operación |

</Accordion>

<br />

## Orden de compra/venta de instrumentos

**→ POST** `/api/publicapi/creasys/Ordenes/IngresarOrdenesMercado`

Ingresa una operación de compra o venta de un instrumento. Esta orden va directamente al sistema Voultech y al mercado a ejecutarse.

**Parámetros:**

| Parámetro | Descripción |
|---|---|
| `uuid` | ID único para la orden (idempotencia) |
| `numCuenta` | Número de cuenta del cliente |
| `tipoOperacion` | `C` (Compra) o `V` (Venta) |
| `cantidad` | Cantidad a transar |
| `nemotecnico` | Código Bolsa del instrumento |
| `tipoSeguridad` | Según [FIX Dictionary 4.2](https://www.onixs.biz/fix-dictionary/4.2/tagnum_167.html) |
| `codBolsa` | Bolsa de Santiago: `XSGO` |
| `tipoLiquidacion` | `CASH` (PH), `NEXT_DAY` (PM) o `T2` (CN) |
| `comision` | Comisión porcentual (opcional, máx. 2 decimales, entre `0` y `1`) |

```json title="Request Body"
[
  {
    "uudi": "abcdefgh-12kl-3456-mnop789qrstu",
    "numCuenta": "11931044/80",
    "nemotecnico": "COPEC",
    "cantidad": 100,
    "precio": 1000,
    "tipoPrecio": "LIMIT",
    "tipoOperacion": "C",
    "tipoSeguridad": "CS",
    "tipoLiquidacion": "T2",
    "codBolsa": "XSGO",
    "comision": 0
  }
]
```

<Callout icon="💡" theme="info">
  Valores especiales en `cantidad` de la respuesta: `98` = Cancelado, `99` = Rechazado, `50` = Parcialmente asignado, `100` = Asignado.
</Callout>

### Anular orden

**→ POST** `/api/publicapi/creasys/Ordenes/AnularOrden`

```json title="Request Body"
[
  {
    "uudi": "abcdefgh-12kl-3456-mnop789qrstu",
    "numCuenta": "12345678/0"
  }
]
```

<br />

## Compra/venta de divisas (Spot)

**→ POST** `/api/publicapi/creasys/Operaciones/IngresoOperacionSpot`

Ingresa una operación spot de compra o venta de divisas.

<Callout icon="⚠️" theme="warning">
  Para comprar efectivamente las divisas debes conectarte a la **API FX de Voultech**.
</Callout>

**Parámetros:**

| Parámetro | Descripción |
|---|---|
| `CodTipoOperacion` | `COMPRA` o `VENTA` |
| `Contraparte` | Usar `M/X` como contraparte default |
| `CodMonedaOperacion` | `CLP`, `USD`, `EUR` |
| `CodMonedaPagoCobro` | `CLP`, `USD`, `EUR` |
| `Precio` | Precio unitario final entregado al cliente |
| `PrecioMesa` | Precio de la mesa (obtenido con API FX) |
| `PrecioTransferencia` | Debe ser igual a `PrecioMesa` |
| `Monto` | Cantidad × Precio. Siempre en CLP, redondeado sin decimales |

```json title="Request Body"
[
  {
    "idOperacion": 0,
    "NumCuenta": "17931004/80",
    "CodTipoOperacion": "VENTA",
    "ObsOperacion": "Prueba",
    "FechaOperacion": "2023-05-26",
    "FechaLiquidacion": "2023-05-26",
    "Contraparte": "M/X",
    "CodMonedaOperacion": "USD",
    "CodMonedaPagoCobro": "CLP",
    "Cantidad": "1",
    "Precio": "500",
    "PrecioMesa": "500",
    "PrecioTransferencia": "500",
    "Monto": "500"
  }
]
```

<Accordion title="Catálogo de errores — Operaciones Spot" icon="fa-duotone fa-circle-exclamation">

| Código | Descripción |
|---|---|
| SPT-001 | No existe cuenta disponible con el número `{numCuenta}` |
| SPT-002 | No se encontró caja vigente `{CodMonedaOperacion}` para la cuenta `{numCuenta}` |
| SPT-003 | No existe caja vigente `{CodMonedaPagoCobro}` para la cuenta `{numCuenta}` |
| SPT-004 | No se encuentra tipo de operación `{CodTipoOperacion}` para Spot |
| SPT-005 | No existe instrumento con nemotécnico `{CodMonedaOperacion}` |
| SPT-006 | No existe la contraparte `{Contraparte}` |
| SPT-007 | Tipo Origen Mov Caja Operación no encontrado |
| SPT-008 | Tipo Origen Mov Caja Pago Cobro no se encuentra |
| SPT-009 | La fecha operación debe ser igual a la fecha máxima de operaciones ingresadas |
| SPT-010 | El monto ingresado no corresponde a (P×Q) |
| SPT-011 | Saldo disponible insuficiente para ejecutar esta operación |
| SPT-012 | UUID duplicado en la misma transacción |
| SPT-013 | UUID ya utilizado en operación anterior |
| SPT-014 | Error del sistema |
| SPT-015 | Excepción del sistema |

</Accordion>