---
title: Autenticación y Ambientes
excerpt: >-
  Configura el acceso a la API con credenciales, ambientes disponibles, Bearer
  Token JWT y gestión de sesión.
deprecated: false
hidden: false
metadata:
  robots: index
---
Configura el acceso a la API de Voultech obteniendo tus credenciales, seleccionando el ambiente correcto y gestionando tu sesión con Bearer Token (JWT).

## Antes de comenzar

1. Solicita al equipo de Voultech tus credenciales de acceso: `userName`, `password` y `abrAsesor`.
2. Confirma en qué ambiente vas a operar antes de autenticarte.
3. Utiliza el token obtenido en `SignIn` en todas las solicitudes a endpoints protegidos.

<Cards columns={3}>
  <Card title="Credenciales" href="#" icon="fa-key">
    Obtén `userName`, `password` y `abrAsesor` para acceder a la API.
  </Card>
  <Card title="Ambientes" href="#" icon="fa-server">
    Verifica si operarás en Sandbox o en Producción antes de iniciar sesión.
  </Card>
  <Card title="Sesión" href="#" icon="fa-shield-halved">
    Inicia sesión, renueva el token y consulta el estado de autenticación.
  </Card>
</Cards>

<br />

## Obtener credenciales

Contacta al equipo de Voultech (**soporte@voultech.com**) para recibir:

| Credencial | Descripción |
|---|---|
| `userName` | Usuario de acceso a la API |
| `password` | Contraseña asociada al usuario |
| `abrAsesor` | Código de asesor que identifica a tu fintech en el sistema |

**Resultado esperado:** dispondrás de los datos necesarios para autenticarte y operar sobre los recursos vinculados a tu asesor.

<br />

## Ambientes disponibles

<Accordion title="Ver ambientes disponibles" icon="fa-server">

| Entorno | URL base |
|---|---|
| **Sandbox** | `https://apiwebcbvoultechcertificacion.azurewebsites.net` |
| **Producción** | Acceso coordinado directamente con el equipo de Voultech |

</Accordion>

<Callout icon="⚠️" theme="warning">
  Las credenciales de un entorno **no funcionan** en el otro. Cada ambiente tiene sus propios usuarios, datos y configuraciones.
</Callout>

**Resultado esperado:** operarás en el ambiente correcto con las credenciales correspondientes.

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

**Resultado esperado:** obtendrás un token válido para autenticar todas las solicitudes protegidas.

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

**Resultado esperado:** extenderás la vigencia de la sesión sin volver a enviar `userName` y `password`.

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

**Resultado esperado:** obtendrás la información del usuario autenticado y la configuración disponible para la sesión actual.

<br />

## Cerrar sesión

<Accordion title="Ver opciones para cerrar sesión" icon="fa-right-from-bracket">

Tienes dos opciones para finalizar la sesión:

| Endpoint | Método | Comportamiento |
|---|---|---|
| `/api/publicapi/shared/auth/SignOut` | POST | Expira la cookie de autenticación |
| `/api/publicapi/shared/auth/Logout` | POST | Elimina la cookie de autenticación |

</Accordion>

**Resultado esperado:** la sesión quedará finalizada según el mecanismo implementado en tu integración.

<br />

## Errores de autenticación

<Accordion title="Ver errores frecuentes de autenticación" icon="fa-triangle-exclamation">

| Código HTTP | Causa | Solución |
|---|---|---|
| **401 Unauthorized** | Token faltante, vencido o inválido | Genera un nuevo token con `SignIn` |
| **403 Forbidden** | Token válido pero sin permisos para el recurso | Verifica que tu `abrAsesor` esté vinculado al cliente/cuenta que intentas acceder |

</Accordion>

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

**Resultado esperado:** podrás probar los endpoints del entorno Sandbox desde el navegador con una sesión autenticada.