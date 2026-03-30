---
title: Respuestas y Errores
excerpt: >-
  Códigos HTTP, paginación, formato de errores, catálogos de errores por módulo
  y entidades principales del sistema.
deprecated: false
hidden: false
metadata:
  robots: index
---
Interpreta las respuestas de la API identificando códigos HTTP, paginación, formatos de error, catálogos por módulo y entidades principales del sistema.

## Contenido de esta sección

<Cards columns={5}>
  <Card title="Códigos HTTP" href="#" icon="fa-code">
    Consulta el significado de los códigos de respuesta más utilizados por la API.
  </Card>
  <Card title="Paginación" href="#" icon="fa-table-list">
    Revisa cómo interpretar la cabecera `X-Pagination` y sus parámetros.
  </Card>
  <Card title="Errores" href="#" icon="fa-circle-exclamation">
    Consulta el formato estándar de errores técnicos y errores de negocio.
  </Card>
  <Card title="Catálogos por módulo" href="#" icon="fa-list-check">
    Revisa códigos de error específicos para aportes, spot, KYC y cuenta remunerada.
  </Card>
  <Card title="Entidades" href="#" icon="fa-diagram-project">
    Consulta las entidades principales utilizadas en la integración.
  </Card>
</Cards>

## Códigos de respuesta

<Accordion title="Ver códigos HTTP soportados" icon="fa-code">

| Código | Tipo | Descripción |
|---|---|---|
| **200** | Éxito | La solicitud se procesó correctamente y devuelve los datos solicitados |
| **201** | Éxito | El recurso fue creado exitosamente. Suele incluir el identificador del nuevo recurso |
| **204** | Éxito | La operación se completó correctamente, pero no hay contenido que devolver |
| **207** | Éxito parcial | Algunos elementos se procesaron correctamente y otros generaron observaciones o errores |
| **400** | Error del cliente | La solicitud contiene datos inválidos o está mal formada. Revisa los parámetros o el body |
| **401** | No autorizado | El token de autenticación falta, está vencido o es inválido. Autentícate nuevamente |
| **403** | Prohibido | El usuario está autenticado pero no tiene permisos suficientes para este recurso |
| **404** | No encontrado | El recurso solicitado no existe o no está disponible en el sistema |
| **409** | Conflicto | La operación entra en conflicto con el estado actual del recurso (ej. registro duplicado) |
| **500** | Error interno | Falla inesperada en el servidor. Intenta nuevamente más tarde o contacta soporte |
| **503** | Servicio no disponible | El servidor o un servicio dependiente no está disponible temporalmente |

</Accordion>

**Resultado esperado:** podrás interpretar si una solicitud fue procesada correctamente o identificar la categoría general del error recibido.

<br />

## Paginación

Los endpoints `GET` que devuelven múltiples resultados incluyen paginación. La respuesta incluye la cabecera **`X-Pagination`**:

```json title="Header X-Pagination"
{
  "TotalCount": 1403,
  "PageSize": 10,
  "CurrentPage": 1,
  "TotalPages": 141,
  "HasNext": true,
  "HasPrevious": false
}
```

**Parámetros de paginación:**

| Parámetro | Descripción |
|---|---|
| `pageNumber` | Número de página (desde 1) |
| `pageSize` | Cantidad de resultados por página (máximo **100**) |

<Callout icon="💡" theme="info">
  Usa siempre paginación en endpoints que lo permitan. El `pageSize` máximo es **100 registros por página**.
</Callout>

**Resultado esperado:** podrás recorrer colecciones paginadas interpretando correctamente el encabezado `X-Pagination`.

<br />

## Formato de errores

La API utiliza el formato estándar **RFC 7807 (ProblemDetails)** para las respuestas de error:

```json title="Ejemplo de error"
{
  "type": "https://tools.ietf.org/html/rfc7235#section-3.1",
  "title": "Unauthorized",
  "status": 401,
  "detail": "El token de acceso no es válido."
}
```

Para operaciones de negocio (aportes, retiros, órdenes), los errores incluyen un código específico:

```json title="Ejemplo de error de negocio"
[
  {
    "aporteRetiroObservacion": {
      "id": -1,
      "codTipoMovimiento": "APO_PAT",
      "numCuenta": "XXXXXXX/X",
      "uuid": "123-123-123"
    },
    "resultado": [
      {
        "tipoAlert": "error",
        "observacion": "Error al generar Movimiento",
        "error": {
          "code": "ARP-001",
          "description": "No se encontro la cuenta XXXXXXX/X"
        }
      }
    ]
  }
]
```

**Resultado esperado:** podrás distinguir entre errores técnicos estándar y errores de negocio con códigos específicos por operación.

<br />

## Catálogos de errores por módulo

<Callout icon="💡" theme="info">
  Utiliza estos catálogos para mapear errores funcionales y definir respuestas de negocio más precisas en tu integración.
</Callout>

<Accordion title="Aportes y Retiros (ARP)" icon="fa-duotone fa-money-bill-transfer">

| Código | Descripción |
|---|---|
| ARP-001 | No se encontró la cuenta `{numCuenta}` |
| ARP-002 | Falta ingresar NumeroCuenta de Banco |
| ARP-003 | No se encontró caja vigente `{codMoneda}` para la cuenta `{numCuenta}` |
| ARP-004 | Tipo Origen Mov Caja `{codTipoMovimiento}` no existe |
| ARP-005 | Falta ingresar TipoCuenta de Banco |
| ARP-006 | Falta ingresar banco |
| ARP-007 | Fecha Movimiento o Fecha Liquidacion NO es igual a la fecha actual |
| ARP-008 | Cuando CodMoneda es CLP el valor del Monto no puede contener decimales |
| ARP-009 | La fecha operación debe ser igual a la fecha máxima de las operaciones ingresadas |
| ARP-010 | Monto Máximo Permitido para RET_PAT_BA 7.000.000 |
| ARP-011 | Saldo disponible insuficiente para ejecutar este movimiento |
| ARP-012 | UUID duplicado en la misma transacción |
| ARP-013 | UUID ya utilizado con anterioridad en otro movimiento de caja |
| ARP-014 | La hora actual fuera de horario permitido |
| ARP-015 | Excepción del sistema |

</Accordion>

<Accordion title="Cuenta Remunerada (OCR)" icon="fa-duotone fa-piggy-bank">

| Código | Descripción |
|---|---|
| OCR-001 | No existe cuenta disponible con el número `{NumCuenta}` |
| OCR-002 | No se pudo obtener el precio para la operación |
| OCR-003 | No existe operación concepto |
| OCR-004 | No existe instrumento con nemotécnico |
| OCR-005 | No se encontró caja vigente `{monedaTransaccion}` para `{NumCuenta}` |
| OCR-006 | El UUID ya ha sido procesado previamente |
| OCR-007 | Error en la operación |

</Accordion>

<Accordion title="Operaciones Spot (SPT)" icon="fa-duotone fa-coins">

| Código | Descripción |
|---|---|
| SPT-001 | No existe cuenta disponible con el número `{numCuenta}` |
| SPT-002 | No se encontró caja vigente `{CodMonedaOperacion}` para la cuenta `{numCuenta}` |
| SPT-003 | No existe caja vigente `{CodMonedaPagoCobro}` para la cuenta `{numCuenta}` |
| SPT-004 | No se encuentra tipo de operación `{CodTipoOperacion}` para Spot |
| SPT-005 | No existe instrumento con nemotécnico `{CodMonedaOperacion}` |
| SPT-006 | No existe la contraparte `{Contraparte}` |
| SPT-007 | Tipo Origen Mov Caja Operación no encontrado |
| SPT-008 | Tipo Origen Mov Caja Pago Cobro no se encuentra |
| SPT-009 | La fecha operación debe ser igual a la fecha máxima de operaciones ingresadas |
| SPT-010 | El monto ingresado no corresponde a (P×Q) |
| SPT-011 | Saldo disponible insuficiente para ejecutar esta operación |
| SPT-012 | UUID duplicado en la misma transacción |
| SPT-013 | UUID ya utilizado en operación anterior |
| SPT-014 | Error del sistema |
| SPT-015 | Excepción del sistema |

</Accordion>

<Accordion title="Validación KYC" icon="fa-duotone fa-user-check">

| Código | Information | Descripción |
|---|---|---|
| 002 | AML High Risk | Cliente de riesgo alto en listas de vigilancia |
| 003 | Cannot get document data | No se puede extraer la información del documento de identidad |
| 004 | Missing data | Falta información del cliente (RUT, codDocumento, etc.) |
| 005 | Document rejected | Documento rechazado por proveedor |
| 006 | Code not found | No se encuentra codIdentificación |

</Accordion>

<br />

## Entidades principales

<Accordion title="Ver entidades principales del sistema" icon="fa-diagram-project">

| Entidad | Descripción |
|---|---|
| **Cliente** | Persona natural o jurídica. Usuario final según normativa CMF |
| **Cuenta** | Cuenta de inversión en una divisa, vinculada a un asesor o fintech |
| **Asesor** | Canal o motor de inversión (la fintech que se integra con Voultech) |
| **Cartera** | Portafolio de instrumentos del cliente con valorización actualizada |
| **Caja** | Fondos disponibles por cuenta y divisa |
| **Movimiento** | Flujo de dinero registrado en una caja |
| **Operación** | Movimiento asociado a nemotécnicos que componen la cartera |
| **Orden** | Instrucción de compra o venta de instrumentos financieros |
| **Asignación** | Detalle de ejecución y distribución de órdenes en el mercado |
| **MovimientosShinkansen** | Retiros vía Banco Online |
| **MovimientosBancoSecurity** | Consulta de movimientos desde Banco Security |
| **MovimientosBancoBice** | Consulta de movimientos desde Banco Bice |

</Accordion>

**Resultado esperado:** dispondrás de una referencia rápida para interpretar las entidades principales mencionadas en la API.