---
title: Cuentas Internacionales
excerpt: >-
  Crea cuentas Alpaca, consulta su detalle, lista las cuentas del asesor y
  revisa saldos.
deprecated: false
hidden: false
metadata:
  robots: index
---
Crea y gestiona cuentas Alpaca para que tus clientes operen en mercados internacionales.

## Qué cubre esta página

En esta guía verás cómo:

- crear una cuenta Alpaca asociada a una cuenta local
- consultar el detalle de una cuenta internacional
- listar las cuentas Alpaca del asesor autenticado
- revisar el saldo disponible de una cuenta

<Callout icon="💡" theme="info">
  En este módulo conviven dos identificadores: `numCuenta` (cuenta local en GPI) y `accountNumber` (cuenta en Alpaca).
</Callout>

<br />

## Crear cuenta Alpaca

**→ POST** `/api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`

Crea una cuenta Alpaca para el cliente especificado usando el asesor autenticado.

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `numCuenta` | Número de cuenta a la cual asociar la nueva cuenta internacional | Sí |
| `identificador` | Identificador único del cliente en GPI | Sí |

```json title="Request Body"
{
  "numCuenta": "19130340/0",
  "identificador": "19130340-7"
}
```

```json title="Respuesta (200 OK)"
{
  "id": "b1fd6038-6b34-4fdd-8164-1c2162f678e7",
  "accountNumber": "901988645",
  "status": "SUBMITTED",
  "currency": "USD",
  "createdAt": "2025-08-04T16:03:34.43686Z",
  "lastEquity": "0",
  "cryptoStatus": "INACTIVE",
  "contact": {
    "emailAddress": "a01o0elbartoinc333+asu@gmail.com",
    "phoneNumber": "+56 9 59595959",
    "streetAddress": [
      "VILLA LAS ACACACIAS, PASAJE TRES No123"
    ],
    "city": "SAN FELIPE",
    "state": "RM",
    "postalCode": "0000000",
    "country": "CHL"
  },
  "identity": {
    "givenName": "SERGIO",
    "familyName": "CONSTERLA DIAZ",
    "dateOfBirth": "1995-09-03",
    "taxId": null,
    "taxIdType": "CHL_RUT",
    "countryOfCitizenship": "CHL",
    "countryOfBirth": "CHL",
    "countryOfTaxResidence": "CHL",
    "fundingSource": []
  },
  "disclosures": {
    "isControlPerson": true,
    "isAffiliatedExchangeOrFinra": true,
    "isPoliticallyExposed": false,
    "immediateFamilyExposed": false
  },
  "agreements": [
    {
      "agreement": "account_agreement",
      "signedAt": "2025-08-04T16:03:32Z",
      "ipAddress": "::1"
    }
  ],
  "enabledAssets": [
    "us_equity"
  ]
}
```

<Callout icon="📘" theme="info">
  La respuesta entrega el `accountNumber` de Alpaca, que necesitarás para consultar saldo, custodias, actividad y órdenes.
</Callout>

<br />

## Obtener una cuenta Alpaca

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}`

Obtiene el detalle de una cuenta Alpaca específica.

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca (URI encoded) | Sí |

```json title="Respuesta (200 OK)"
{
  "account_id": "6f1d3a2b-3c4d-4b8f-9a1e-112233445566",
  "account_number": "19837710/0",
  "status": "ACTIVE",
  "currency": "USD",
  "cash": "2500.00",
  "portfolio_value": "2575.25",
  "pattern_day_trader": false,
  "trading_blocked": false,
  "idCliente": 14521,
  "numCuenta": "12345678"
}
```

**Campos destacados:**

| Campo | Descripción |
|---|---|
| `status` | Estado actual de la cuenta Alpaca |
| `currency` | Moneda de operación de la cuenta |
| `cash` | Efectivo disponible en la cuenta |
| `portfolio_value` | Valor total del portafolio |
| `pattern_day_trader` | Indica si la cuenta está marcada como day trader |
| `trading_blocked` | Indica si el trading está bloqueado |
| `numCuenta` | Relación con la cuenta local en GPI |

<br />

## Obtener todas las cuentas Alpaca

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/ObtenerCuentas`

Obtiene todas las cuentas Alpaca asociadas al asesor autenticado.

```json title="Respuesta (200 OK)"
[
  {
    "account_id": "6f1d3a2b-3c4d-4b8f-9a1e-112233445566",
    "account_number": "19837710/0",
    "status": "ACTIVE",
    "currency": "USD"
  },
  {
    "account_id": "4a2f9b10-8c39-4b8e-8b2e-778899001122",
    "account_number": "19837710/1",
    "status": "SUBMITTED",
    "currency": "USD"
  }
]
```

<br />

## Obtener saldo de una cuenta Alpaca

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}`

Obtiene el saldo de una cuenta Alpaca específica.

```json title="Respuesta (200 OK)"
{
  "cajaLocal": 150,
  "cajaEstimadaAlpaca": 729,
  "totalFeesDelPeriodo": -0.08,
  "valorInvertido": 0,
  "buyingPower": 710.04,
  "cashDisponible": 728.92,
  "last_Equity": 710.12
}
```

**Campos destacados:**

| Campo | Descripción |
|---|---|
| `cajaLocal` | Caja local asociada |
| `cajaEstimadaAlpaca` | Caja estimada en Alpaca |
| `totalFeesDelPeriodo` | Comisiones acumuladas del período |
| `valorInvertido` | Monto invertido |
| `buyingPower` | Poder de compra disponible |
| `cashDisponible` | Efectivo disponible para operar |
| `last_Equity` | Último equity registrado |

<br />

## Flujo recomendado

1. Crea la cuenta con `POST /CuentaAlpaca/CrearClienteAlpaca`
2. Guarda el `accountNumber` retornado
3. Consulta el detalle con `GET /CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}`
4. Verifica el saldo con `GET /CuentaAlpaca/SaldoAlpaca/{accountNumber}`
5. Usa ese `accountNumber` en las páginas de actividad, custodias y órdenes

## Siguiente paso

Continúa con **Assets e Instrumentos** para buscar símbolos disponibles, o con **Órdenes Internacionales** para enviar tu primera orden.