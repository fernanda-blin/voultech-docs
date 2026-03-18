---
title: FAQ
excerpt: >-
  Respuestas a las preguntas más comunes sobre autenticación, clientes,
  operaciones, eventos y testing.
deprecated: false
hidden: false
metadata:
  robots: index
---
Respuestas a las preguntas más comunes sobre la integración con la API de Voultech.

## Autenticación y acceso

<Accordion title="¿Qué necesito para comenzar a integrarme con la API?" icon="fa-duotone fa-key">

Debes tener:

- Un código de asesor (clave que identifica a tu fintech)
- Credenciales para ambiente de certificación (`userName` y `password`)
- Un flujo definido para onboarding de clientes y movimientos financieros

Todo lo anterior se coordina directamente con el equipo de Voultech.

</Accordion>

<Accordion title="¿El token de autenticación tiene vencimiento? ¿Cómo lo renuevo?" icon="fa-duotone fa-clock">

Sí, los tokens expiran. Puedes renovarlo con:

```
GET /api/publicapi/shared/auth/RefreshToken
```

Si la sesión ya expiró, deberás usar nuevamente el endpoint de `SignIn`.

</Accordion>

<Accordion title="¿Por qué me llega un error 403 aunque tengo token?" icon="fa-duotone fa-lock">

Puede deberse a:

- Tu token es válido, pero estás intentando acceder a un recurso no autorizado para tu asesor
- El código de asesor no está correctamente vinculado al cliente o cuenta

Valida con el equipo de Voultech que la configuración del asesor esté correcta.

</Accordion>

<Accordion title="¿Qué pasa si envío un token válido pero con un identificador de otro asesor?" icon="fa-duotone fa-shield">

Recibirás un error **403**, aunque estés autenticado correctamente. La seguridad está basada no solo en el token, sino en la **asociación entre el token, el asesor y el recurso** (cliente/cuenta). No puedes ver datos de clientes que no estén vinculados a tu asesor.

</Accordion>

<br />

## Clientes y cuentas

<Accordion title="¿Puedo crear múltiples cuentas para un mismo cliente?" icon="fa-duotone fa-users">

Sí. Cada fintech puede administrar varias cuentas por cliente (por ejemplo, para diferentes objetivos de inversión), siempre y cuando estén asociadas al mismo identificador (RUT/pasaporte) y código de asesor.

</Accordion>

<Accordion title="¿Qué pasa si intento crear un cliente que ya existe?" icon="fa-duotone fa-user-plus">

El sistema no lo bloqueará, pero **deberás volver a subir los documentos y crear su cuenta** de todas formas. Puedes validar existencia con:

```
GET /Clientes?Identificador={rut}
```

</Accordion>

<Accordion title="¿Qué pasa si intento crear una cuenta con el mismo numCuenta que otra ya existente?" icon="fa-duotone fa-copy">

El campo `numCuenta` **debe ser único por fintech**. Si repites uno ya existente, puedes recibir un error 409 (conflict), o una sobreescritura silenciosa si los datos coinciden parcialmente. Se recomienda concatenar el identificador con una secuencia: `rut/secuencia`.

</Accordion>

<Accordion title="¿Puedo usar un RUT con guion o dígito verificador en identificador?" icon="fa-duotone fa-id-card">

Sí, pero el formato **debe ser consistente**. Algunos endpoints aceptan `12345678-K`, otros solo `12345678`. Se recomienda validar previamente el formato correcto con un `GET /Clientes` antes de hacer un `POST`.

</Accordion>

<Accordion title="¿Puedo hacer PATCH para actualizar clientes o cuentas?" icon="fa-duotone fa-pen">

Actualmente no existe soporte para `PATCH` o actualización parcial. Si necesitas cambiar información, debes usar `PUT` con el objeto completo o consultar al equipo técnico por un flujo de actualización permitido.

</Accordion>

<br />

## Documentos

<Accordion title="¿Qué pasa si intento inyectar un documento con codTipo incorrecto?" icon="fa-duotone fa-file-circle-xmark">

El endpoint `POST /Documentos` **no devuelve error explícito** si el `codTipo` es inválido, pero el documento **no será procesado internamente**. Siempre usa nomenclaturas conocidas: `ciFrontal`, `ciReverso`, `contrato`, `mandato`. En caso de duda, confirma con el equipo de soporte.

</Accordion>

<Accordion title="¿Puedo enviar múltiples documentos del mismo tipo?" icon="fa-duotone fa-files">

Sí, pero solo el último será considerado como válido por defecto, a menos que tengas un flujo que procese versiones. No hay un control de versiones nativo en el endpoint.

</Accordion>

<Accordion title="¿En qué formato debe ir el documento base64?" icon="fa-duotone fa-file-code">

Para subir un documento vía `POST /Documentos`, el archivo debe:

- Estar **completo** (PDF o JPG, no PNG)
- Codificado en base64 puro, **sin encabezados** (`data:application/pdf;base64,`)
- Tener nombre con prefijo de la fintech, ej. `Voultech_ciFrontal`

</Accordion>

<br />

## Operaciones y movimientos

<Accordion title="¿Puedo hacer transferencias a terceros desde la API?" icon="fa-duotone fa-ban">

**No.** Todos los retiros deben ir a cuentas bancarias del mismo cliente. No se permite transferir fondos a terceros desde las cajas.

</Accordion>

<Accordion title="¿Puedo reintentar una operación sin crear duplicados?" icon="fa-duotone fa-rotate">

Sí. Los endpoints como `POST /Movimientos/IngresoAporteRetiroMasivo` aceptan el parámetro `Uuid` para **idempotencia**. Si reintentas una operación con el mismo UUID, el sistema detectará que ya fue procesada y no la duplicará.

</Accordion>

<Accordion title="¿Qué pasa si envío una fecha mal formateada?" icon="fa-duotone fa-calendar-xmark">

Las fechas deben estar en formato **ISO 8601 completo con hora** (ej. `2025-10-06T00:00:00`). Si omites la hora o usas solo la fecha (`YYYY-MM-DD`), **el backend puede interpretar mal los resultados** o devolver un 500 sin explicación clara.

</Accordion>

<Accordion title="¿Cómo se interpretan los saldos?" icon="fa-duotone fa-scale-balanced">

Los saldos de caja consultados por API no incluyen operaciones **en proceso de liquidación**. Si estás operando justo en la apertura/cierre de mercado, podrías ver desfases. Usa `GET /shared/Cierre/DatosEnProceso` para detectar si los datos están aún en movimiento.

</Accordion>

<Accordion title="¿Cómo obtener el precio del dólar?" icon="fa-duotone fa-dollar-sign">

Puedes:

- Suscribirte al WebSocket de precios
- Consultar directamente vía `GET /v2/quotes/last` (requiere autenticación en la API FX)

</Accordion>

<Accordion title="¿Qué pasa si envío un número de cuenta sin formato?" icon="fa-duotone fa-hashtag">

Campos como `NumCuenta`, `NumeroCuentaCte` o `numCuenta` deben seguir el formato interno (por ejemplo: `12345678/17` sin guion del RUT). Formatos incorrectos pueden causar errores sin código específico. Valida con el equipo de Voultech el formato esperado.

</Accordion>

<Accordion title="¿Puedo asociar cuentas bancarias repetidas a diferentes clientes?" icon="fa-duotone fa-building-columns">

Técnicamente sí, el sistema lo permite, pero **no es recomendable**. Cada cuenta debe pertenecer al mismo cliente que el identificador que estás usando, de lo contrario puedes recibir errores silenciosos o inconsistencias en los movimientos.

</Accordion>

<br />

## Eventos y monitoreo

<Accordion title="¿Cómo pruebo el sistema de eventos sin tráfico real?" icon="fa-duotone fa-flask">

El sistema de eventos no permite simular desde API directamente. La única forma es:

1. Inyectar datos reales o ficticios (ej. abonos con `POST /Movimientos/...`)
2. Estar suscrito al Service Bus para recibir los eventos

Si no llegan mensajes, valida que estés usando el código de asesor correcto y que tu listener esté habilitado.

</Accordion>

<Accordion title="¿Puedo obtener el estado de un evento anterior?" icon="fa-duotone fa-clock-rotate-left">

No. El sistema de eventos está diseñado para notificaciones **en tiempo real** (pub/sub). Si pierdes el mensaje, deberás consultar el estado manualmente vía API (por ejemplo: consultar saldo o estado KYC directamente).

</Accordion>

<Accordion title="¿Hay forma de saber si los datos están aún en proceso?" icon="fa-duotone fa-spinner">

Sí. Usa este endpoint:

```
GET /api/publicapi/shared/Cierre/DatosEnProceso
```

Te indica si hay datos que aún se están actualizando, como saldos o movimientos aún no consolidados.

</Accordion>

<br />

## Testing y validación

<Accordion title="¿Cómo valido que mi integración está funcionando correctamente?" icon="fa-duotone fa-circle-check">

Utiliza el entorno de certificación (`apiwebcbvoultechcertificacion.azurewebsites.net`) para probar onboarding, movimientos, compra/venta y eventos. Puedes monitorear abonos o estados de KYC usando eventos, sin necesidad de hacer polling constante.

</Accordion>

<Accordion title="¿Cómo probar abonos o eventos sin operaciones reales?" icon="fa-duotone fa-vial">

Puedes simular abonos enviando un `POST /Movimientos/IngresoAporteRetiroMasivo` y luego recibirás el evento correspondiente, lo cual te permite testear el flujo de caja y eventos sin esperar una transferencia bancaria real.

</Accordion>

<Accordion title="¿Existe un límite de registros por página?" icon="fa-duotone fa-list-ol">

Sí. El `PageSize` máximo permitido en los endpoints de consulta es **100**. Usa `PageNumber` para paginar resultados y evitar respuestas truncadas.

</Accordion>