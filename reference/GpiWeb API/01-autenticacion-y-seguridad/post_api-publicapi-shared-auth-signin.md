---
title: Iniciar sesion
excerpt: >-
  **Autentica al usuario** con email y contraseña. Retorna un token JWT de
  acceso.


  El token debe incluirse en todas las peticiones posteriores:

  ```

  Authorization: Bearer {token}

  ```


  > El token tiene duración limitada. Usar **RefreshToken** para renovarlo antes
  de que expire.
api:
  file: public-api.json
  operationId: post_api-publicapi-shared-auth-signin
hidden: false
---