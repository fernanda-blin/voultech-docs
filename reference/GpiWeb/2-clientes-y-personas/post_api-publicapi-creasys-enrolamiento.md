---
title: Procesar Enrolamiento
excerpt: "Sample request:\r\n            \r\n    POST /api/publicapi/creasys/Enrolamiento/FinalizarEnrolamiento\r\n    {\r\n        \"codPerfilRiesgo\": \"PERFBAJ\",\r\n        \"personaFulls\": [\r\n        {\r\n            \"tipoEntidad\": \"N\",\r\n            \"identificador\": \"12345678-9\",\r\n            \"tipoIdentificador\": \"R\",\r\n            \"nombre\": \"Juan\",\r\n            \"paterno\": \"Perez\",\r\n            \"materno\": \"Perez\",\r\n            \"sexo\": \"M\",\r\n            \"fechaNacimiento\": \"1990-01-01\",\r\n            \"codPais\": \"CHL\",\r\n            \"codEstadoCivil\": \"SOLTERO\",\r\n            \"codRegimenMatrimonial\": \"REGMATRIMONIAL\",\r\n            \"dscGiro\": \"Giro\",\r\n            \"codTipoSociedad\": \"Sociedad\",\r\n            \"codTipoRelacion\": \"CLINTE\",\r\n            \"telefonoFulls\": [\r\n            {\r\n                \"telefono\": \"123456789\",\r\n                \"observacion\": \"Observacion\",\r\n                \"codTipoTelefono\": \"TTELEFIJO\"\r\n            }\r\n            ],\r\n            \"direccionFulls\": [\r\n            {\r\n                \"linea1\": \"Direccion\",\r\n                \"codTipoDireccion\": \"TDIREFPART\",\r\n                \"prospComunaCiudadId\": 1\r\n            }\r\n            ],\r\n            \"emailFulls\": [\r\n            {\r\n                \"email\": \"Email\",\r\n                \"codTipoEmail\": \"TDIREECOME\"\r\n            }\r\n            ],\r\n            \"cuentaBancarioFulls\": [\r\n            {\r\n                \"numero\": \"123456789\",\r\n                \"tipo\": \"Cuenta Corriente\",\r\n                \"prospBancoId\": 1\r\n            }\r\n            ]\r\n        }\r\n        ]\r\n    }"
api:
  file: swagger.json
  operationId: post_api-publicapi-creasys-enrolamiento
hidden: false
---
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