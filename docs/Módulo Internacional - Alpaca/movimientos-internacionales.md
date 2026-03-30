---
title: Movimientos Internacionales
excerpt: >-
  Consulte y registre aportes y retiros patrimoniales internacionales entre
  cuentas locales y cuentas Alpaca.
deprecated: false
hidden: false
metadata:
  robots: index
---
Registre y consulte movimientos patrimoniales internacionales entre cuentas locales y cuentas Alpaca.

## Alcance de esta página

Esta guía cubre los endpoints del módulo internacional para:

- consultar movimientos internacionales por rango de fechas o filtros específicos
- registrar aportes y retiros patrimoniales internacionales

<br />

## Consultar movimientos internacionales

**→ GET** `/api/publicapi/creasys/MovimientosAlpaca`

Obtiene una lista de movimientos en un rango de fecha determinado o filtrados por ID, UUID u otros criterios.

### Parámetros de consulta

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `Desde` | Fecha de inicio en formato `YYYY-MM-DD` | Sí, si no se proporciona `Id` o `Uuid` |
| `Hasta` | Fecha de término en formato `YYYY-MM-DD` | Sí, si no se proporciona `Id` o `Uuid` |
| `Identificador` | Identificador del cliente | No |
| `NumCuenta` | Número de cuenta | No |
| `CodTipoMovimiento` | Código del tipo de movimiento | No |
| `IdMovCaja` | ID del movimiento de caja | No |
| `Id` | ID del movimiento | No |
| `Uuid` | UUID del movimiento | No |
| `PageNumber` | Número de página para paginación | No |
| `PageSize` | Tamaño de página para paginación | No |

### Respuesta

```json title="Respuesta (200 OK)"
[
  {
    "id": 12345,
    "codTipoMovimiento": "RET_PAT_IT",
    "numCuenta": "19130340/0",
    "tipoMovimiento": "Retiro Patrimonial Internacional",
    "dscMovimiento": "Retiro patrimonial internacional",
    "fechaMovimiento": "2025-08-06T15:10:06-04:00",
    "fechaLiquidacion": "2025-08-06T15:10:06-04:00",
    "monto": 1000.00,
    "codMoneda": "USD",
    "dscCajaCuenta": "Cuenta Principal",
    "dscEstadoMovimiento": "Completado"
  },
  {
    "id": 12346,
    "codTipoMovimiento": "APO_PAT_IT",
    "numCuenta": "19130340/0",
    "tipoMovimiento": "Aporte Patrimonial Internacional",
    "dscMovimiento": "Aporte patrimonial internacional",
    "fechaMovimiento": "2025-08-05T10:15:20-04:00",
    "fechaLiquidacion": "2025-08-05T10:15:20-04:00",
    "monto": 2500.00,
    "codMoneda": "USD",
    "dscCajaCuenta": "Cuenta Principal",
    "dscEstadoMovimiento": "Completado"
  }
]
```

### Campos destacados de la respuesta

| Campo | Descripción |
|---|---|
| `codTipoMovimiento` | Tipo de movimiento registrado |
| `numCuenta` | Cuenta asociada al movimiento |
| `tipoMovimiento` | Nombre descriptivo del movimiento |
| `fechaMovimiento` | Fecha en que se registró el movimiento |
| `fechaLiquidacion` | Fecha de liquidación |
| `monto` | Monto del movimiento |
| `codMoneda` | Moneda del movimiento |
| `dscEstadoMovimiento` | Estado del movimiento |

<br />

## Registrar movimiento internacional

**→ POST** `/api/publicapi/creasys/MovimientosAlpaca/MovimientoInternacionalAlpaca`

Registra un movimiento internacional de tipo **retiro** o **aporte patrimonial** entre cuentas locales y cuentas Alpaca, según el tipo de movimiento especificado.

### Parámetros del request

| Parámetro | Descripción | Obligatorio |
|---|---|---|
| `codTipoMovimiento` | Tipo de movimiento: `RET_PAT_IT` o `APO_PAT_IT` | Sí |
| `numCuenta` | Número de cuenta (máx. 15 caracteres) | Sí |
| `obsMovimiento` | Observaciones del movimiento (máx. 100 caracteres) | No |
| `fechaMovimiento` | Fecha del movimiento en formato ISO 8601 | No |
| `fechaLiquidacion` | Fecha de liquidación en formato ISO 8601 | No |
| `monto` | Monto del movimiento (> 0) | Sí |
| `codMoneda` | Código de moneda (máx. 3 caracteres) | Sí |
| `dscMedioPagoCobro` | Medio de pago o cobro (máx. 20 caracteres) | No |
| `banco` | Nombre del banco (máx. 60 caracteres) | No |
| `numeroCuenta` | Número de cuenta bancaria (máx. 20 caracteres) | No |
| `tipoCuenta` | Tipo de cuenta bancaria (máx. 20 caracteres) | No |
| `uuid` | UUID del movimiento (máx. 36 caracteres) | No |
| `idUsuarioGpi` | ID del usuario en GPI | Sí |
| `tipo` | Tipo de operación (campo genérico) | No |

### Tipos de movimiento soportados

| Código | Descripción |
|---|---|
| `RET_PAT_IT` | Retiro patrimonial internacional |
| `APO_PAT_IT` | Aporte patrimonial internacional |

### Ejemplo de request

```json title="Request Body"
{
  "codTipoMovimiento": "RET_PAT_IT",
  "numCuenta": "19130340/0",
  "monto": 1000.00,
  "codMoneda": "USD",
  "idUsuarioGpi": 12345
}
```

### Respuesta

```json title="Respuesta (201 Created)"
{
  "id": 12345,
  "codTipoMovimiento": "RET_PAT_IT",
  "numCuenta": "19130340/0",
  "obsMovimiento": "Retiro patrimonial internacional",
  "fechaMovimiento": "2025-08-06T15:10:06-04:00",
  "fechaLiquidacion": "2025-08-06T15:10:06-04:00",
  "monto": 1000.00,
  "codMoneda": "USD",
  "dscMedioPagoCobro": "Transferencia",
  "banco": "Banco Internacional",
  "numeroCuenta": "123456789",
  "tipoCuenta": "Corriente",
  "uuid": "a1b2c3d4-e5f6-g7h8-i9j0-k1l2m3n4o5p6",
  "idUsuarioGpi": 12345
}
```

<Callout icon="💡" theme="info">
  Utilice `uuid` para mantener trazabilidad e idempotencia sobre cada movimiento internacional registrado.
</Callout>

<br />

## Flujo recomendado

1. Registre el movimiento con `POST /MovimientosAlpaca/MovimientoInternacionalAlpaca`
2. Guarde el `uuid` y el `id` retornado en la respuesta
3. Consulte el histórico con `GET /MovimientosAlpaca`
4. Filtre por `CodTipoMovimiento`, `NumCuenta`, `Id` o `Uuid` según corresponda

## Relación con otros componentes del módulo internacional

- Use **Cuentas Internacionales** para validar la cuenta Alpaca asociada
- Use **Actividad y Custodias** para revisar el impacto posterior del movimiento en la cuenta
- Use **Órdenes Internacionales** para operar instrumentos una vez disponibles los fondos
