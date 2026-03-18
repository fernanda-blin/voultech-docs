---
title: Autenticación y Ambientes
excerpt: >-
  Cómo autenticarte con Bearer Token JWT, ambientes disponibles (Sandbox y
  Producción) y gestión de sesiones.
deprecated: false
hidden: false
metadata:
  robots: index
---
La API de Voultech utiliza autenticación basada en **Bearer Token (JWT)**. Toda solicitud a endpoints protegidos requiere un token válido en la cabecera HTTP.

## Obtener credenciales

Contacta al equipo de Voultech (**soporte@voultech.com**) para recibir:

| Credencial | Descripción |
|---|---|
| `userName` | Usuario de acceso a la API |
| `password` | Contraseña asociada al usuario |
| `abrAsesor` | Código de asesor que identifica a tu fintech en el sistema |

<br />

## Ambientes disponibles

| Entorno | URL base |
|---|---|
| **Sandbox** | `https://apiwebcbvoultechcertificacion.azurewebsites.net` |
| **Producción** | Acceso coordinado directamente con el equipo de Voultech |

<Callout icon="⚠️" theme="warning">
  Las credenciales de un entorno **no funcionan** en el otro. Cada ambiente tiene sus propios usuarios, datos y configuraciones.
</Callout>

<br />

## Iniciar sesión

Envía tus credenciales para obtener un token JWT que incluirás en todas las llamadas posteriores.

**→ POST** `/api/publicapi/shared/auth/SignIn`

```json title="Request Body"
{
  "userName": "tu_usuario",
  "password": "tu_contraseña"
}
```

```json title="Respuesta exitosa"
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiration": "2025-10-21T18:00:00Z"
}
```

Incluye el token en **todas** las solicitudes protegidas:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

<Callout icon="🔒" theme="danger">
  No compartas tu token. Si sospechas que fue comprometido, genera uno nuevo con `SignIn` inmediatamente.
</Callout>

<br />

## Renovar el token

Si el token está por expirar, puedes renovarlo **sin iniciar sesión nuevamente**.

**→ GET** `/api/publicapi/shared/auth/RefreshToken`

```
Authorization: Bearer {tu_token_actual}
```

```json title="Respuesta"
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiration": "2025-10-21T20:00:00Z"
}
```

<Callout icon="💡" theme="info">
  Si la sesión ya expiró por completo, `RefreshToken` no funcionará. Deberás autenticarte nuevamente con `SignIn`.
</Callout>

<br />

## Consultar sesión actual

Obtén los datos del usuario autenticado y la configuración del sistema.

**→ GET** `/api/publicapi/shared/auth/Me`

```
Authorization: Bearer {tu_token}
```

<br />

## Cerrar sesión

Tienes dos opciones para finalizar la sesión:

| Endpoint | Método | Comportamiento |
|---|---|---|
| `/api/publicapi/shared/auth/SignOut` | POST | Expira la cookie de autenticación |
| `/api/publicapi/shared/auth/Logout` | POST | Elimina la cookie de autenticación |

<br />

## Errores de autenticación

| Código HTTP | Causa | Solución |
|---|---|---|
| **401 Unauthorized** | Token faltante, vencido o inválido | Genera un nuevo token con `SignIn` |
| **403 Forbidden** | Token válido pero sin permisos para el recurso | Verifica que tu `abrAsesor` esté vinculado al cliente/cuenta que intentas acceder |

<Callout icon="💡" theme="info">
  La seguridad se basa en la **asociación entre el token, el asesor y el recurso**. No puedes consultar datos de clientes o cuentas que no estén vinculados a tu código de asesor.
</Callout>

<br />

## Swagger interactivo

Prueba los endpoints directamente desde el navegador usando el Swagger UI del entorno Sandbox:

🔗 **[Abrir Swagger UI](https://apiwebcbvoultechcertificacion.azurewebsites.net/swagger/index.html?urls.primaryName=Public%20API)**

Para autenticarte en Swagger:
1. Ejecuta el endpoint `SignIn` y copia el token de la respuesta
2. Haz clic en el botón **Authorize** (🔒) en la parte superior
3. Ingresa: `Bearer {tu_token}`
4. Todos los endpoints quedarán autenticados para esa sesión