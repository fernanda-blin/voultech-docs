---
title: Modificar Relación Comercial
excerpt: >-
  Actualiza la relación jerárquica o de vinculación entre un usuario y un
  cliente.
api:
  file: swagger.json
  operationId: put_api-publicapi-creasys-usuario-relacionusuariocliente
hidden: false
---
Sample request:

```http
PUT /api/publicapi/creasys/Usuario/RelacionUsuarioCliente

{
  "idUsuario": "066074xx-8734-4060-9004-757fc593d7b7",
  "identificador": "1-9",
  "ContenidoBase64": "https://storage.blob.core.windows.net/container/category/name.pdf?sig=...."
}
```
