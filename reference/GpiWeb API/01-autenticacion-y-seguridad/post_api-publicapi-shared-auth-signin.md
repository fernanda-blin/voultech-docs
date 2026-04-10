---
title: Iniciar sesion
excerpt: >-
  Autentica al usuario con sus credenciales (email y contrasena) y retorna un
  token JWT de acceso. Este token debe incluirse en el header `Authorization:
  Bearer {token}` en todas las peticiones posteriores. El token tiene una
  duracion limitada y debe renovarse con el endpoint RefreshToken antes de
  expirar.
api:
  file: public-api.json
  operationId: post_api-publicapi-shared-auth-signin
hidden: false
---