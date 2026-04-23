---
title: "Cuentas Internacionales"
excerpt: "Crea cuentas Alpaca, consulta su detalle, lista las cuentas del asesor y revisa saldos."
---

Crea y gestiona cuentas Alpaca para que tus clientes operen en mercados internacionales.

## Qué cubre esta página

En esta guía verás cómo:

- crear una cuenta Alpaca asociada a una cuenta local
- consultar el detalle de una cuenta internacional
- listar las cuentas Alpaca del asesor autenticado
- revisar el saldo disponible de una cuenta

> 📌 En este módulo conviven dos identificadores: `numCuenta` (cuenta local en GPI) y `accountNumber` (cuenta en Alpaca).

---

## Tipos de Fintech: ¿Qué pasos necesitas?

Antes de crear una cuenta Alpaca, identifica el tipo de fintech para saber cuántos pasos ejecutar:

| Tipo | Descripción | Pasos necesarios |
|------|-------------|-----------------|
| **Fintech con mercado local** | Ya opera con Voultech en bolsa chilena. El cliente ya existe en el sistema. | Solo **Paso 3**: `POST /CrearClienteAlpaca` |
| **Fintech solo Alpaca** | Nueva o extranjera. El cliente no existe en Voultech. | **3 pasos en secuencia** |

> ⚠️ Para fintechs **solo Alpaca**, los Pasos 1 y 2 son obligatorios antes de llamar a `CrearClienteAlpaca`.

---

## Flujo completo (Fintech solo Alpaca)

Si el cliente no existe en Voultech, ejecuta estos 3 pasos en orden:

```
Paso 1               Paso 2              Paso 3
POST /Clientes  →  POST /Cuentas  →  POST /CrearClienteAlpaca
 (crea cliente)    (crea cuenta)       (activa en Alpaca)
```

### Paso 1 — Crear cliente en Voultech

`POST /api/publicapi/creasys/Clientes`

```json
{
  "persona": {
    "identificador": "11111111-3",
    "tipoIdentificador": "P",
    "tipoEntidad": "N",
    "nombre": "Tomás",
    "paterno": "Prueba",
    "email": "tomas.prueba@fintech.com",
    "celular": "+59891415499",
    "fechaNacimiento": "2000-11-29",
    "empleador": "Empresa",
    "cargo": "Desarrollador",
    "dscEstadoCivil": "CASADO",
    "dscPais": "URUGUAY",
    "direccionPersona": [
      {
        "direccion": "Av. 18 de Julio",
        "numero": "1234",
        "idComunaCiudadNavigation": {
          "dscComunaCiudad": "URUGUAY"
        }
      }
    ]
  },
  "codIdentificacion": "codigo-rillis-del-usuario",
  "asesor": [{ "abrNombre": "abreviatura-fintech" }]
}
```

> 🚨 `codIdentificacion` es el código KYC del proveedor (Rillis o Identyz). Es **obligatorio en producción**. Su ausencia hará fallar las activaciones aunque todo funcione en QA.

### Paso 2 — Crear cuenta en Voultech

`POST /api/publicapi/creasys/Cuentas`

```json
{
  "numCuenta": "123456789/0",
  "dscCuenta": "Cuenta Alpaca USD",
  "identificador": "11111111-3",
  "codMoneda": "USD",
  "codTipoAdministracion": "NF",
  "dscPerfilRiesgo": "Moderado",
  "dscTipoCuenta": "EXTRANJERA",
  "abrAsesor": "abreviatura-fintech"
}
```

### Paso 3 — Crear cuenta en Alpaca

`POST /api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`

```json
{
  "numCuenta": "123456789/0",
  "identificador": "11111111-3",
  "fundingSource": ["employment_income"]
}
```

**Valores válidos para `fundingSource`:**

| Valor | Descripción |
|-------|-------------|
| `employment_income` | Ingresos laborales |
| `savings` | Ahorros |
| `investments` | Inversiones |
| `inheritance` | Herencia |
| `business_income` | Ingresos de negocio |
| `family` | Fondos familiares |

**Respuesta exitosa (200):**

```json
{
  "id": "b1fd6038-6b34-4fdd-8164-1c2162f678e7",
  "accountNumber": "901988645",
  "status": "SUBMITTED",
  "currency": "USD",
  "createdAt": "2025-08-04T16:03:34.43686Z",
  "lastEquity": "0",
  "cryptoStatus": "INACTIVE",
  "contact": {
    "emailAddress": "tomas.prueba@fintech.com",
    "phoneNumber": "+56 9 59595959",
    "streetAddress": ["VILLA LAS ACACIAS, PASAJE TRES No123"],
    "city": "SAN FELIPE",
    "state": "RM",
    "postalCode": "0000000",
    "country": "CHL"
  },
  "identity": {
    "givenName": "SERGIO",
    "familyName": "CONSTERLA DIAZ",
    "dateOfBirth": "1995-09-03",
    "taxIdType": "CHL_RUT",
    "countryOfCitizenship": "CHL",
    "countryOfBirth": "CHL",
    "countryOfTaxResidence": "CHL",
    "fundingSource": ["employment_income"]
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

> 📌 La respuesta entrega el `accountNumber` de Alpaca, que necesitarás para consultar saldo, custodias, actividad y órdenes.

---

## KYC: Validación de identidad

Alpaca exige cumplimiento del **CIP (Customer Identification Program)** para todos los usuarios en producción.

| Proveedor | Descripción |
|-----------|-------------|
| **Rillis** | Proveedor principal integrado. *(Recomendado para nuevas integraciones)* |
| **Identyz** | Proveedor alternativo. *(Para quienes ya lo tienen integrado)* |

Cuando un usuario completa el flujo de validación en Rillis, el webhook entrega:

```json
{
  "type": "aprobado",
  "id": "<id_flujo_rillis>"
}
```

Usa el campo `id` como `codIdentificacion` al crear el cliente. **No uses** `data.flow_instance_id`.

> ⚠️ En QA, Alpaca **no solicita** validación KYC. Es esperado y no impide abrir cuentas de prueba.

---

## Consultar una cuenta Alpaca

`GET /api/publicapi/creasys/CuentaAlpaca/ObtenerCuentaAlpaca/{accountNumber}`

Obtiene el detalle de una cuenta Alpaca específica.

| Parámetro | Descripción | Obligatorio |
|-----------|-------------|------------|
| `accountNumber` | Número de cuenta Alpaca (URI encoded) | Sí |

**Respuesta (200):**

```json
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

| Campo | Descripción |
|-------|------------|
| `status` | Estado actual de la cuenta Alpaca |
| `cash` | Efectivo disponible |
| `portfolio_value` | Valor total del portafolio |
| `pattern_day_trader` | Indica si la cuenta está marcada como day trader |
| `trading_blocked` | Indica si el trading está bloqueado |
| `numCuenta` | Relación con la cuenta local en GPI |

---

## Listar todas las cuentas Alpaca

`GET /api/publicapi/creasys/CuentaAlpaca/ObtenerCuentas`

Obtiene todas las cuentas Alpaca asociadas al asesor autenticado.

**Respuesta (200):**

```json
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

---

## Consultar saldo de una cuenta Alpaca

`GET /api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}`

Obtiene el saldo de una cuenta Alpaca específica.

**Respuesta (200):**

```json
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

| Campo | Descripción |
|-------|------------|
| `cajaLocal` | Caja local asociada |
| `cajaEstimadaAlpaca` | Caja estimada en Alpaca |
| `totalFeesDelPeriodo` | Comisiones acumuladas del período |
| `valorInvertido` | Monto invertido |
| `buyingPower` | Poder de compra disponible |
| `cashDisponible` | Efectivo disponible para operar |
| `last_Equity` | Último equity registrado |

---

## Flujo recomendado

1. Si es fintech **solo Alpaca**: ejecuta Paso 1 → Paso 2 → Paso 3
2. Si el cliente ya existe en Voultech: ejecuta solo el Paso 3 (`POST /CrearClienteAlpaca`)
3. Guarda el `accountNumber` retornado
4. Consulta el detalle con `GET /ObtenerCuentaAlpaca/{accountNumber}`
5. Verifica el saldo con `GET /SaldoAlpaca/{accountNumber}`
6. Usa ese `accountNumber` en actividad, custodias y órdenes

> 📚 Para el flujo completo de onboarding, fondeo y trading, consulta la **[Guía de Integración con Alpaca Markets](/docs/guia-integracion-alpaca)**.

---

Continúa con **Assets e Instrumentos** para buscar símbolos disponibles, o con **Órdenes Internacionales** para enviar tu primera orden.
