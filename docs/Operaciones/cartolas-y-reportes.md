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
Genera cartolas en formato PDF para consultar el estado de cuentas de tus clientes, incluyendo saldos, movimientos y operaciones.

## Reportes disponibles

<Cards columns={2}>
  <Card title="Cartola por cuenta" href="#" icon="fa-file-pdf">
    Obtén un resumen en PDF con saldos de una cuenta a una fecha específica.
  </Card>
  <Card title="Cartola completa personalizada" href="#" icon="fa-file-lines">
    Genera un PDF completo con movimientos, operaciones, resumen mensual y personalización visual.
  </Card>
</Cards>

<Callout icon="🚨" theme="danger">
  La cartola de una cuenta puede generarse con fecha límite **T-1** (día anterior al actual).
</Callout>

## Parámetros comunes

<Accordion title="Ver parámetros requeridos" icon="fa-table">

| Parámetro | Tipo | Tamaño Máx. | Descripción |
|---|---|---|---|
| `numCuenta` | string | 20 | Número identificador de la cuenta cliente. Ejemplo: `9999999/9` |
| `fecha` | date | — | Fecha de cierre a consultar. Ejemplo: `2022-12-26` |
| `codMoneda` | string | 3 | Moneda en la cual obtener la cartola. Ejemplo: `CLP` |

</Accordion>

<br />

## Cartola por cuenta

**→ GET** `/api/publicapi/creasys/Cartola/GetCartolaVoultech`

Obtiene un **resumen en formato PDF** del estado de una cuenta a una fecha específica. Equivale a una cartola simple con saldos.

<Accordion title="Ver alcance de este documento" icon="fa-file-pdf">

- Entrega una cartola simple en formato PDF.
- Resume el estado de la cuenta a una fecha de cierre específica.
- Se utiliza cuando necesitas consultar saldos sin el detalle completo de movimientos y operaciones.

</Accordion>

```
GET /api/publicapi/creasys/Cartola/GetCartolaVoultech?NumCuenta={numCuenta}&Fecha={fecha}&CodMonedaSld={codMoneda}
```

**Resultado esperado:** obtendrás un PDF con el resumen de la cuenta para la fecha y moneda consultadas.

<br />

## Cartola completa personalizada

**→ GET** `/api/publicapi/creasys/Cartola/GetCartolaCompletaVoultech`

Retorna un **documento PDF completo** que incluye:

- Todos los movimientos y operaciones del cliente
- Un **resumen mensual**
- Posibilidad de personalización con **logo y colores de la fintech**

<Accordion title="Ver alcance de este documento" icon="fa-file-lines">

- Incluye el detalle completo de movimientos y operaciones.
- Incorpora un resumen mensual de la cuenta.
- Permite personalización visual con identidad de la fintech.

</Accordion>

```
GET /api/publicapi/creasys/Cartola/GetCartolaCompletaVoultech?NumCuenta={numCuenta}&Fecha={fecha}&CodMonedaSld={codMoneda}
```

**Resultado esperado:** obtendrás un PDF completo con el detalle operativo de la cuenta y, cuando corresponda, la personalización visual configurada.