---
title: "Endpoints Alpaca — Referencia Completa"
excerpt: "Referencia técnica de todos los endpoints de Alpaca: cuenta, movimientos, órdenes y market data."
---

# 📡 Endpoints Alpaca — Referencia Completa

Usar el token obtenido en `POST /api/publicapi/shared/Auth/SignIn` en todos los endpoints de esta sección.

**Base URL QA:**
```
https://apiwebcbvoultechcertificacion.azurewebsites.net
```

---

## 🏦 Cuenta Alpaca

### GET /api/publicapi/creasys/Asset

Obtiene lista paginada de activos (assets) de Alpaca disponibles para trading.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `exchangeName` | string | No | Nombre del exchange (ej: NASDAQ, NYSE) |
| `search` | string | No | Búsqueda por símbolo o nombre del activo |
| `nemo` | string | No | Nemotécnico / símbolo del activo |
| `page` | int | No | Número de página (default: 1) |
| `pageSize` | int | No | Tamaño de página (default: 20) |

**Response 200**
```json
{
  "items": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "cusip": "037833100",
      "assetClassName": "us_equity",
      "exchangeName": "NASDAQ",
      "symbol": "AAPL",
      "name": "Apple Inc.",
      "status": "active",
      "tradable": true,
      "fractionable": true
    }
  ],
  "totalCount": 150
}
```

---

### GET /api/publicapi/creasys/Asset/LastQuote

Obtiene la última cotización disponible para un activo (precio compra/venta, tamaños, timestamp).

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `nemo` | string | **Sí** | Símbolo del activo (ej: AAPL, TSLA) |
| `feed` | string | No | Fuente de datos (default: `sip`) |

**Tipos de feed disponibles**

| Valor | Descripción |
|-------|-------------|
| `sip` | Securities Information Processor — datos consolidados en tiempo real *(Default)* |
| `iex` | Investors Exchange — datos exclusivos de IEX |
| `delayed_sip` | SIP con delay de 15 minutos |
| `boats` | Blue Ocean ATS — operaciones fuera de horario regular |
| `overnight` | Cotizaciones para operaciones overnight |
| `otc` | Over-The-Counter |

**Response 200**
```json
{
  "symbol": "AMZN",
  "lastPrice": 233,
  "bidPrice": 232.95,
  "askPrice": 233,
  "bidSize": 700,
  "askSize": 200,
  "timestamp": "2025-11-28T18:45:39.437Z",
  "feed": "delayed_sip",
  "currency": "USD"
}
```

---

### GET /api/publicapi/creasys/ClockAlpaca

Obtiene el estado actual del reloj de mercado de Alpaca.

**Response 200**
```json
{
  "timestamp": "2025-11-28T19:11:33.788Z",
  "is_open": true,
  "next_open": "2025-11-29T14:30:00Z",
  "next_close": "2025-11-28T21:00:00Z"
}
```

---

### POST /api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca

Crea una cuenta Alpaca para el cliente especificado.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `numCuenta` | string | **Sí** | Número de cuenta Voultech |
| `identificador` | string | **Sí** | RUT del cliente |
| `fundingSource` | array[string] | **Sí** | Origen de los fondos |

**Valores válidos para `fundingSource`**

| Valor | Descripción |
|-------|-------------|
| `employment_income` | Ingresos por empleo |
| `savings` | Ahorros personales |
| `investments` | Inversiones |
| `inheritance` | Herencia |
| `business_income` | Ingresos por negocios |
| `family` | Familia |

**Request**
```json
{
  "numCuenta": "12345",
  "identificador": "12345678-9",
  "fundingSource": ["employment_income"]
}
```

**Response 200**
```json
{
  "id": "f5e0a8d9-4c3e-4b1a-8f2e-1d3c4e5f6a7b",
  "account_number": "987654321",
  "status": "SUBMITTED",
  "currency": "USD",
  "created_at": "2025-11-28T18:30:00.000Z",
  "equity": "0",
  "buying_power": "0",
  "cash": "0"
}
```

---

### GET /api/publicapi/creasys/CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}

Obtiene detalles de una cuenta Alpaca.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `accountNumber` *(path)* | string | **Sí** | Número de cuenta Voultech asociada con Alpaca |

**Response 200**
```json
{
  "id": "f5e0a8d9-4c3e-4b1a-8f2e-1d3c4e5f6a7b",
  "accountNumber": "987654321",
  "status": "ACTIVE",
  "kycResults": { "summary": "pass" },
  "currency": "USD",
  "lastEquity": "10500.50",
  "identity": {
    "givenName": "Juan",
    "familyName": "Pérez",
    "dateOfBirth": "1990-01-15",
    "taxIdType": "CHL_RUT",
    "countryOfTaxResidence": "CHL",
    "fundingSource": ["employment_income"]
  },
  "numCuenta": "98765"
}
```

---

### GET /api/publicapi/creasys/CuentaAlpaca/ObtenerCuentas

Obtiene todas las cuentas Alpaca asociadas al usuario autenticado.

**Response 200**
```json
[
  {
    "idCliente": 12345,
    "numCuenta": "98765",
    "dscCuenta": "Juan Pérez - Cuenta Internacional",
    "alpacaAccountNumber": "987654321",
    "status": "ACTIVE",
    "nombreAsesor": "María González"
  }
]
```

---

### GET /api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}

Obtiene saldo completo de una cuenta Alpaca.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `accountNumber` *(path)* | string | **Sí** | Número de cuenta Voultech |

**Campos clave del Response 200**

| Campo | Descripción |
|-------|-------------|
| `equity` | Patrimonio total actualizado (posiciones + cash) |
| `last_equity` | Equity del cierre del día hábil anterior |
| `cash` | Efectivo disponible |
| `buying_power` | Poder de compra disponible |
| `portfolio_value` | Valor total del portafolio |
| `long_market_value` | Valor de mercado de posiciones largas |

```json
{
  "account_number": "870221079",
  "status": "ACTIVE",
  "currency": "USD",
  "equity": 2129.75,
  "last_equity": 2135.66,
  "cash": 113.09,
  "buying_power": 113.09,
  "portfolio_value": 2129.75,
  "long_market_value": 2016.66,
  "pattern_day_trader": false,
  "daytrade_count": 1
}
```

> ⚠️ No existe WebSocket para el equity total. Implementar **polling periódico** a este endpoint. Para saldo de caja (`cash`) sí está disponible WebSocket.

---

### GET /api/publicapi/creasys/CuentaAlpaca/ObtenerActividadCuenta

Obtiene actividades históricas de una cuenta Alpaca.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `NumCuenta` | string | **Sí** | Número de cuenta Voultech (ej: `"18784154/0"`) |
| `activity_types` | array[string] | No | Tipos: `FILL`, `JNLC`, `DIV`, `OPCSH`, etc. |
| `category` | string | No | `trade_activity` o `non_trade_activity` |
| `date` | date | No | Fecha específica (yyyy-MM-dd) |
| `after` | date | No | Fecha mínima del rango |
| `until` | date | No | Fecha máxima del rango |
| `direction` | string | No | `asc` o `desc` (default: `desc`) |
| `page_size` | int | No | 1–100 (default: 100) |
| `page_token` | string | No | Token de paginación de Alpaca |

**Response 200**
```json
[
  {
    "id": "202512010000000000::ff02076a-c012-4ebf-80b7-5665b9f17dd2",
    "account_id": "5db213eb-0765-4364-8115-a65c7695038d",
    "activity_type": "JNLC",
    "status": "executed",
    "date": "2025-12-01T00:00:00",
    "created_at": "2025-12-01T18:20:24.785Z",
    "net_amount": -5,
    "symbol": null,
    "qty": 0,
    "price": 0
  }
]
```

---

### GET /api/publicapi/creasys/CuentaAlpaca/Custodias/{accountNumber}

Obtiene posiciones (custodias) actuales de una cuenta Alpaca.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `accountNumber` *(path)* | string | **Sí** | Número de cuenta Voultech |

---

### GET /api/publicapi/creasys/MovimientosAlpaca/patrimoniales/{numCuenta}

Obtiene movimientos patrimoniales (journals) sincronizados desde Alpaca.

| Parámetro | Tipo | Obligatorio | Descripción |
|-----------|------|------------|-------------|
| `numCuenta` *(path)* | string | **Sí** | Número de cuenta Voultech |
| `fechaDesde` | date | No | Fecha inicial del rango (YYYY-MM-DD) |
| `fechaHasta` | date | No | Fecha final del rango (YYYY-MM-DD) |
| `CuentaOrigen` | string | No | UUID de cuenta origen en Alpaca |
| `CuentaDestino` | string | No | UUID de cuenta destino en Alpaca |
| `estadoActual` | string | No | Estado del journal: `executed`, `pending`, `canceled` |

**Response 200**
```json
[
  {
    "idMovimiento": 14380227,
    "fechaMovimiento": "2025-12-01T00:00:00",
    "codOrigen": "RET_PAT_IT",
    "monto": 5.0,
    "descripcion": "RETIRO PATRIMONIAL ALPACA",
    "entryType": "JNLC",
    "estadoActual": "executed",
    "numeroCuenta": "18784154/0"
  }
]
```

---

## 📋 Órdenes (Nacional e Internacional)

### POST /api/publicapi/creasys/Ordenes/IngresarOrdenesMercado

Ingresa órdenes de compra/venta para cuentas **nacionales (XSGO)** o **internacionales (ALPACA)**.

> ⚠️ La ejecución **no es inmediata**. La confirmación llega de forma asíncrona vía **Service Bus**.

| Parámetro | Obligatorio | Descripción |
|-----------|------------|-------------|
| `uuid` | **Sí** | Identificador único de la orden (idempotencia) |
| `numCuenta` | **Sí** | Número de cuenta del cliente (xxxxx/x) |
| `nemotecnico` | **Sí** | Símbolo o ticker del activo |
| `tipoSeguridad` | **Sí** | Para ALPACA usar `"CS"` |
| `tipoOperacion` | **Sí** | `C` = Compra, `V` = Venta |
| `cantidad` | **Sí** | Cantidad de títulos (mayor a 0) |
| `precio` | Condicional | Requerido si `tipoPrecio = LIMIT`, debe ser > 0 |
| `tipoPrecio` | **Sí** | `MARKET` o `LIMIT`. Con MARKET no enviar `precio` |
| `codBolsa` | **Sí** | `XSGO` = nacional, `ALPACA` = internacional |
| `tipoLiquidacion` | **Sí** | Para ALPACA usar `"T2"` |

**Request**
```json
[
  {
    "uuid": "550e8400-e29b-41d4-a716-446655440021",
    "numCuenta": "18784154/0",
    "nemotecnico": "AMZN",
    "cantidad": 1,
    "precio": 180,
    "tipoPrecio": "LIMIT",
    "tipoOperacion": "C",
    "tipoSeguridad": "CS",
    "tipoLiquidacion": "T2",
    "codBolsa": "ALPACA"
  }
]
```

**Response 200**
```json
[
  {
    "uuid": "550e8400-e29b-41d4-a716-446655440021",
    "mensaje": "Orden internacional procesada correctamente.",
    "exitoso": true,
    "uuidBolsa": null
  }
]
```

---

## 📊 Market Data — Stocks

Todos los endpoints de market data son proxies al micro de Alpaca. Requieren autenticación Bearer.

### Resumen de endpoints disponibles

| Endpoint | Descripción |
|----------|-------------|
| `GET /Asset/stocks/bars` | Barras históricas OHLCV para varios símbolos |
| `GET /Asset/stocks/bars/latest` | Última barra por símbolo (batch) |
| `GET /Asset/stocks/quotes` | Cotizaciones históricas para varios símbolos |
| `GET /Asset/stocks/quotes/latest` | Última cotización por símbolo (batch) |
| `GET /Asset/stocks/snapshots` | Snapshot actual por símbolo (batch) |
| `GET /Asset/stocks/trades` | Trades históricos para varios símbolos |
| `GET /Asset/stocks/trades/latest` | Último trade por símbolo (batch) |
| `GET /Asset/stocks/auctions` | Subastas para uno o varios símbolos |
| `GET /Asset/stocks/meta/conditions/{tickType}` | Condiciones de tick por tipo |
| `GET /Asset/stocks/meta/exchanges` | Listado de exchanges disponibles |
| `GET /Asset/stocks/{symbol}/bars` | Barras OHLCV para un símbolo en ruta |
| `GET /Asset/stocks/{symbol}/quotes` | Cotizaciones históricas para un símbolo en ruta |
| `GET /Asset/stocks/{symbol}/trades` | Trades históricos para un símbolo en ruta |
| `GET /Asset/LogoParquet/{nemotecnico}` | Logo de un activo financiero |

### Parámetros comunes de Market Data

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `symbols` | string | Tickers separados por coma. Tiene prioridad sobre `nemo` |
| `nemo` | string | Un solo ticker si no se envía `symbols` |
| `symbol` *(path)* | string | Ticker en la ruta para endpoints `/{symbol}/...` |
| `start` | string | Inicio del rango (YYYY-MM-DD o ISO 8601) |
| `end` | string | Fin del rango de fechas |
| `feed` | string | Fuente de datos: `sip`, `iex`, `delayed_sip`, etc. |
| `currency` | string | Moneda (ej: USD) |
| `limit` | int | Máximo de filas por página |
| `page_token` | string | Token de paginación devuelto por Alpaca |
| `sort` | string | Orden: `asc` o `desc` |

### GET /Asset/stocks/bars

Barras históricas OHLCV (Open, High, Low, Close, Volume).

| Parámetro adicional | Descripción |
|---------------------|-------------|
| `timeframe` | Agregación temporal: `1Min`, `15Min`, `1Hour`, `1Day` |
| `adjustment` | Ajuste corporativo: `raw`, `split`, `dividend`, `all` |

### GET /Asset/stocks/snapshots

Snapshot completo actual por símbolo: incluye última cotización, último trade, barra del día y barra del minuto previo.

### GET /Asset/stocks/meta/conditions/{tickType}

| Parámetro | Descripción |
|-----------|-------------|
| `tickType` *(path)* | Tipo de tick: `trade`, `quote`, `bar` |
| `tape` | Tape NYSE: `A`, `B`, `C` |

### GET /Asset/stocks/meta/exchanges

Retorna el listado completo de exchanges disponibles en Alpaca. Sin parámetros.

### GET /Asset/LogoParquet/{nemotecnico}

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `nemotecnico` *(path)* | string | Símbolo del activo (ej: AAPL) |

---

## ⚠️ Notas Generales

- Todos los endpoints retornan **`401 Unauthorized`** si el token expiró o es inválido.
- Todos los endpoints retornan **`403 Forbidden`** si el usuario no tiene acceso al recurso.
- Todos los endpoints retornan **`500 Server Error`** en caso de fallo interno.
- Para endpoints batch (`symbols=AAPL,MSFT`), el parámetro `symbols` tiene prioridad sobre `nemo`.
- En los endpoints `/{symbol}/...`, usar el ticker directamente en la ruta para un solo símbolo. Para múltiples, usar los endpoints batch con `symbols=`.
