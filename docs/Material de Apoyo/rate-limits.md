---
title: Rate Limits
excerpt: >-
  Límites de paginación, restricciones horarias, idempotencia con UUID y
  recomendaciones de uso.
deprecated: false
hidden: false
metadata:
  robots: index
---
Límites de carga, restricciones de horario y recomendaciones para el uso óptimo de la API de Voultech.

## Límites de paginación

| Parámetro | Límite | Descripción |
|---|---|---|
| `PageSize` | **100** registros máximo | Cantidad máxima de resultados por página en endpoints GET |
| `PageSize` (CarteraActualOptimizada) | **2.000** registros máximo | Excepción para el endpoint optimizado de cartera |

<Callout icon="💡" theme="info">
  Usa siempre paginación (`PageNumber` + `PageSize`) en todos los endpoints que lo permitan para evitar respuestas truncadas o tiempos de respuesta altos.
</Callout>

<br />

## Restricciones horarias

| Operación | Horario | Detalle |
|---|---|---|
| Retiros Shinkansen ≤ 5.000.000 CLP | Instantáneo | Procesados automáticamente |
| Retiros Shinkansen hasta 7.000.000 CLP | ~16:00 hrs | Requieren firma de apoderado |
| Eventos de aportes automáticos | 09:00 — 14:55 hrs | Fuera de este horario se encolan para el día siguiente |

<br />

## Idempotencia y reintentos

Usa el campo `Uuid` en operaciones POST para evitar duplicados si se reintenta el mismo request.

| Comportamiento | Resultado |
|---|---|
| Primer envío con `Uuid` | La operación se procesa normalmente |
| Reenvío con el mismo `Uuid` | El sistema detecta que ya fue procesada y **no la duplica** |
| `Uuid` duplicado en la misma transacción masiva | Error `ARP-012` / `SPT-012` |

<Callout icon="⚠️" theme="warning">
  El `Uuid` debe ser **único por operación**. Genera un UUID nuevo para cada operación distinta (formato: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`).
</Callout>

<br />

## Eventos vs polling

| Enfoque | Recomendación |
|---|---|
| **Polling frecuente** | ❌ No recomendado — genera carga innecesaria en el sistema |
| **Sistema de Eventos** | ✅ Recomendado — recibe actualizaciones automáticas vía Service Bus |

El sistema de eventos notifica en tiempo real sobre:
- Movimientos bancarios (`MOVBANCO`)
- Retiros Shinkansen (`MOVSHINKANSEN`)
- Validaciones KYC (`KYCVALIDATION`)

<Callout icon="💡" theme="info">
  Si necesitas consultar estados manualmente, hazlo con intervalos razonables y siempre usando paginación.
</Callout>