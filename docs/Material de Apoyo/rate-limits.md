---
title: Rate Limits
excerpt: >-
  Límites de paginación, restricciones horarias, uso de UUID para idempotencia y
  recomendaciones de consumo de la API.
deprecated: false
hidden: false
metadata:
  robots: index
---
Consulta los límites operativos, restricciones horarias y recomendaciones de consumo que debes considerar al integrar con la API de Voultech.

## Contenido de esta sección

<Cards columns={4}>
  <Card title="Paginación" href="#" icon="fa-table-list">
    Revisa los límites de `PageSize` y el uso recomendado de paginación.
  </Card>
  <Card title="Restricciones horarias" href="#" icon="fa-clock">
    Consulta horarios de procesamiento para retiros y eventos automáticos.
  </Card>
  <Card title="Idempotencia" href="#" icon="fa-rotate">
    Usa `Uuid` para reintentos seguros y prevención de duplicados.
  </Card>
  <Card title="Eventos vs polling" href="#" icon="fa-bell">
    Prioriza notificaciones automáticas sobre consultas frecuentes al sistema.
  </Card>
</Cards>

## Límites de paginación

<Accordion title="Ver límites de paginación" icon="fa-table-list">

| Parámetro | Límite | Descripción |
|---|---|---|
| `PageSize` | **100** registros máximo | Cantidad máxima de resultados por página en endpoints GET |
| `PageSize` (CarteraActualOptimizada) | **2.000** registros máximo | Excepción para el endpoint optimizado de cartera |

</Accordion>

<Callout icon="💡" theme="info">
  Usa siempre paginación (`PageNumber` + `PageSize`) en todos los endpoints que lo permitan para evitar respuestas truncadas o tiempos de respuesta altos.
</Callout>

**Resultado esperado:** podrás consultar colecciones grandes sin degradar tiempos de respuesta ni truncar resultados.

<br />

## Restricciones horarias

<Accordion title="Ver restricciones horarias" icon="fa-clock">

| Operación | Horario | Detalle |
|---|---|---|
| Retiros Shinkansen ≤ 5.000.000 CLP | Instantáneo | Procesados automáticamente |
| Retiros Shinkansen hasta 7.000.000 CLP | ~16:00 hrs | Requieren firma de apoderado |
| Eventos de aportes automáticos | 09:00 — 14:55 hrs | Fuera de este horario se encolan para el día siguiente |

</Accordion>

**Resultado esperado:** podrás planificar operaciones y conciliaciones según las ventanas reales de procesamiento.

<br />

## Idempotencia y reintentos

Usa el campo `Uuid` en operaciones POST para evitar duplicados si se reintenta el mismo request.

| Comportamiento | Resultado |
|---|---|
| Primer envío con `Uuid` | La operación se procesa normalmente |
| Reenvío con el mismo `Uuid` | El sistema detecta que ya fue procesada y **no la duplica** |
| `Uuid` duplicado en la misma transacción masiva | Error `ARP-012` / `SPT-012` |

**Resultado esperado:** podrás reintentar operaciones POST sin generar duplicados cuando conserves el mismo `Uuid`.

<Callout icon="⚠️" theme="warning">
  El `Uuid` debe ser **único por operación**. Genera un UUID nuevo para cada operación distinta (formato: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).
</Callout>

<br />

## Eventos vs polling

| Enfoque | Recomendación |
|---|---|
| **Polling frecuente** | ❌ No recomendado — genera carga innecesaria en el sistema |
| **Sistema de Eventos** | ✅ Recomendado — recibe actualizaciones automáticas vía Service Bus |

<Accordion title="Ver eventos notificados en tiempo real" icon="fa-bell">

El sistema de eventos notifica en tiempo real sobre:
- Movimientos bancarios (`MOVBANCO`)
- Retiros Shinkansen (`MOVSHINKANSEN`)
- Validaciones KYC (`KYCVALIDATION`)

</Accordion>

<Callout icon="💡" theme="info">
  Si necesitas consultar estados manualmente, hazlo con intervalos razonables y siempre usando paginación.
</Callout>

**Resultado esperado:** podrás reducir consultas innecesarias al sistema usando eventos como mecanismo principal de actualización.