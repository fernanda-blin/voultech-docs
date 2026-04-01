---
title: Procesar Enrolamiento
excerpt: >-
  Evalúa información de prospectos y registra de forma centralizada nuevos
  clientes y sus dependencias.
api:
  file: swagger.json
  operationId: post_api-publicapi-creasys-enrolamiento
hidden: false
---
<br />

Sample request

```json
{
  "codPerfilRiesgo": "PERFBAJ",
  "personaFulls": [
    {
      "tipoEntidad": "N",
      "identificador": "12345678-9",
      "tipoIdentificador": "R",
      "nombre": "Juan",
      "paterno": "Perez",
      "materno": "Perez",
      "sexo": "M",
      "fechaNacimiento": "1990-01-01",
      "codPais": "CHL",
      "codEstadoCivil": "SOLTERO",
      "codRegimenMatrimonial": "REGMATRIMONIAL",
      "dscGiro": "Giro",
      "codTipoSociedad": "Sociedad",
      "codTipoRelacion": "CLINTE",
      "telefonoFulls": [
        {
          "telefono": "123456789",
          "observacion": "Observacion",
          "codTipoTelefono": "TTELEFIJO"
        }
      ],
      "direccionFulls": [
        {
          "linea1": "Direccion",
          "codTipoDireccion": "TDIREFPART",
          "prospComunaCiudadId": 1
        }
      ],
      "emailFulls": [
        {
          "email": "Email",
          "codTipoEmail": "TDIREECOME"
        }
      ],
      "cuentaBancarioFulls": [
        {
          "numero": "123456789",
          "tipo": "Cuenta Corriente",
          "prospBancoId": 1
        }
      ]
    }
  ]
}
```

<br />
