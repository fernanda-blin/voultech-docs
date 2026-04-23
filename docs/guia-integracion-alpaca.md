---
title: "Guía de Integración con Alpaca Markets"
excerpt: "Documentación técnica para fintechs que deseen integrar operaciones bursátiles internacionales a través de Voultech."
---

# 🦙 Guía de Integración con Alpaca Markets

Documentación técnica para fintechs que deseen integrar operaciones bursátiles internacionales.

---

## 🎯 Visión General

La integración permite a fintechs ofrecer a sus usuarios la compra y venta de instrumentos bursátiles extranjeros a través de **Alpaca Markets**. La arquitectura es **multi-tenant**: los usuarios se crean primero en el ecosistema de Voultech y luego se replican a Alpaca Markets.

```
Tu aplicación → Endpoints Voultech → Alpaca Markets (EE.UU.)
```

> ℹ️ Las confirmaciones de ejecución llegan de forma **asíncrona** vía **Service Bus / Webhook**.

---

## 👥 Tipos de Cliente

Antes del kickoff, identificar el tipo de fintech para determinar el flujo correcto:

| Tipo | Descripción |
|------|-------------|
| **Fintech con mercado nacional** | Ya opera con Voultech en bolsa chilena. Solo necesita ejecutar el paso de `CrearClienteAlpaca` para usuarios que deseen operar internacionalmente. |
| **Fintech solo Alpaca** | Nueva o extranjera. Requiere creación completa de usuarios en el sistema Voultech (flujo de 3 pasos). |

---

## 🚀 Onboarding

### Reunión de Kickoff

Se debe realizar una reunión de kickoff para que el equipo técnico de la fintech conozca el flujo completo.

**Preguntas clave a definir:**
- ¿Ya están integrados con mercado local (Voultech)?
- ¿Qué proveedor de KYC van a usar? *(Rillis / Identyz — se sugiere Rillis)*
- ¿Qué nacionalidades tendrán sus usuarios?
- ¿Necesitan WebSocket de precios en tiempo real?

### Configuración Previa

1. **Solicitud de asesor** — La fintech solicita la creación de un asesor en el sistema. Este asesor agrupa a todos los usuarios de esa fintech.
2. **Asignación de secuencia / numCuenta** — Se asigna la secuencia y `numCuenta`.
3. **Creación de usuario API** — La fintech debe proveer un correo electrónico de contacto técnico. Con esto se generan las credenciales de API.

> ⚠️ La documentación técnica y acceso a endpoints se entrega **después** del kickoff y configuración, no antes.

---

## 🔐 KYC y Validación de Identidad

Alpaca exige el cumplimiento del **CIP (Customer Identification Program)** para todos los usuarios en producción.

### Proveedores Disponibles

| Proveedor | Descripción |
|-----------|-------------|
| **Rillis** | Proveedor principal integrado. *(Solo nuevos)* |
| **Identyz** | Proveedor alternativo. *(Para quienes ya lo tienen integrado)* |

### Webhook de Rillis

Payload esperado:

```json
{
  "type": "aprobado" | "rechazado",
  "id": "<id_flujo_rillis>"
}
```

> 🚨 **Crítico:** Usar el campo `id` para parear con el usuario. **NO usar** `data.flow_instance_id`.

> 🚨 **Crítico para producción:** El campo `codIdentificacion` (código Rillis) es **obligatorio en producción**. Su ausencia hará fallar las activaciones aunque funcionen en QA. No omitirlo en el POST de creación de cliente.

> ℹ️ **KYC en QA:** El flujo de validación KYC no es solicitado por Alpaca en el ambiente QA. Esto es esperado y no es necesario para abrir cuentas en QA.

---

## 👤 Flujo de Apertura de Cuenta

Se ejecutan **3 llamadas en secuencia**. Si la fintech ya tiene al cliente en Voultech (mercado local), **solo ejecutar el Paso 3**.

```
POST /Clientes  →  POST /Cuentas  →  POST /CrearClienteAlpaca
  (Voultech)         (Voultech)            (Alpaca)
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
    "profesion": "Desarrollador",
    "dscEstadoCivil": "CASADO",
    "dscPais": "URUGUAY",
    "direccionPersona": [
      {
        "direccion": "Av. 18 de Julio",
        "numero": "1234",
        "adicional": "Piso 5, Oficina 502",
        "idComunaCiudadNavigation": {
          "dscComunaCiudad": "URUGUAY"
        }
      }
    ]
  },
  "codIdentificacion": "Código Rillis",
  "asesor": [{ "abrNombre": "fintech" }]
}
```

> 📌 `dscComunaCiudad` se usa para mapear comunas extranjeras. `codIdentificacion` es obligatorio en producción.

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
  "abrAsesor": "fintech"
}
```

### Paso 3: Crear Cliente en Alpaca

`POST /api/publicapi/creasys/CuentaAlpaca/CrearClienteAlpaca`

```json
{
  "numCuenta": "123456789/0",
  "identificador": "11111111-3",
  "fundingSource": ["employment_income"]
}
```

---

## 💰 Fondeo de Cuenta

El fondeo en USD considera que los movimientos `APO_PAT_LI` y `RET_PAT_LI` se ejecutan vía APIs de Voultech (no directo a la caja). El movimiento hacia/desde Alpaca se realiza con el endpoint de **MovimientosAlpaca**.

### Aportes (FINTECH → Caja USD Voultech → Cliente Alpaca)

**Paso 1: Registrar Aporte**

`POST /api/publicapi/creasys/Movimientos/IngresoAporteRetiro`

```json
{
  "codTipoMovimiento": "APO_PAT_LI",
  "numCuenta": "123456789/0",
  "obsMovimiento": "Aporte cliente",
  "fechaMovimiento": "2026-01-27T00:00:00.000Z",
  "fechaLiquidacion": "2026-01-27T00:00:00.000Z",
  "monto": 1000,
  "codMoneda": "USD",
  "dscMedioPagoCobro": "TRANSFERENCIA",
  "banco": "Banco XYZ",
  "numeroCuenta": "12345678",
  "tipoCuenta": "CORRIENTE",
  "uuid": "550e8400-e29b-41d4-a716-446655440000"
}
```

**Paso 2: Enviar Dinero a Alpaca**

`POST /api/publicapi/creasys/MovimientosAlpaca/MovimientoInternacionalAlpaca`

```json
{
  "codTipoMovimiento": "APO_PAT_LI",
  "numCuenta": "123456789/0",
  "obsMovimiento": "Envío a Alpaca",
  "monto": 1000,
  "codMoneda": "USD",
  "dscMedioPagoCobro": "TRANSFERENCIA"
}
```

### Retiros (Cliente Alpaca → Caja USD Voultech → FINTECH)

**Paso 1: Retirar desde Alpaca** — Mismo endpoint `MovimientoInternacionalAlpaca` con `codTipoMovimiento` de retiro (`RET_PAT_LI`).

**Paso 2: Registrar Retiro**

`POST /api/publicapi/creasys/Movimientos/NuevoIngresoAporteRetiro`

```json
{
  "numCuenta": "123456789/0",
  "obsMovimiento": "Retiro cliente",
  "fechaMovimiento": "2026-01-27T00:00:00.000Z",
  "fechaLiquidacion": "2026-01-27T00:00:00.000Z",
  "monto": 500,
  "codMoneda": "USD",
  "codTipoMovimiento": "RET_PAT_LI",
  "banco": "Banco XYZ",
  "numeroCuenta": "12345678",
  "tipoCuenta": "CORRIENTE",
  "uuid": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

### Consultas de Movimientos y Saldo

| Endpoint | Descripción |
|----------|-------------|
| `GET /api/publicapi/creasys/Movimientos` | Historial de movimientos por cuenta y fecha |
| `GET /api/publicapi/creasys/Cajas/ConSaldoOnline` | Saldo online de cajas (parámetros: `NumCuenta`, `Fecha`, `CodMoneda`) |

---

## 📈 Operaciones de Trading

Para ingresar órdenes de compra/venta internacionales, ver la sección **Endpoints Alpaca — Referencia Completa**.

| Endpoint | Descripción |
|----------|-------------|
| `POST /api/publicapi/creasys/Ordenes/IngresarOrdenesMercado` | Ingresa órdenes nacionales (XSGO) o internacionales (ALPACA) |
| `GET /api/publicapi/creasys/Asset/LastQuote` | Última cotización de un instrumento |
| `GET /api/publicapi/creasys/CuentaAlpaca/SaldoAlpaca/{accountNumber}` | Saldo y equity de la cuenta Alpaca |

> ⚠️ La ejecución de órdenes **no es inmediata**. La confirmación llega de forma asíncrona vía **Service Bus**.

---

## 🔌 WebSocket y Precios en Tiempo Real

| Canal | Disponibilidad | Alternativa |
|-------|---------------|-------------|
| Precios de instrumentos | ⚙️ Requiere configuración | Endpoint `LastQuote` |
| Saldo de caja (cash) | ✅ Disponible | — |
| Equity total | ❌ No disponible | Polling a `SaldoAlpaca` |

> Si la fintech requiere precios en tiempo real vía WebSocket, debe coordinarse con el equipo para la habilitación del puerto correspondiente.

---

## 🧪 Ambiente de Pruebas (QA)

**Swagger QA:**
```
https://apiwebcbvoultechcertificacion.azurewebsites.net/swagger/index.html?urls.primaryName=Public%20API
```

**Flujo recomendado en QA:**
1. Crear usuarios de prueba con el flujo completo (Paso 1 → 2 → 3)
2. Probar aportes, retiros, órdenes y consultas
3. Validar recepción de confirmaciones por Service Bus
4. Revisión con equipo fintech antes del go-live

> ⚠️ Hacer pruebas en QA con un **número reducido de usuarios** antes del go-live en producción.

---

## ✅ Checklist de Go-Live

- [ ] Kickoff realizado y flujo validado
- [ ] Tipo de cliente identificado (local / solo Alpaca)
- [ ] Proveedor KYC definido (Rillis / Identyz)
- [ ] Webhook de Rillis configurado y probado
- [ ] Asesor creado en sistema
- [ ] `sys_usuario` y credenciales API entregadas
- [ ] Secuencia / `numCuenta` asignados
- [ ] Flujo completo probado en QA
- [ ] Revisión de procesos aprobada por equipo
- [ ] `codIdentificacion` incluido en POST Clientes
- [ ] Service Bus configurado para recibir confirmaciones
- [ ] WebSocket de precios solicitado (si aplica)
