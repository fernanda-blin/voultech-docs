---
title: Cuentas Internacionales
excerpt: >-
  Crea, consulta y gestiona cuentas Alpaca para operar en mercados
  internacionales.
deprecated: false
hidden: false
metadata:
  robots: index
---
Crea y gestiona cuentas Alpaca para que tus clientes operen en mercados internacionales (acciones de EE.UU.).

## Crear cuenta Alpaca

**→ POST** `/api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`

Crea una cuenta Alpaca para el cliente especificado. Utiliza el identificador del asesor autenticado.

**Parámetros:**

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
    "emailAddress": "cliente@ejemplo.com",
    "phoneNumber": "+56 9 59595959",
    "streetAddress": ["VILLA LAS ACACACIAS, PASAJE TRES No123"],
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
  "enabledAssets": ["us_equity"]
}
```

<Callout icon="💡" theme="info">
  La cuenta se crea con status `SUBMITTED`. Una vez aprobada por Alpaca, cambiará a `ACTIVE`.
</Callout>

<br />

## Obtener cuenta Alpaca

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}`

Obtiene los detalles de una cuenta Alpaca específica.

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

<br />

## Listar todas las cuentas

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

## Consultar saldo

**→ GET** `/api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}`

Obtiene el saldo de una cuenta Alpaca específica.

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `accountNumber` | Número de cuenta Alpaca | Sí |

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

**Campos de la respuesta:**

| Campo | Descripción |
|---|---|
| `cajaLocal` | Saldo en la caja local (CLP) |
| `cajaEstimadaAlpaca` | Saldo estimado en la cuenta Alpaca (USD) |
| `totalFeesDelPeriodo` | Total de comisiones del período |
| `valorInvertido` | Valor total invertido en instrumentos |
| `buyingPower` | Poder de compra disponible |
| `cashDisponible` | Efectivo disponible para operar |
| `last_Equity` | Último valor de equity de la cuenta |