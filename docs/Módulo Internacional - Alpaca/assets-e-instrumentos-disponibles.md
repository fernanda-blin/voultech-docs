---
title: Assets e Instrumentos Disponibles
excerpt: >-
  Busca instrumentos internacionales por símbolo, nombre o bolsa y usa
  paginación para navegar resultados.
deprecated: false
hidden: false
metadata:
  robots: index
---
Busca instrumentos disponibles para operar en bolsa internacional usando filtros por mercado, símbolo o nombre.

## Qué puedes hacer con este endpoint

Usa `GET /api/publicapi/creasys/Asset` para:

- buscar instrumentos por nombre o símbolo
- filtrar por bolsa o mercado
- consultar un instrumento específico por símbolo exacto
- paginar resultados cuando la búsqueda devuelve muchos registros

<Callout icon="💡" theme="info">
  Este endpoint es la base para construir buscadores de instrumentos antes de enviar órdenes internacionales con `CodBolsa = "ALPACA"`.
</Callout>

<br />

## Buscar assets

**→ GET** `/api/publicapi/creasys/Asset`

Servicio para obtener los instrumentos disponibles para operar en bolsa internacional, con opciones de búsqueda y filtrado por mercado, símbolo o nombre.

## Parámetros de búsqueda

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `exchangeName` | Nombre del mercado o bolsa donde se negocia el instrumento. Ejemplos: `NASDAQ`, `NYSE`. Si se omite, devuelve instrumentos de todos los mercados | No |
| `search` | Búsqueda amplia por **símbolo** o **nombre completo** del instrumento. Ejemplo: `Apple` | No |
| `id_correlativo` | Identificador correlativo interno del instrumento en el sistema | No |
| `nemo` | Búsqueda exacta por símbolo del instrumento. Ejemplo: `AAPL` | No |
| `page` | Número de página para paginación. Valor por defecto: `1` | No |
| `pageSize` | Cantidad máxima de registros por página. Valor por defecto: `20` | No |

<br />

## Cómo usar los filtros

### Búsqueda amplia

Usa `search` cuando quieras buscar por nombre o símbolo sin conocer el valor exacto.

**Ejemplo:**

```http
GET /api/publicapi/creasys/Asset?search=Apple
```

### Búsqueda exacta por símbolo

Usa `nemo` cuando ya conoces el ticker exacto.

**Ejemplo:**

```http
GET /api/publicapi/creasys/Asset?nemo=AAPL
```

### Filtro por bolsa

Usa `exchangeName` para limitar los resultados a una bolsa específica.

**Ejemplo:**

```http
GET /api/publicapi/creasys/Asset?exchangeName=NASDAQ
```

### Paginación

Usa `page` y `pageSize` para dividir resultados extensos.

**Ejemplo:**

```http
GET /api/publicapi/creasys/Asset?search=tech&page=1&pageSize=20
```

<br />

## Respuesta

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

## Campos de la respuesta

| Campo | Descripción |
|---|---|
| `asset_id` | Identificador único del instrumento |
| `symbol` | Símbolo bursátil del instrumento |
| `exchange` | Bolsa donde se negocia |
| `asset_class` | Clase de activo |
| `name` | Nombre completo del instrumento |
| `tradable` | Indica si el instrumento está disponible para operar |
| `marginable` | Indica si permite operaciones con margen |
| `totalCount` | Total de registros que coinciden con la búsqueda |

<br />

## Flujo recomendado

1. Busca instrumentos con `search` si no conoces el ticker exacto
2. Usa `nemo` para validar el símbolo final antes de operar
3. Filtra por `exchangeName` si tu interfaz separa instrumentos por mercado
4. Usa el `symbol` obtenido como `Nemotecnico` al enviar órdenes internacionales

## Siguiente paso

Continúa con **Órdenes Internacionales** para enviar una orden con `CodBolsa = "ALPACA"` usando el símbolo encontrado.