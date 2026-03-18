---
title: Cartolas y Reportes
excerpt: >-
  Genera cartolas simples y completas en PDF con saldos, movimientos y
  operaciones de las cuentas.
deprecated: false
hidden: false
metadata:
  robots: index
---
Genera cartolas en formato PDF con el estado de las cuentas de tus clientes, incluyendo saldos, movimientos y operaciones.

<Callout icon="🚨" theme="danger">
  La cartola de una cuenta puede generarse con fecha límite **T-1** (día anterior al actual).
</Callout>

## Parámetros comunes

| Parámetro | Tipo | Tamaño Máx. | Descripción |
|---|---|---|---|
| `numCuenta` | string | 20 | Número identificador de la cuenta cliente. Ejemplo: `9999999/9` |
| `fecha` | date | — | Fecha de cierre a consultar. Ejemplo: `2022-12-26` |
| `codMoneda` | string | 3 | Moneda en la cual obtener la cartola. Ejemplo: `CLP` |

<br />

## Cartola por cuenta

**→ GET** `/api/publicapi/creasys/Cartola/GetCartolaVoultech`

Obtiene un **resumen en formato PDF** del estado de una cuenta a una fecha específica. Equivale a una cartola simple con saldos.

```
GET /api/publicapi/creasys/Cartola/GetCartolaVoultech?NumCuenta={numCuenta}&Fecha={fecha}&CodMonedaSld={codMoneda}
```

<br />

## Cartola completa personalizada

**→ GET** `/api/publicapi/creasys/Cartola/GetCartolaCompletaVoultech`

Retorna un **documento PDF completo** que incluye:

- Todos los movimientos y operaciones del cliente
- Un **resumen mensual**
- Posibilidad de personalización con **logo y colores de la fintech**

```
GET /api/publicapi/creasys/Cartola/GetCartolaCompletaVoultech?NumCuenta={numCuenta}&Fecha={fecha}&CodMonedaSld={codMoneda}
```