---
title: Custodias y Assets
excerpt: >-
  Consulta posiciones en cuentas Alpaca y busca instrumentos disponibles en
  mercados internacionales.
deprecated: false
hidden: false
metadata:
  robots: index
---
Consulta las posiciones (custodias) de cuentas Alpaca y busca instrumentos disponibles para operar en mercados internacionales.

## Custodias

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/Custodias/{accountNumber}`

Obtiene las posiciones actuales de una cuenta Alpaca, incluyendo valorización, ganancias/pérdidas y precio de mercado.

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |

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

**Campos de la respuesta:**

| Campo | Descripción |
|---|---|
| `symbol` | Símbolo del instrumento (ej: `AAPL`, `TSLA`) |
| `exchange` | Bolsa donde se negocia (`NASDAQ`, `NYSE`) |
| `qty` | Cantidad de títulos en posición |
| `avg_entry_price` | Precio promedio de entrada |
| `side` | Dirección de la posición (`long`) |
| `market_value` | Valor de mercado actual de la posición |
| `cost_basis` | Costo base total de la posición |
| `unrealized_pl` | Ganancia/pérdida no realizada (USD) |
| `unrealized_plpc` | Ganancia/pérdida no realizada (porcentaje) |
| `unrealized_intraday_pl` | Ganancia/pérdida intraday (USD) |
| `current_price` | Precio actual del instrumento |
| `lastday_price` | Precio de cierre del día anterior |
| `change_today` | Variación porcentual del día |
| `qty_available` | Cantidad disponible para venta |
| `maintenanceMarginRequirement` | Requerimiento de margen de mantenimiento (%) |

<br />

## Assets (Instrumentos disponibles)

**→ GET** `/api/publicapi/creasys/Asset`

Obtiene los instrumentos disponibles para operar en bolsa internacional, con opciones de búsqueda y filtrado por mercado, símbolo o nombre.

**Parámetros:**

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `exchangeName` | Bolsa donde se negocia. Ej: `"NASDAQ"`, `"NYSE"`. Si se omite, devuelve todos los mercados | No |
| `search` | Búsqueda por símbolo o nombre. Ej: `"Apple"` devolverá `AAPL` y cualquier instrumento que contenga "Apple" | No |
| `nemo` | Búsqueda exacta por símbolo. Ej: `"AAPL"` devuelve solo ese instrumento | No |
| `id_correlativo` | Identificador correlativo interno del instrumento | No |
| `page` | Número de página (default: `1`) | No |
| `pageSize` | Registros por página (default: `20`, recomendado entre 10 y 100) | No |

```json title="Respuesta (200 OK)"
{
  "items": [
    {
      "asset_id": "b0b6dd9d-8b9b-48a9-ba46-b9d54906e415",
      "symbol": "AAPL",
      "exchange": "NASDAQ",
      "asset_class": "us_equity",
      "name": "Apple Inc. Common Stock",
      "tradable": true,
      "marginable": true
    },
    {
      "asset_id": "8ccae427-5dd0-45b3-b5fe-7ba5e422c766",
      "symbol": "TSLA",
      "exchange": "NASDAQ",
      "asset_class": "us_equity",
      "name": "Tesla, Inc. Common Stock",
      "tradable": true,
      "marginable": true
    }
  ],
  "totalCount": 250
}
```

**Campos de la respuesta:**

| Campo | Descripción |
|---|---|
| `asset_id` | Identificador único del instrumento en Alpaca |
| `symbol` | Símbolo de cotización (ej: `AAPL`, `TSLA`) |
| `exchange` | Bolsa donde se negocia |
| `asset_class` | Clase de activo (`us_equity`) |
| `name` | Nombre completo del instrumento |
| `tradable` | Si el instrumento está disponible para operar |
| `marginable` | Si permite operaciones con margen |
| `totalCount` | Total de instrumentos que coinciden con la búsqueda |

<Callout icon="💡" theme="info">
  Usa `search` para búsquedas amplias (por nombre o símbolo) y `nemo` para búsquedas exactas por símbolo. Combina con `exchangeName` para filtrar por bolsa específica.
</Callout>