---
title: "Guía de Integración con Alpaca Markets"
excerpt: "Cómo integrar operaciones bursátiles internacionales a través de Voultech: onboarding, apertura de cuenta, fondeo y trading."
---

# 🦙 Guía de Integración con Alpaca Markets

Esta guía explica el flujo completo para que una fintech integre operaciones bursátiles internacionales a través de Voultech. Los detalles técnicos de cada endpoint están disponibles en el **API Reference**.

---

## Visión General

La arquitectura es **multi-tenant**: los usuarios se crean primero en el ecosistema de Voultech y luego se replican a Alpaca Markets.

```
Tu aplicación → API Voultech → Alpaca Markets (EE.UU.)
```

> Las confirmaciones de ejecución llegan de forma **asíncrona** vía **Service Bus / Webhook**.

---

## Tipos de Cliente

Antes del kickoff, identificar el tipo de fintech para determinar el flujo:

| Tipo | Descripción |
|------|-------------|
| **Fintech con mercado nacional** | Ya opera con Voultech en bolsa chilena. Solo necesita ejecutar `CrearClienteAlpaca` para usuarios que deseen operar internacionalmente. |
| **Fintech solo Alpaca** | Nueva o extranjera. Requiere creación completa en Voultech (flujo de 3 pasos). |

---

## Onboarding

### Reunión de Kickoff

Instancia obligatoria antes de cualquier integración. El objetivo es que el equipo técnico de la fintech conozca el flujo completo.

**Preguntas clave a definir en el kickoff:**
- ¿Ya están integrados con mercado local (Voultech)?
- ¿Qué proveedor de KYC usarán? *(Rillis recomendado / Identyz)*
- ¿Qué nacionalidades tendrán sus usuarios?
- ¿Necesitan WebSocket de precios en tiempo real?

> ⚠️ La documentación técnica y acceso a endpoints se entrega **después** del kickoff, no antes.

### Configuración Previa (a cargo de Voultech)

1. **Asesor** — Se crea un asesor en el sistema que agrupa a todos los usuarios de la fintech.
2. **Secuencia / numCuenta** — Se asigna la secuencia y el formato de `numCuenta` para esa fintech.
3. **Credenciales API** — La fintech provee un correo de contacto técnico y se generan las credenciales.

---

## KYC y Validación de Identidad

Alpaca exige el cumplimiento del **CIP (Customer Identification Program)** para todos los usuarios en producción.

### Proveedores disponibles

| Proveedor | Descripción |
|-----------|-------------|
| **Rillis** | Proveedor principal integrado. *(Recomendado para nuevas integraciones)* |
| **Identyz** | Proveedor alternativo. *(Para quienes ya lo tienen integrado)* |

### Webhook de Rillis

Cuando un usuario completa el flujo de validación, Rillis envía un webhook con este payload:

```json
{
  "type": "aprobado",
  "id": "<id_flujo_rillis>"
}
```

> 🚨 Usar el campo `id` para identificar al usuario. **No usar** `data.flow_instance_id`.

### Puntos críticos

- El campo `codIdentificacion` (código Rillis) es **obligatorio en producción**. Su ausencia hará fallar las activaciones aunque todo funcione en QA.
- En QA, Alpaca **no solicita** validación KYC. Es esperado y no impide abrir cuentas de prueba.

---

## Flujo de Apertura de Cuenta

Se ejecutan **3 llamadas en secuencia**. Si la fintech ya tiene al cliente en Voultech (mercado local), **solo ejecutar el Paso 3**.

```
Paso 1            Paso 2           Paso 3
POST /Clientes → POST /Cuentas → POST /CrearClienteAlpaca
 (Voultech)       (Voultech)         (Alpaca)
```

### Paso 1: Crear Cliente en Voultech

`POST /api/publicapi/creasys/Clientes`

```json
{
  "persona": {
    "identificador": "11111111-3",
    "tipoIdentificador": "P",
    "tipoEntidad": "N",
    "nombre": "Tomás",
    "paterno": "Prueba",
    "email": "tomas.prueba@fintech.com",
    "celular": "+59891415499",
    "fechaNacimiento": "2000-11-29",
    "empleador": "Empresa",
    "cargo": "Desarrollador",
    "dscEstadoCivil": "CASADO",
    "dscPais": "URUGUAY",
    "direccionPersona": [
      {
        "direccion": "Av. 18 de Julio",
        "numero": "1234",
        "idComunaCiudadNavigation": {
          "dscComunaCiudad": "URUGUAY"
        }
      }
    ]
  },
  "codIdentificacion": "codigo-rillis-del-usuario",
  "asesor": [{ "abrNombre": "abreviatura-fintech" }]
}
```

> `dscComunaCiudad` se usa para mapear comunas de países extranjeros.
> `codIdentificacion` es obligatorio en producción.

### Paso 2: Crear Cuenta en Voultech

`POST /api/publicapi/creasys/Cuentas`

```json
{
  "numCuenta": "123456789/0",
  "dscCuenta": "Cuenta Alpaca USD",
  "identificador": "11111111-3",
  "codMoneda": "USD",
  "codTipoAdministracion": "NF",
  "dscPerfilRiesgo": "Moderado",
  "dscTipoCuenta": "EXTRANJERA",
  "abrAsesor": "abreviatura-fintech"
}
```

### Paso 3: Crear Cuenta en Alpaca

`POST /api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`

```json
{
  "numCuenta": "123456789/0",
  "identificador": "11111111-3",
  "fundingSource": ["employment_income"]
}
```

**Valores válidos para `fundingSource`:** `employment_income`, `savings`, `investments`, `inheritance`, `business_income`, `family`.

---

## Fondeo de Cuenta

Los movimientos de fondos entre la fintech, la caja USD de Voultech y Alpaca siguen este esquema:

### Aportes

```
FINTECH → Registrar aporte (Voultech) → Enviar a Alpaca
```

**1. Registrar el aporte en Voultech**
`POST /api/publicapi/creasys/Movimientos/IngresoAporteRetiro`

```json
{
  "codTipoMovimiento": "APO_PAT_LI",
  "numCuenta": "123456789/0",
  "monto": 1000,
  "codMoneda": "USD",
  "fechaMovimiento": "2026-01-27T00:00:00.000Z",
  "fechaLiquidacion": "2026-01-27T00:00:00.000Z",
  "dscMedioPagoCobro": "TRANSFERENCIA",
  "banco": "Banco XYZ",
  "numeroCuenta": "12345678",
  "tipoCuenta": "CORRIENTE",
  "uuid": "uuid-unico-del-movimiento"
}
```

**2. Enviar los fondos a Alpaca**
`POST /api/publicapi/creasys/MovimientosAlpaca/MovimientoInternacionalAlpaca`

```json
{
  "codTipoMovimiento": "APO_PAT_LI",
  "numCuenta": "123456789/0",
  "monto": 1000,
  "codMoneda": "USD",
  "dscMedioPagoCobro": "TRANSFERENCIA"
}
```

### Retiros

```
Alpaca → Retirar desde Alpaca → Registrar retiro (Voultech) → FINTECH
```

**1. Retirar desde Alpaca** — mismo endpoint `MovimientoInternacionalAlpaca` con `codTipoMovimiento` de retiro (`RET_PAT_LI`).

**2. Registrar el retiro en Voultech**
`POST /api/publicapi/creasys/Movimientos/NuevoIngresoAporteRetiro`

```json
{
  "numCuenta": "123456789/0",
  "codTipoMovimiento": "RET_PAT_LI",
  "monto": 500,
  "codMoneda": "USD",
  "fechaMovimiento": "2026-01-27T00:00:00.000Z",
  "fechaLiquidacion": "2026-01-27T00:00:00.000Z",
  "banco": "Banco XYZ",
  "numeroCuenta": "12345678",
  "tipoCuenta": "CORRIENTE",
  "uuid": "uuid-unico-del-movimiento"
}
```

---

## Trading

Las órdenes se ingresan con `POST /api/publicapi/creasys/Ordenes/IngresarOrdenesMercado`.

Para cuentas internacionales usar:
- `codBolsa`: `ALPACA`
- `tipoSeguridad`: `CS`
- `tipoLiquidacion`: `T2`

```json
[{
  "uuid": "uuid-unico-de-la-orden",
  "numCuenta": "123456789/0",
  "nemotecnico": "AMZN",
  "cantidad": 1,
  "precio": 180,
  "tipoPrecio": "LIMIT",
  "tipoOperacion": "C",
  "tipoSeguridad": "CS",
  "tipoLiquidacion": "T2",
  "codBolsa": "ALPACA"
}]
```

> ⚠️ La ejecución **no es inmediata**. La confirmación llega de forma asíncrona vía **Service Bus**.

Para consultar precios usar `GET /api/publicapi/creasys/Asset/LastQuote` con el parámetro `nemo` (ej: `AMZN`).

---

## WebSocket y Precios en Tiempo Real

| Canal | Estado |
|-------|--------|
| Precios de instrumentos | ⚙️ Requiere coordinación con el equipo de Voultech |
| Saldo de caja (cash) | ✅ Disponible |
| Equity total | ❌ No disponible — usar polling a `SaldoAlpaca` |

---

## Ambiente de Pruebas (QA)

**Swagger QA:**
```
https://apiwebcbvoultechcertificacion.azurewebsites.net/swagger/index.html?urls.primaryName=Public%20API
```

**Flujo recomendado:**
1. Crear usuarios de prueba con el flujo completo (Paso 1 → 2 → 3)
2. Probar aportes, retiros, órdenes y consultas
3. Validar recepción de confirmaciones por Service Bus
4. Revisión con equipo Voultech antes del go-live

> ⚠️ Hacer pruebas con un **número reducido de usuarios** antes del go-live en producción.

---

## Checklist de Go-Live

- [ ] Kickoff realizado y flujo validado
- [ ] Tipo de cliente identificado (local / solo Alpaca)
- [ ] Proveedor KYC definido (Rillis / Identyz)
- [ ] Webhook de Rillis configurado y probado
- [ ] Asesor creado en sistema Voultech
- [ ] Credenciales API entregadas
- [ ] Secuencia / `numCuenta` asignados
- [ ] Flujo completo probado en QA (Paso 1 → 2 → 3)
- [ ] `codIdentificacion` incluido en POST Clientes
- [ ] Service Bus configurado para recibir confirmaciones
- [ ] WebSocket de precios solicitado (si aplica)
- [ ] Aprobación del equipo Voultech para pase a producción
