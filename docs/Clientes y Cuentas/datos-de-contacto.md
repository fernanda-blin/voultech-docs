---
title: Datos de Contacto
excerpt: >-
  Gestiona teléfonos, direcciones y correos electrónicos asociados a personas
  registradas en el sistema.
deprecated: false
hidden: false
metadata:
  robots: index
---
Gestiona la información de contacto de las personas registradas en el sistema: teléfonos, direcciones y correos electrónicos.

## Teléfonos

**→ GET** `/api/publicapi/creasys/TelefonoPersona`

Obtiene la lista de teléfonos registrados en el sistema.

**→ POST** `/api/publicapi/creasys/TelefonoPersona`

Crea un nuevo teléfono asociado a una persona.

**→ PUT** `/api/publicapi/creasys/TelefonoPersona`

Actualiza un teléfono existente por identificador y número.

<Callout icon="⚠️" theme="warning">
  El acceso a la actualización (PUT) requiere **autorización previa** del equipo de Voultech.
</Callout>

<Callout icon="💡" theme="info">
  Consulta los tipos de teléfono disponibles (celular, fijo, laboral) con `GET /TipoDireccion/GetTipoTelefono`.
</Callout>

<br />

## Direcciones

**→ GET** `/api/publicapi/creasys/DireccionPersona`

Obtiene la lista de direcciones registradas.

**→ POST** `/api/publicapi/creasys/DireccionPersona`

Crea una nueva dirección asociada a una persona.

**→ PUT** `/api/publicapi/creasys/DireccionPersona`

Actualiza una dirección por su identificador.

<Callout icon="⚠️" theme="warning">
  El acceso a la actualización (PUT) requiere **autorización previa** del equipo de Voultech.
</Callout>

<Callout icon="💡" theme="info">
  Consulta los tipos de dirección disponibles (comercial, personal, tributaria) con `GET /TipoDireccion/GetTipoDireccion`.
</Callout>

<br />

## Correos electrónicos

**→ GET** `/api/publicapi/creasys/EmailPersona`

Obtiene la lista de correos electrónicos registrados.

**→ POST** `/api/publicapi/creasys/EmailPersona`

Crea un nuevo correo electrónico asociado a una persona.

**→ PUT** `/api/publicapi/creasys/EmailPersona`

Actualiza un correo existente por su identificador.

<Callout icon="⚠️" theme="warning">
  El acceso a la actualización (PUT) requiere **autorización previa** del equipo de Voultech.
</Callout>

<Callout icon="💡" theme="info">
  Consulta los tipos de email disponibles (personal, corporativo, principal) con `GET /TipoDireccion/GetTipoMail`.
</Callout>