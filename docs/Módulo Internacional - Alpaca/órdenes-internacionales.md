---
title: Órdenes Internacionales
excerpt: >-
  Ingresa órdenes internacionales con CodBolsa = ALPACA y consulta el historial
  de órdenes por cuenta.
deprecated: false
hidden: false
metadata:
  robots: index
---
Ingresa y consulta órdenes de compra o venta para cuentas internacionales usando `CodBolsa = "ALPACA"`.

## Qué cubre esta página

En esta guía verás cómo:

- enviar órdenes internacionales de compra o venta
- distinguir una orden internacional de una nacional
- entender las validaciones específicas de Alpaca
- consultar órdenes ya enviadas por cuenta y estado

<br />

## Ingresar órdenes de mercado

**→ POST** `/api/publicapi/creasys/Ordenes/IngresarOrdenesMercado`

Este endpoint permite ingresar órdenes de mercado para cuentas **nacionales (`XSGO`)** o **internacionales (`ALPACA`)** según el valor de `CodBolsa`.

- Puedes enviar múltiples órdenes en una sola solicitud
- Cada orden se valida y procesa de forma independiente

## Parámetros

| Parámetro | Descripción | Obligatorio | Observaciones |
|---|---|---|---|
| `Uuid` | Identificador único de la orden | Sí | Debe ser único |
| `NumCuenta` | Número de cuenta | Sí | Debe ser válida y autorizada para el usuario |
| `Nemotecnico` | Código del instrumento | Sí | Para ALPACA no se valida contra catálogo local |
| `TipoSeguridad` | Tipo de seguridad | Sí | Para ALPACA usar `"CS"` |
| `TipoOperacion` | Tipo de operación (`C` o `V`) | Sí | `C` = Compra, `V` = Venta |
| `Cantidad` | Cantidad de títulos | Sí | Debe ser mayor a 0 |
| `Precio` | Precio unitario | Sí | Debe ser mayor a 0 |
| `CodBolsa` | Código de bolsa | Sí | `"XSGO"` para nacional, `"ALPACA"` para internacional |
| `TipoLiquidacion` | Tipo de liquidación | Sí | Para ALPACA usar `"T2"` |
| `TipoPrecio` | Tipo de precio | Sí | Ej: `"LIMIT"`, `"MARKET"` |
| `Comision` | Comisión (decimal entre 0 y 1, máx. 2 decimales) | Sí | Para ALPACA se usa `0` |

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

## Respuesta

```json title="Respuesta (200 OK)"
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

```json title="Respuesta con error (400 Bad Request)"
[
  {
    "Uuid": "550e8400-e29b-41d4-a716-446655440000",
    "Mensaje": "La cuenta Alpaca 19130340/0 no está autorizada para este usuario o no existe.",
    "Exitoso": false
  }
]
```

<Accordion title="Validaciones específicas" icon="fa-duotone fa-circle-info">

### Órdenes nacionales (`CodBolsa = "XSGO"`)

- La cuenta debe existir y estar disponible para el usuario
- El instrumento debe existir en el catálogo local
- El UUID no debe haber sido usado antes

### Órdenes internacionales (`CodBolsa = "ALPACA"`)

- La cuenta debe ser una cuenta Alpaca autorizada para el usuario
- El UUID no debe haber sido usado antes
- Se usan valores por defecto:
  - `TipoSeguridad = "CS"`
  - `TipoLiquidacion = "T2"`
  - `CodMoneda = "USD"`

### Notas adicionales

- Puedes enviar múltiples órdenes en una sola solicitud
- Cada orden se procesa por separado
- Las órdenes **nacionales** retornan `UuidBolsa`
- Las órdenes **internacionales** **no** retornan `UuidBolsa`

</Accordion>

<br />

## Consultar órdenes Alpaca

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/Orden/ObtenerOrden`

Obtiene las órdenes de una cuenta Alpaca específica.

## Parámetros de consulta

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |
| `status` | Estado de la orden | No |
| `after` | Fecha/hora desde | No |
| `until` | Fecha/hora hasta | No |
| `direction` | `asc` / `desc` | No |
| `limit` | Cantidad máxima de registros | No |

```json title="Respuesta (200 OK)"
[
  {
    "id": "61e69015-8549-4bfd-b9c3-01e75843f47d",
    "client_order_id": "eb9e2aaa-f71a-4f51-b5b4-52a6c565dad4",
    "created_at": "2025-08-04T14:21:00.40505Z",
    "updated_at": "2025-08-04T14:21:00.427453Z",
    "submitted_at": "2025-08-04T14:21:00.404171Z",
    "filled_at": null,
    "expired_at": null,
    "canceled_at": null,
    "failed_at": null,
    "replaced_at": null,
    "replaced_by": null,
    "replaces": null,
    "asset_id": "b0b6dd9d-8b9b-48a9-ba46-b9d54906e415",
    "symbol": "AAPL",
    "asset_class": "us_equity",
    "notional": null,
    "qty": "1",
    "filled_qty": "0",
    "filled_avg_price": null,
    "order_class": "",
    "order_type": "market",
    "type": "market",
    "side": "buy",
    "time_in_force": "day",
    "limit_price": null,
    "stop_price": null,
    "status": "accepted",
    "extended_hours": false,
    "legs": null,
    "trail_percent": null,
    "trail_price": null,
    "hwm": null,
    "subtag": null,
    "source": null
  }
]
```

## Campos destacados de la respuesta

| Campo | Descripción |
|---|---|
| `client_order_id` | Identificador de la orden en cliente |
| `symbol` | Instrumento operado |
| `side` | Lado de la orden (`buy` o `sell`) |
| `qty` | Cantidad solicitada |
| `filled_qty` | Cantidad ejecutada |
| `order_type` | Tipo de orden |
| `time_in_force` | Vigencia de la orden |
| `status` | Estado actual de la orden |

<br />

## Flujo recomendado

1. Busca el instrumento en **Assets e Instrumentos Disponibles**
2. Usa el símbolo encontrado como `Nemotecnico`
3. Envía la orden con `CodBolsa = "ALPACA"`
4. Guarda el `Uuid` para trazabilidad
5. Consulta la orden con `GET /CuentaAlpaca/Orden/ObtenerOrden`

## Siguiente paso

Continúa con **Actividad y Custodias** para revisar posiciones abiertas, actividad ejecutada y valorización de la cuenta.