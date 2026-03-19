---
title: Operaciones
excerpt: >-
  Órdenes de mercado internacionales, movimientos entre cuentas locales y
  Alpaca, actividad y consulta de órdenes.
deprecated: false
hidden: false
metadata:
  robots: index
---
Ingresa órdenes de compra/venta en mercados internacionales, registra movimientos entre cuentas locales y Alpaca, y consulta actividad y órdenes.

## Ingresar órdenes de mercado

**→ POST** `/api/publicapi/creasys/Ordenes/IngresarOrdenesMercado`

Permite ingresar órdenes de mercado para cuentas **internacionales (ALPACA)** o **nacionales (XSGO)** según el `CodBolsa`. Procesa múltiples órdenes en una sola solicitud (array), cada una validada de forma independiente.

**Parámetros:**

| Parámetro | Descripción | Obligatorio | Observaciones |
|---|---|---|---|
| `Uuid` | Identificador único de la orden | Sí | Debe ser único |
| `NumCuenta` | Número de cuenta | Sí | Debe ser válida y autorizada |
| `Nemotecnico` | Código del instrumento | Sí | Para ALPACA no se valida contra catálogo local |
| `TipoSeguridad` | Tipo de seguridad | Sí | Para ALPACA usar `"CS"` |
| `TipoOperacion` | `C` (Compra) o `V` (Venta) | Sí | |
| `Cantidad` | Cantidad de títulos | Sí | Mayor a 0 |
| `Precio` | Precio unitario | Sí | Mayor a 0 |
| `CodBolsa` | Código de bolsa | Sí | `"ALPACA"` internacional, `"XSGO"` nacional |
| `TipoLiquidacion` | Tipo de liquidación | Sí | Para ALPACA usar `"T2"` |
| `TipoPrecio` | Tipo de precio | Sí | `"LIMIT"`, `"MARKET"` |
| `Comision` | Comisión (0 a 1, máx 2 decimales) | Sí | Para ALPACA usar `0` |

<Tabs>
  <Tab title="Orden Internacional (ALPACA)">

```json title="Request Body"
[
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440000",
    "NumCuenta": "19130340/0",
    "Nemotecnico": "AAPL",
    "TipoSeguridad": "CS",
    "TipoOperacion": "C",
    "Cantidad": 10,
    "Precio": 198.45,
    "CodBolsa": "ALPACA",
    "TipoLiquidacion": "T2",
    "TipoPrecio": "LIMIT",
    "Comision": 0
  }
]
```

  </Tab>
  <Tab title="Orden Nacional (XSGO)">

```json title="Request Body"
[
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440001",
    "NumCuenta": "19130340/0",
    "Nemotecnico": "FALABELLA",
    "TipoSeguridad": "CS",
    "TipoOperacion": "C",
    "Cantidad": 100,
    "Precio": 2500,
    "CodBolsa": "XSGO",
    "TipoLiquidacion": "PM",
    "TipoPrecio": "LIMIT",
    "Comision": 0.5
  }
]
```

  </Tab>
</Tabs>

```json title="Respuesta exitosa (200 OK)"
[
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440000",
    "Mensaje": "Orden internacional con UUID 550e8400-e29b-41d4-a716-446655440000 procesada correctamente.",
    "Exitoso": true
  },
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440001",
    "Mensaje": "Orden nacional con UUID 550e8400-e29b-41d4-a716-446655440001 procesada correctamente.",
    "Exitoso": true,
    "UuidBolsa": "BOL-12345"
  }
]
```

```json title="Respuesta con error (400)"
[
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440000",
    "Mensaje": "La cuenta Alpaca 19130340/0 no está autorizada para este usuario o no existe.",
    "Exitoso": false
  }
]
```

<Accordion title="Validaciones y notas" icon="fa-duotone fa-circle-info">

**Órdenes nacionales (`XSGO`):**
- La cuenta debe existir y estar disponible para el usuario
- El instrumento debe existir en el catálogo local
- El UUID no debe haber sido usado antes

**Órdenes internacionales (`ALPACA`):**
- La cuenta debe ser una cuenta Alpaca autorizada para el usuario
- El UUID no debe haber sido usado antes
- Valores por defecto: `TipoSeguridad = "CS"`, `TipoLiquidacion = "T2"`, `CodMoneda = "USD"`

**Notas:**
- Puedes enviar múltiples órdenes en una sola solicitud
- Cada orden se procesa por separado
- Las órdenes nacionales retornan `UuidBolsa`, las internacionales **no**

</Accordion>

<br />

## Movimientos internacionales

**→ POST** `/api/publicapi/creasys/MovimientosAlpaca/MovimientoInternacionalAlpaca`

Registra un movimiento internacional de tipo **retiro** o **aporte patrimonial** entre cuentas locales y cuentas Alpaca.

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `codTipoMovimiento` | `"RET_PAT_IT"` (retiro) o `"APO_PAT_IT"` (aporte) | Sí |
| `numCuenta` | Número de cuenta (máx 15 caracteres) | Sí |
| `monto` | Monto del movimiento (> 0) | Sí |
| `codMoneda` | Código de moneda (máx 3 caracteres) | Sí |
| `idUsuarioGpi` | ID del usuario en GPI | Sí |
| `obsMovimiento` | Observaciones (máx 100 caracteres) | No |
| `fechaMovimiento` | Fecha del movimiento (ISO 8601). Default: actual | No |
| `fechaLiquidacion` | Fecha de liquidación (ISO 8601). Default: actual | No |
| `dscMedioPagoCobro` | Medio de pago o cobro (máx 20 caracteres) | No |
| `uuid` | UUID del movimiento (máx 36 caracteres) | No |

**Tipos de movimiento:**

| Código | Descripción |
|---|---|
| `RET_PAT_IT` | Retiro patrimonial internacional |
| `APO_PAT_IT` | Aporte patrimonial internacional |

```json title="Request Body"
{
  "codTipoMovimiento": "RET_PAT_IT",
  "numCuenta": "19130340/0",
  "monto": 1000.00,
  "codMoneda": "USD",
  "idUsuarioGpi": 12345
}
```

```json title="Respuesta (201 Created)"
{
  "id": 12345,
  "codTipoMovimiento": "RET_PAT_IT",
  "numCuenta": "19130340/0",
  "obsMovimiento": "Retiro patrimonial internacional",
  "fechaMovimiento": "2025-08-06T15:10:06-04:00",
  "fechaLiquidacion": "2025-08-06T15:10:06-04:00",
  "monto": 1000.00,
  "codMoneda": "USD",
  "dscMedioPagoCobro": "Transferencia",
  "banco": "Banco Internacional",
  "numeroCuenta": "123456789",
  "tipoCuenta": "Corriente",
  "uuid": "a1b2c3d4-e5f6-g7h8-i9j0-k1l2m3n4o5p6",
  "idUsuarioGpi": 12345
}
```

<br />

## Consultar movimientos

**→ GET** `/api/publicapi/creasys/MovimientosAlpaca`

Obtiene movimientos en un rango de fecha o filtrados por ID, UUID u otros criterios.

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `Desde` | Fecha inicio (YYYY-MM-DD) | Sí (si no se usa Id o Uuid) |
| `Hasta` | Fecha fin (YYYY-MM-DD) | Sí (si no se usa Id o Uuid) |
| `Identificador` | Identificador del cliente | No |
| `NumCuenta` | Número de cuenta | No |
| `CodTipoMovimiento` | Código del tipo de movimiento | No |
| `Id` | ID del movimiento | No |
| `Uuid` | UUID del movimiento | No |
| `PageNumber` | Número de página | No |
| `PageSize` | Tamaño de página | No |

```json title="Respuesta (200 OK)"
[
  {
    "id": 12345,
    "codTipoMovimiento": "RET_PAT_IT",
    "numCuenta": "19130340/0",
    "tipoMovimiento": "Retiro Patrimonial Internacional",
    "dscMovimiento": "Retiro patrimonial internacional",
    "fechaMovimiento": "2025-08-06T15:10:06-04:00",
    "fechaLiquidacion": "2025-08-06T15:10:06-04:00",
    "monto": 1000.00,
    "codMoneda": "USD",
    "dscCajaCuenta": "Cuenta Principal",
    "dscEstadoMovimiento": "Completado"
  }
]
```

<br />

## Consultar actividad de cuenta

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/ObtenerActividadCuenta`

Obtiene las actividades de una cuenta Alpaca. Se pueden filtrar por fecha y tipo de actividad.

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |
| `startDate` | Fecha inicio (YYYY-MM-DD) | No |
| `endDate` | Fecha fin (YYYY-MM-DD) | No |
| `activityTypes` | Tipos de actividad separados por coma | No |

```json title="Respuesta (200 OK)"
{
  "value": [
    {
      "id": "20250805000000000::f817fdc3-9fcf-4b7e-9856-1a8a8bb5e932",
      "account_id": "5d2b13eb-0765-4364-8115-a65c7695303d",
      "activity_type": "JNLC",
      "description": "",
      "status": "executed",
      "date": "2025-08-05T00:00:00",
      "created_at": "2025-08-05T22:15:52.008988Z",
      "net_amount": -5,
      "symbol": null,
      "side": null,
      "qty": 0,
      "price": 0
    },
    {
      "id": "20250804000000000::72440196-d092-4674-975c-af23699c2726",
      "account_id": "5d2b13eb-0765-4364-8115-a65c7695303d",
      "activity_type": "FEE",
      "description": "TAF fee for proceed of 2 shares (1 trades) on 2025-08-04",
      "status": "executed",
      "date": "2025-08-04T00:00:00",
      "created_at": "2025-08-05T00:15:29.134855Z",
      "net_amount": -0.01,
      "symbol": null,
      "side": null,
      "qty": 0,
      "price": 0
    }
  ]
}
```

<br />

## Consultar órdenes

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/Orden/ObtenerOrden`

Obtiene las órdenes de una cuenta Alpaca.

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |
| `status` | Estado de la orden | No |
| `after` | Fecha/hora desde | No |
| `until` | Fecha/hora hasta | No |
| `direction` | `asc` o `desc` | No |
| `limit` | Cantidad máxima de registros | No |

```json title="Respuesta (200 OK)"
[
  {
    "id": "61e69015-8549-4bfd-b9c3-01e75843f47d",
    "client_order_id": "eb9e2aaa-f71a-4f51-b5b4-52a6c565dad4",
    "created_at": "2025-08-04T14:21:00.40505Z",
    "submitted_at": "2025-08-04T14:21:00.404171Z",
    "filled_at": null,
    "canceled_at": null,
    "asset_id": "b0b6dd9d-8b9b-48a9-ba46-b9d54906e415",
    "symbol": "AAPL",
    "asset_class": "us_equity",
    "qty": "1",
    "filled_qty": "0",
    "filled_avg_price": null,
    "order_type": "market",
    "side": "buy",
    "time_in_force": "day",
    "limit_price": null,
    "stop_price": null,
    "status": "accepted",
    "extended_hours": false
  }
]
```