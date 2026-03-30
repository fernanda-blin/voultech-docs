---
title: Actividad y Custodias
excerpt: >-
  Consulte la actividad de una cuenta Alpaca y las posiciones vigentes con su
  valorización actual.
deprecated: false
hidden: false
metadata:
  robots: index
---
Consulte la actividad operativa y las posiciones vigentes de una cuenta Alpaca para monitorear ejecuciones, cargos y valorización de instrumentos.

## Alcance de esta página

Esta guía cubre dos consultas del módulo internacional:

- **Actividad de cuenta**, para revisar eventos y cargos registrados sobre una cuenta Alpaca
- **Custodias**, para consultar las posiciones vigentes y su valorización

<br />

## Consultar actividad de cuenta

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/ObtenerActividadCuenta`

Obtiene las actividades de una cuenta Alpaca específica. Permite filtrar por rango de fechas y tipo de actividad.

### Parámetros de consulta

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |
| `startDate` | Fecha de inicio en formato `YYYY-MM-DD` | No |
| `endDate` | Fecha de término en formato `YYYY-MM-DD` | No |
| `activityTypes` | Tipos de actividad separados por coma | No |

### Respuesta

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
      "price": 0,
      "transaction_time": null,
      "type": null,
      "per_share_amount": 0
    },
    {
      "id": "20250805000000000::843288ed-cc91-49ed-8a5d-6bcd0637097d",
      "account_id": "5d2b13eb-0765-4364-8115-a65c7695303d",
      "activity_type": "JNLC",
      "description": "",
      "status": "executed",
      "date": "2025-08-05T00:00:00",
      "created_at": "2025-08-05T22:13:29.927703Z",
      "net_amount": 5,
      "symbol": null,
      "side": null,
      "qty": 0,
      "price": 0,
      "transaction_time": null,
      "type": null,
      "per_share_amount": 0
    },
    {
      "id": "20250804000000000::72440196-d092-4674-975c-af23699c2726",
      "account_id": "5d2b13eb-0765-4364-8115-a65c7695303d",
      "activity_type": "FEE",
      "description": "TAF fee for proceed of 2 shares (1 trades) on 2025-08-04 by 87022",
      "status": "executed",
      "date": "2025-08-04T00:00:00",
      "created_at": "2025-08-05T00:15:29.134855Z",
      "net_amount": -0.01,
      "symbol": null,
      "side": null,
      "qty": 0,
      "price": 0,
      "transaction_time": null,
      "type": null,
      "per_share_amount": 0
    },
    {
      "id": "20250804000000000::18a9b145-e3d7-4715-a83d-ee69a73d6d6 f",
      "account_id": "5d2b13eb-0765-4364-8115-a65c7695303d",
      "activity_type": "FEE",
      "description": "CAT fee for proceed of 1 trades on 2025-08-04 by 870221079",
      "status": "executed",
      "date": "2025-08-04T00:00:00",
      "created_at": "2025-08-05T00:05:49.796157Z",
      "net_amount": -0.01,
      "symbol": null,
      "side": null,
      "qty": 0,
      "price": 0,
      "transaction_time": null,
      "type": null,
      "per_share_amount": 0
    }
  ]
}
```

### Campos destacados de actividad

| Campo | Descripción |
|---|---|
| `activity_type` | Tipo de actividad registrada |
| `status` | Estado de la actividad |
| `date` | Fecha asociada a la actividad |
| `created_at` | Fecha y hora de creación del registro |
| `net_amount` | Monto neto asociado a la actividad |
| `symbol` | Instrumento asociado, si aplica |
| `qty` | Cantidad asociada, si aplica |
| `price` | Precio asociado, si aplica |

<br />

## Consultar custodias

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/Custodias/{accountNumber}`

Obtiene las posiciones de una cuenta Alpaca específica.

### Parámetros

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |

### Respuesta

```json title="Respuesta (200 OK)"
[
  {
    "asset_id": "b0b6dd9d-8b9b-48a9-ba46-b9d54906e415",
    "symbol": "AAPL",
    "exchange": "NASDAQ",
    "asset_class": "us_equity",
    "asset_marginable": true,
    "qty": 1,
    "avg_entry_price": 209.53,
    "side": "long",
    "market_value": 214.69,
    "cost_basis": 209.53,
    "unrealized_pl": 5.16,
    "unrealized_plpc": 0.0246,
    "unrealized_intraday_pl": 11.77,
    "unrealized_intraday_plpc": 0.058,
    "current_price": 214.69,
    "lastday_price": 202.92,
    "change_today": 0.058,
    "qty_available": 1,
    "assetName": "Apple Inc. Common Stock",
    "tradable": true,
    "marginable": true,
    "maintenanceMarginRequirement": 30
  },
  {
    "asset_id": "81f1a067-7fa4-409a-8cbd-75129da2a32c",
    "symbol": "GNLN",
    "exchange": "NASDAQ",
    "asset_class": "us_equity",
    "asset_marginable": true,
    "qty": 8,
    "avg_entry_price": 3.855,
    "side": "long",
    "market_value": 27.0304,
    "cost_basis": 30.84,
    "unrealized_pl": -3.8096,
    "unrealized_plpc": -0.1235,
    "unrealized_intraday_pl": -1.5296,
    "unrealized_intraday_plpc": -0.0535,
    "current_price": 3.3788,
    "lastday_price": 3.57,
    "change_today": -0.0535,
    "qty_available": 8,
    "assetName": "Greenlane Holdings, Inc. Class A Common Stock",
    "tradable": true,
    "marginable": true,
    "maintenanceMarginRequirement": 30
  },
  {
    "asset_id": "8ccae427-5dd0-45b3-b5fe-7ba5e422c766",
    "symbol": "TSLA",
    "exchange": "NASDAQ",
    "asset_class": "us_equity",
    "asset_marginable": true,
    "qty": 1,
    "avg_entry_price": 321.14,
    "side": "long",
    "market_value": 319.263,
    "cost_basis": 321.14,
    "unrealized_pl": -1.877,
    "unrealized_plpc": -0.0058,
    "unrealized_intraday_pl": 10.543,
    "unrealized_intraday_plpc": 0.0341,
    "current_price": 319.263,
    "lastday_price": 308.72,
    "change_today": 0.0341,
    "qty_available": 1,
    "assetName": "Tesla, Inc. Common Stock",
    "tradable": true,
    "marginable": true,
    "maintenanceMarginRequirement": 50
  }
]
```

### Campos destacados de custodias

| Campo | Descripción |
|---|---|
| `symbol` | Símbolo del instrumento |
| `exchange` | Bolsa donde se negocia |
| `qty` | Cantidad mantenida en posición |
| `avg_entry_price` | Precio promedio de entrada |
| `market_value` | Valor de mercado actual |
| `cost_basis` | Costo base de la posición |
| `unrealized_pl` | Ganancia o pérdida no realizada |
| `unrealized_plpc` | Ganancia o pérdida no realizada en porcentaje |
| `current_price` | Precio actual del instrumento |
| `lastday_price` | Precio del día anterior |
| `change_today` | Variación del día |
| `qty_available` | Cantidad disponible para operar |
| `maintenanceMarginRequirement` | Requerimiento de margen de mantenimiento |

<br />

## Flujo recomendado

1. Consulte la actividad para revisar cargos, eventos y movimientos asociados a la cuenta
2. Consulte las custodias para revisar posiciones vigentes y valorización
3. Use ambos endpoints en conjunto para mostrar historial y portafolio internacional en su interfaz

## Siguiente paso

Continúe con **Movimientos Internacionales** para registrar aportes o retiros patrimoniales entre cuentas locales y cuentas Alpaca.
