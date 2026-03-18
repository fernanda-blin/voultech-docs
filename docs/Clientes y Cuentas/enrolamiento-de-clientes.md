---
title: Enrolamiento de Clientes
excerpt: >-
  Flujo completo de onboarding: crear cliente, subir documentos de identidad y
  contratos, y gestionar datos de personas.
deprecated: false
hidden: false
metadata:
  robots: index
---
Registra clientes en la plataforma de Voultech completando el flujo de onboarding: crear el cliente, subir documentos de identidad y asociar un asesor.

## Flujo de enrolamiento

El proceso para enrolar a una persona como cliente incluye estos pasos:

1. **Crear el cliente** con sus datos personales
2. **Subir documentos requeridos**: cédula de identidad y contratos
3. **Crear una cuenta de inversión** asociada a tu fintech
4. **Asociar una cuenta bancaria** al cliente

Una vez completados estos pasos, el cliente queda habilitado para operar en el sistema.

<Callout icon="💡" theme="info">
  Es posible que el cliente ya exista en la corredora. En ese caso, igualmente debes subir sus documentos y crear su cuenta. Si la cuenta bancaria ya está asociada, no representa un problema.
</Callout>

<br />

## Crear cliente

**→ POST** `/api/publicapi/creasys/Clientes`

Crea un nuevo cliente persona natural o jurídica asociado a tu fintech.

**Campos clave:**

| Campo | Descripción |
|---|---|
| `identificador` | RUT o Pasaporte del cliente |
| `tipoIdentificador` | `"R"` (RUT) o `"P"` (Pasaporte) |
| `tipoEntidad` | `"N"` (Natural) o `"J"` (Jurídica) |
| `sexo` | `"M"` o `"F"` |
| `pep`, `fatca` | `"Si"` o `null` |
| `relacionado` | Objeto con `relacion`, `observacion`, `informacionPrivilegiada` |
| `direccionPersona` | Lista de direcciones con comuna, región y país |
| `asesor` | Abreviación de nombre de asesor (`abrNombre`) |

```json title="Request Body"
{
  "persona": {
    "identificador": "12345678-K",
    "tipoIdentificador": "R",
    "tipoEntidad": "N",
    "nombre": "Juan",
    "paterno": "Soto",
    "materno": "Perez",
    "sexo": "M",
    "fechaNacimiento": "1999-01-19",
    "profesion": "developer",
    "telefono": "99999999",
    "email": "juan@ejemplo.com",
    "direccionPersona": [
      {
        "direccion": "calle 24",
        "numero": "1",
        "adicional": "5A",
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
  "pep": "Si",
  "relacionado": {
    "relacion": "Cliente",
    "observacion": "sin observaciones",
    "informacionPrivilegiada": true
  },
  "asesor": [
    {
      "abrNombre": "nombreasesor"
    }
  ]
}
```

Respuesta exitosa: **`201 Created`**

<br />

## Consultar clientes

**→ GET** `/api/publicapi/creasys/Clientes?Identificador={identificador}`

Obtiene la información registrada de un cliente según su identificador (RUT o pasaporte).

**→ GET** `/api/publicapi/creasys/Personas`

Devuelve la lista de personas registradas en el sistema.

<Callout icon="💡" theme="info">
  Puedes verificar si un cliente ya existe antes de crearlo con `GET /Clientes?Identificador={rut}`.
</Callout>

<br />

## Cliente casado

Si el cliente está casado, debes registrar también los datos de su cónyuge en dos pasos:

### 1. Crear persona (cónyuge)

**→ POST** `/api/publicapi/creasys/Personas`

Registra a la pareja del cliente con los mismos campos de persona (`identificador`, `tipoIdentificador`, `tipoEntidad`, `nombre`, `direccionPersona`, etc.).

### 2. Relacionar las personas

**→ POST** `/api/publicapi/creasys/Contactos`

Vincula al cliente con su cónyuge.

```json title="Request Body"
{
  "identificadorCliente": "12345678-K",
  "tipoContacto": "CONYUGE",
  "identificadorContacto": "98765432-1"
}
```

<br />

## Actualizar un cliente

**→ PUT** `/api/publicapi/creasys/Clientes/{identificador}`

**→ PUT** `/api/publicapi/creasys/Personas/{identificador}`

Permite actualizar la información de un cliente o persona existente, identificándolo mediante su RUT, pasaporte u otro identificador único.

<Callout icon="⚠️" theme="warning">
  El acceso a esta funcionalidad requiere **autorización previa** del equipo de Voultech.
</Callout>

<br />

## Subir documentos

**→ POST** `/api/publicapi/creasys/Documentos`

Sube archivos en formato `.pdf` o `.jpg`, codificados en **Base64**. El nombre del archivo debe seguir la nomenclatura: `{{NombreFintech}}_tipoDocumento`.

**Campos clave:**

| Campo | Descripción |
|---|---|
| `Identificador` | ID del cliente (ej. RUT) |
| `TipoDocumento` | `.pdf` o `.jpg` |
| `NombreDocumento` | Nombre con formato requerido |
| `ContenidoBase64` | Archivo codificado a Base64 |
| `codTipo` | Tipo de documento (ver tabla abajo) |
| `Observacion` | Comentario opcional |

**Tipos de documento admitidos:**

| `codTipo` | Descripción |
|---|---|
| `ciFrontal` | Cédula de identidad (frente) |
| `ciReverso` | Cédula de identidad (reverso) |
| `contrato` | Contrato firmado |
| `mandato` | Mandato de operación |

```json title="Request Body"
{
  "Identificador": "17931004-K",
  "TipoDocumento": ".pdf",
  "NombreDocumento": "Voultech_contrato",
  "ContenidoBase64": "JVBERi0xLjQKJ...",
  "Observacion": "probando API Voultech",
  "codTipo": "ciFrontal"
}
```

<Callout icon="⚠️" theme="warning">
  El archivo Base64 debe ser **puro**, sin encabezados como `data:application/pdf;base64,`. Formatos admitidos: PDF y JPG (no PNG).
</Callout>

<br />

## Consultar documentos

**→ GET** `/api/publicapi/creasys/Documentos?Identificador={identificador}`

Devuelve los documentos asociados al cliente.

<Callout icon="💡" theme="info">
  Si envías un `codTipo` inválido, el endpoint no devuelve error explícito pero el documento **no será procesado internamente**. Usa siempre los códigos de la tabla anterior.
</Callout>