---
title: Guía de Inicio Rápido
excerpt: >-
  Completa tu primera integración en 5 pasos: autentícate, consulta parámetros,
  crea un cliente, abre una cuenta y registra un aporte inicial.
deprecated: false
hidden: false
metadata:
  robots: index
---
Completa tu primera integración con la API de Voultech: autentícate, consulta parámetros válidos, registra un cliente, crea su cuenta y confirma el primer aporte.

## Antes de empezar

1. Solicita al equipo de Voultech tus credenciales de Sandbox: `userName`, `password` y `abrAsesor` (código de asesor).
2. Guarda `abrAsesor`, porque lo usarás al crear el cliente y la cuenta.
3. Ejecuta los pasos en este orden para validar el flujo completo de punta a punta.

<Columns layout="auto">
  <Column>
    <Callout icon="🔐" theme="info">
      Usa estas credenciales en el entorno **Sandbox** para esta primera integración.
    </Callout>
  </Column>
  <Column>
    <Callout icon="🧭" theme="success">
      Flujo recomendado: autenticación → parámetros → cliente → cuenta → aporte → saldo.
    </Callout>
  </Column>
</Columns>

<br />

## Paso 1: Autentícate

Envía tus credenciales al endpoint `SignIn` para obtener un **Bearer Token** que incluirás en todas las llamadas posteriores.

**→ POST** `/api/publicapi/shared/auth/SignIn`

```json title="Request Body"
{
  "userName": "tu_usuario_sandbox",
  "password": "tu_contraseña_sandbox"
}
```

```json title="Respuesta exitosa"
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiration": "2025-10-21T18:00:00Z"
}
```

**Resultado:** ya puedes autenticar las llamadas siguientes con `Authorization: Bearer {tu_token}`.

Agrega el token en **todas** las solicitudes siguientes con este header:

```
Authorization: Bearer {tu_token}
```

<Callout icon="🔄" theme="info">
  Si tu token está por expirar, renuévalo con **GET** `/api/publicapi/shared/auth/RefreshToken` sin necesidad de volver a iniciar sesión.
</Callout>

<br />

## Paso 2: Consulta los parámetros del sistema

Antes de crear clientes o cuentas, necesitas los **códigos válidos** que el sistema espera. No puedes enviar `"Santiago"` como texto libre — debes enviar el `idComunaCiudad` correspondiente.

<Accordion title="Ver endpoints de parámetros recomendados" icon="fa-list">

Usa los endpoints `GET` de listados para obtener estos códigos:

| Endpoint | Uso principal | Dato clave |
|---|---|---|
| `GET /api/publicapi/creasys/Moneda` | Crear cuentas y movimientos | `codMoneda` (ej: `CLP`, `USD`) |
| `GET /api/publicapi/creasys/Comuna` | Definir dirección del cliente | `idComunaCiudad` |
| `GET /api/publicapi/creasys/Pais` | Asignar nacionalidad | `codPais` |
| `GET /api/publicapi/creasys/PerfilRiesgo` | Crear cuenta de inversión | `dscPerfilRiesgo` |
| `GET /api/publicapi/creasys/Banco` | Asociar cuentas bancarias | `dscBanco` |

</Accordion>

**Ejemplo: Obtener monedas disponibles**

**→ GET** `/api/publicapi/creasys/Moneda`

```json title="Respuesta"
[
  { "codMoneda": "CLP", "dscMoneda": "PESO CHILENO" },
  { "codMoneda": "USD", "dscMoneda": "DÓLAR USA" }
]
```

**Resultado:** ya tienes los códigos válidos que necesitas para crear el cliente, la cuenta y los movimientos.

<br />

## Paso 3: Registra tu primer cliente

Con los códigos obtenidos, registra el cliente en el sistema, carga sus documentos y crea su cuenta de inversión.

### 3.1 Crear el cliente

**→ POST** `/api/publicapi/creasys/Clientes`

```json title="Request Body"
{
  "persona": {
    "identificador": "11111111-1",
    "tipoIdentificador": "R",
    "tipoEntidad": "N",
    "nombre": "Ana",
    "paterno": "Prueba",
    "email": "ana.prueba@email.com",
    "direccionPersona": [
      {
        "direccion": "Av. Siempre Viva 123",
        "idComunaCiudadNavigation": {
          "dscComunaCiudad": "Santiago",
          "idRegionNavigation": {
            "dscRegion": "Metropolitana",
            "codPaisNavigation": {
              "dscPais": "CHILE"
            }
          }
        }
      }
    ]
  },
  "asesor": [
    { "abrNombre": "TU_CODIGO_ASESOR" }
  ]
}
```

Si la creación fue exitosa, recibirás una respuesta **`201 Created`**.

**Resultado:** el cliente queda registrado para continuar con la carga de documentos y la apertura de cuenta.

### 3.2 Subir documentos

Sube la documentación requerida (cédula, contrato) codificada en **Base64**.

**→ POST** `/api/publicapi/creasys/Documentos`

```json title="Request Body"
{
  "Identificador": "11111111-1",
  "TipoDocumento": ".pdf",
  "NombreDocumento": "TuFintech_ciFrontal",
  "ContenidoBase64": "JVBERi0xLjQKJ...",
  "codTipo": "ciFrontal"
}
```

<Accordion title="Ver tipos de documento admitidos" icon="fa-file-lines">

Tipos de documento admitidos:

| `codTipo` | Descripción |
|---|---|
| `ciFrontal` | Cédula de identidad (frente) |
| `ciReverso` | Cédula de identidad (reverso) |
| `contrato` | Contrato firmado |

</Accordion>

**Resultado esperado:** la documentación requerida quedará asociada al cliente.

### 3.3 Crear la cuenta de inversión

### 3.3 Crear la cuenta de inversión

**→ POST** `/api/publicapi/creasys/Cuentas`

```json title="Request Body"
{
  "numCuenta": "11111111/1",
  "dscCuenta": "Ana Prueba Inversiones",
  "identificador": "11111111-1",
  "codMoneda": "CLP",
  "codTipoAdministracion": "NF",
  "dscPerfilRiesgo": "MODERADO",
  "dscTipoCuenta": "NACIONAL",
  "abrAsesor": "TU_CODIGO_ASESOR"
}
```

**Resultado:** ya tienes una cuenta de inversión lista para registrar el aporte inicial.

<br />

## Paso 4: Registra un aporte inicial

Simula el primer aporte de fondos del cliente.

**→ POST** `/api/publicapi/creasys/Movimientos/IngresoAporteRetiro`

```json title="Request Body"
{
  "Uuid": "gen-un-uuid-unico-aqui",
  "CodTipoMovimiento": "APO_PAT",
  "NumCuenta": "11111111/1",
  "Monto": "100000",
  "CodMoneda": "CLP",
  "DscMedioPagoCobro": "TRANSFERENCIA",
  "ObsMovimiento": "Aporte inicial de prueba"
}
```
<Callout icon="⚠️" theme="warning">
  El `Uuid` debe ser **único por operación**. Si reintentas el mismo request con el mismo UUID, el sistema detectará que ya fue procesado y no lo duplicará.
</Callout>

**Resultado esperado:** el aporte quedará registrado una sola vez por cada `Uuid` único.

<br />
**Resultado:** el sistema procesará el aporte una sola vez por cada `Uuid` único.

<br />

## Paso 5: Verifica el saldo

Confirma que el aporte se reflejó correctamente en la cuenta del cliente.

**→ GET** `/api/publicapi/creasys/Cajas?NumCuenta=11111111/1`

```json title="Respuesta esperada"
{
  "codMoneda": "CLP",
  "saldoDisponible": 100000
}
```

Si `saldoDisponible` refleja `100000`, completaste el flujo básico de esta integración.

<br />

## Integración completada

<Callout icon="✅" theme="success">
  Completaste el flujo básico: autenticación, consulta de parámetros, creación de cliente, apertura de cuenta y registro de aporte.
</Callout>

### Próximos pasos

<Cards>
  <Card title="Ejecutar una orden de mercado" href="#" icon="fa-duotone fa-chart-line">Compra tu primer instrumento con POST /Ordenes/IngresarOrdenesMercado</Card>

  <Card title="Consultar la cartera" href="#" icon="fa-duotone fa-briefcase">Revisa el portafolio del cliente con GET /Cartera</Card>

  <Card title="Sistema de Eventos" href="#" icon="fa-duotone fa-bell">Suscríbete para monitorear movimientos en tiempo real</Card>

  <Card title="Errores comunes" href="#" icon="fa-duotone fa-circle-exclamation">Aprende a manejar los códigos de error de la API</Card>
</Cards>