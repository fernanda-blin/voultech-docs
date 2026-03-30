---
title: Gestión de Cuentas
excerpt: >-
  Gestiona la operativa de cuentas creando cuentas de inversión, asociando
  cuentas bancarias, configurando comisiones y habilitando cajas por moneda.
deprecated: false
hidden: false
metadata:
  robots: index
---
Gestiona la operativa de cuentas creando cuentas de inversión, asociando cuentas bancarias, configurando comisiones y habilitando cajas por moneda.

## Operaciones disponibles

<Cards columns={4}>
  <Card title="Cuentas de inversión" href="#" icon="fa-folder-open">
    Crea, consulta y actualiza cuentas asociadas a un cliente.
  </Card>
  <Card title="Cuentas bancarias" href="#" icon="fa-building-columns">
    Vincula la cuenta bancaria utilizada para abonos y retiros.
  </Card>
  <Card title="Comisiones" href="#" icon="fa-percent">
    Asigna una comisión a una cuenta existente.
  </Card>
  <Card title="Cajas por moneda" href="#" icon="fa-wallet">
    Habilita saldos separados por divisa dentro de una cuenta.
  </Card>
</Cards>

<br />

## Crear cuenta de inversión

**→ POST** `/api/publicapi/creasys/Cuentas`

Crea una cuenta individual de inversión para un cliente existente, asociada a tu fintech como asesor.

<Accordion title="Ver campos principales de creación de cuenta" icon="fa-file-lines">

- `numCuenta`: identificador único de la cuenta.
- `dscCuenta`: nombre descriptivo de la cuenta.
- `abrCuenta`: abreviatura de la cuenta.
- `identificador`: RUT del cliente.
- `codMoneda`: moneda base de la cuenta.
- `codTipoAdministracion`: tipo de administración.
- `dscPerfilRiesgo`: perfil de riesgo asociado.
- `dscTipoCuenta`: tipo de cuenta.
- `abrAsesor`: código de asesor vinculado a tu fintech.

</Accordion>

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

**Resultado esperado:** la cuenta de inversión quedará creada y asociada al cliente indicado.

<br />

## Consultar cuentas de inversión

**→ GET** `/api/publicapi/creasys/Cuentas?Identificador={identificador}`

Retorna todas las cuentas asociadas al cliente identificado.

<Accordion title="Ver campos de la respuesta" icon="fa-table">

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

</Accordion>

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

**Resultado esperado:** obtendrás el detalle de las cuentas asociadas al cliente consultado.

<br />

## Actualizar una cuenta

**→ PUT** `/api/publicapi/creasys/Cuentas/{numCuenta}`

Actualiza la información de una cuenta existente, identificándola mediante su número de cuenta (`numCuenta`).

<Callout icon="⚠️" theme="warning">
  El acceso a este endpoint es limitado y requiere **autorización directa** del equipo de Voultech.
</Callout>

**Resultado esperado:** podrás modificar datos de una cuenta existente cuando tu integración tenga este permiso habilitado.

<br />

## Asociar cuenta bancaria

**→ POST** `/api/publicapi/creasys/CuentaCorriente`

Crea el vínculo entre un cliente y su cuenta bancaria. Esta cuenta se utiliza para recibir abonos y ejecutar retiros.

<Accordion title="Ver campos clave" icon="fa-building-columns">

**Campos clave:**

| Campo | Descripción |
|---|---|
| `Identificador` | ID del cliente (RUT) |
| `NumeroCuentaCte` | Número de la cuenta bancaria |
| `CodMoneda` | Moneda: `CLP`, `USD`, `EUR` |
| `DscBanco` | Nombre del banco (ej. `BANCO BICE`) |
| `tipoCuenta` | `Cuenta Corriente`, `Cuenta Vista`, `Cuenta de Ahorro`, `Chequera`, `Electrónica`, etc. |

</Accordion>

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

**Resultado esperado:** la cuenta bancaria quedará asociada al cliente para operar abonos y retiros.

<br />

## Consultar cuenta bancaria

**→ GET** `/api/publicapi/creasys/CuentaCorriente?numCuenta={numCuenta}`

Devuelve la información de la cuenta bancaria asociada al número especificado.

**Resultado esperado:** obtendrás los datos de la cuenta bancaria vinculada a la cuenta consultada.

<br />

## Crear comisión en una cuenta

**→ POST** `/api/publicapi/creasys/Comision`

Asigna una comisión específica a una cuenta existente dentro del sistema.

**Resultado esperado:** la comisión quedará registrada para la cuenta indicada.

<br />

## Crear una caja para una cuenta

**→ POST** `/api/publicapi/creasys/Cajas`

Crea una nueva caja para un cliente en una moneda específica. Las cajas representan los fondos disponibles por divisa en una cuenta.

<Callout icon="💡" theme="info">
  Un cliente puede tener múltiples cajas en distintas monedas (CLP, USD, EUR) dentro de la misma cuenta de inversión.
</Callout>

**Resultado esperado:** la cuenta dispondrá de una caja adicional para operar en la moneda indicada.