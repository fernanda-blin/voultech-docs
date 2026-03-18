---
title: Gestión de Cuentas
excerpt: >-
  Crea cuentas de inversión, asocia cuentas bancarias, configura comisiones y
  gestiona cajas en múltiples divisas.
deprecated: false
hidden: false
metadata:
  robots: index
---
Crea y administra cuentas de inversión, asocia cuentas bancarias, configura comisiones y gestiona cajas en múltiples divisas.

## Crear cuenta de inversión

**→ POST** `/api/publicapi/creasys/Cuentas`

Crea una cuenta individual de inversión para un cliente existente, asociada a tu fintech como asesor.

```json title="Request Body"
{
  "numCuenta": "12345678/17",
  "dscCuenta": "Javiera Río Casanova",
  "abrCuenta": "12345678/17",
  "identificador": "12345678-K",
  "codMoneda": "CLP",
  "codTipoAdministracion": "NF",
  "dscPerfilRiesgo": "AGRESIVO",
  "dscTipoCuenta": "NACIONAL",
  "abrAsesor": "codigo_asesor"
}
```

Respuesta exitosa: **`201 Created`**

<br />

## Consultar cuentas de inversión

**→ GET** `/api/publicapi/creasys/Cuentas?Identificador={identificador}`

Retorna todas las cuentas asociadas al cliente identificado.

**Campos de la respuesta:**

| Campo | Descripción |
|---|---|
| `numCuenta` | Identificador único de cuenta |
| `dscCuenta` | Nombre completo del cliente |
| `abrCuenta` | Abreviatura, usualmente igual a `numCuenta` |
| `identificador` | RUT del cliente asociado |
| `codMoneda` | Moneda de la cuenta: `CLP`, `USD`, `EUR` |
| `codTipoAdministracion` | Tipo de administración (`NF` por defecto) |
| `dscPerfilRiesgo` | Perfil: `CONSERVADOR`, `MODERADO`, `ARRIESGADO`, `AGRESIVO`, `CALIFICADO` |
| `dscTipoCuenta` | Tipo: `NACIONAL`, `FIP`, `EXTRANJERA`, `PERSHING` |
| `abrAsesor` | Código del asesor/fintech asociado |

```json title="Respuesta"
{
  "numCuenta": "12345678/17",
  "dscCuenta": "Javiera Río Casanova",
  "abrCuenta": "12345678/17",
  "identificador": "12345678-K",
  "codMoneda": "CLP",
  "codTipoAdministracion": "NF",
  "dscPerfilRiesgo": "AGRESIVO",
  "dscTipoCuenta": "NACIONAL",
  "abrAsesor": "codigo_asesor"
}
```

<br />

## Actualizar una cuenta

**→ PUT** `/api/publicapi/creasys/Cuentas/{numCuenta}`

Actualiza la información de una cuenta existente, identificándola mediante su número de cuenta (`numCuenta`).

<Callout icon="⚠️" theme="warning">
  El acceso a este endpoint es limitado y requiere **autorización directa** del equipo de Voultech.
</Callout>

<br />

## Asociar cuenta bancaria

**→ POST** `/api/publicapi/creasys/CuentaCorriente`

Crea el vínculo entre un cliente y su cuenta bancaria. Esta cuenta se utiliza para recibir abonos y ejecutar retiros.

**Campos clave:**

| Campo | Descripción |
|---|---|
| `Identificador` | ID del cliente (RUT) |
| `NumeroCuentaCte` | Número de la cuenta bancaria |
| `CodMoneda` | Moneda: `CLP`, `USD`, `EUR` |
| `DscBanco` | Nombre del banco (ej. `BANCO BICE`) |
| `tipoCuenta` | `Cuenta Corriente`, `Cuenta Vista`, `Cuenta de Ahorro`, `Chequera`, `Electrónica`, etc. |

```json title="Request Body"
{
  "Identificador": "18737322-0",
  "NumeroCuentaCte": "11111111112",
  "CodMoneda": "CLP",
  "DscBanco": "BANCO BICE",
  "tipoCuenta": "Cuenta Corriente"
}
```

<Callout icon="💡" theme="info">
  Consulta los valores válidos de bancos con `GET /Banco` y los tipos de cuenta con `GET /TipoCuentaBanco`.
</Callout>

<br />

## Consultar cuenta bancaria

**→ GET** `/api/publicapi/creasys/CuentaCorriente?numCuenta={numCuenta}`

Devuelve la información de la cuenta bancaria asociada al número especificado.

<br />

## Crear comisión en una cuenta

**→ POST** `/api/publicapi/creasys/Comision`

Asigna una comisión específica a una cuenta existente dentro del sistema.

<br />

## Crear una caja para una cuenta

**→ POST** `/api/publicapi/creasys/Cajas`

Crea una nueva caja para un cliente en una moneda específica. Las cajas representan los fondos disponibles por divisa en una cuenta.

<Callout icon="💡" theme="info">
  Un cliente puede tener múltiples cajas en distintas monedas (CLP, USD, EUR) dentro de la misma cuenta de inversión.
</Callout>