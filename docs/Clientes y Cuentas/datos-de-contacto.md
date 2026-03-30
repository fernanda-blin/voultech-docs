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
Gestiona los datos de contacto de personas registradas en el sistema, incluyendo teléfonos, direcciones y correos electrónicos.

## Operaciones disponibles

<Cards columns={3}>
  <Card title="Teléfonos" href="#" icon="fa-phone">
    Consulta, registra y actualiza teléfonos asociados a una persona.
  </Card>
  <Card title="Direcciones" href="#" icon="fa-location-dot">
    Consulta, crea y actualiza direcciones registradas en el sistema.
  </Card>
  <Card title="Correos electrónicos" href="#" icon="fa-envelope">
    Consulta, crea y actualiza correos electrónicos asociados a una persona.
  </Card>
</Cards>

<br />

## Teléfonos

<Accordion title="Ver operaciones disponibles para teléfonos" icon="fa-phone">

- `GET /api/publicapi/creasys/TelefonoPersona`: obtiene la lista de teléfonos registrados.
- `POST /api/publicapi/creasys/TelefonoPersona`: crea un nuevo teléfono asociado a una persona.
- `PUT /api/publicapi/creasys/TelefonoPersona`: actualiza un teléfono existente por identificador y número.

</Accordion>

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

**Resultado esperado:** podrás consultar, registrar y mantener teléfonos asociados a una persona utilizando los tipos válidos del sistema.

<br />

## Direcciones

<Accordion title="Ver operaciones disponibles para direcciones" icon="fa-location-dot">

- `GET /api/publicapi/creasys/DireccionPersona`: obtiene la lista de direcciones registradas.
- `POST /api/publicapi/creasys/DireccionPersona`: crea una nueva dirección asociada a una persona.
- `PUT /api/publicapi/creasys/DireccionPersona`: actualiza una dirección existente por identificador.

</Accordion>

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

**Resultado esperado:** podrás consultar, registrar y actualizar direcciones usando los tipos admitidos por el sistema.

<br />

## Correos electrónicos

<Accordion title="Ver operaciones disponibles para correos electrónicos" icon="fa-envelope">

- `GET /api/publicapi/creasys/EmailPersona`: obtiene la lista de correos electrónicos registrados.
- `POST /api/publicapi/creasys/EmailPersona`: crea un nuevo correo electrónico asociado a una persona.
- `PUT /api/publicapi/creasys/EmailPersona`: actualiza un correo existente por identificador.

</Accordion>

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

**Resultado esperado:** podrás consultar, registrar y actualizar correos electrónicos con tipos válidos para la integración.