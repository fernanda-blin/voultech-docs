---
title: Listados del Sistema
excerpt: >-
  Catálogos del sistema: bancos, comunas, monedas, perfiles de riesgo, tipos de
  cuenta, documentos y más.
deprecated: false
hidden: false
metadata:
  robots: index
---
Catálogos de datos usados en diferentes operaciones del sistema. Estos listados sirven como referencia para poblar menús, validar información o construir solicitudes hacia otros endpoints.

**Formato general de las URLs:**

```
GET /api/publicapi/creasys/{NombreLista}
```

<br />

## Datos geográficos

**→ GET** `/api/publicapi/creasys/Pais` — Países disponibles para residencia o nacionalidad

**→ GET** `/api/publicapi/creasys/Region` — Regiones del país

**→ GET** `/api/publicapi/creasys/Comuna` — Comunas, utilizadas en la definición de domicilios

<br />

## Datos financieros

**→ GET** `/api/publicapi/creasys/Moneda` — Monedas admitidas (`CLP`, `USD`, `EUR`, etc.)

**→ GET** `/api/publicapi/creasys/Banco` — Bancos disponibles para operaciones de pago o cobro

**→ GET** `/api/publicapi/creasys/PerfilRiesgo` — Perfiles de riesgo configurados en la corredora

<Callout icon="💡" theme="info">
  El perfil de riesgo se utiliza al crear una cuenta, ya que se debe asignar un perfil al titular.
</Callout>

**→ GET** `/api/publicapi/creasys/Contraparte` — Contrapartes registradas en operaciones

**→ GET** `/api/publicapi/creasys/PublicadorPrecio` — Precios publicados de instrumentos financieros

<br />

## Tipos de cuenta y administración

**→ GET** `/api/publicapi/creasys/TipoCuenta` — Tipos de cuenta disponibles (corriente, inversión, custodia, etc.)

**→ GET** `/api/publicapi/creasys/TipoCuentaBanco` — Tipos de cuenta bancaria (vista, corriente, ahorro, etc.)

**→ GET** `/api/publicapi/creasys/TipoAdministracion` — Categorías de administración aplicables a cuentas o carteras

<br />

## Tipos de entidad y persona

**→ GET** `/api/publicapi/creasys/TipoEntidad` — Categorías de entidad jurídica o natural

**→ GET** `/api/publicapi/creasys/TipoIdentificacion` — Tipos de documento de identidad (RUT, pasaporte, DNI extranjero, etc.)

**→ GET** `/api/publicapi/creasys/TipoSociedad` — Tipos societarios aplicables a personas jurídicas

**→ GET** `/api/publicapi/creasys/EstadoCivil` — Estados civiles disponibles para personas naturales

**→ GET** `/api/publicapi/creasys/ContratoMatrimonial` — Regímenes matrimoniales (sociedad conyugal, separación, participación, etc.)

<br />

## Tipos de contacto y dirección

**→ GET** `/api/publicapi/creasys/TipoContacto` — Tipos de contacto asociados a una persona

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoDireccion` — Tipos de dirección (comercial, personal, tributaria, etc.)

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoTelefono` — Tipos de teléfono (celular, fijo, laboral)

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoMail` — Tipos de email (personal, corporativo, principal)

**→ GET** `/api/publicapi/creasys/Contacto` — Contactos asociados a clientes, contrapartes o personas naturales

<br />

## Operaciones y movimientos

**→ GET** `/api/publicapi/creasys/TipoMedioPagoCobro` — Medios de pago o cobro habilitados (`P` = Pago/abonos, `C` = Cobro/retiros)

**→ GET** `/api/publicapi/creasys/TipoMovCaja` — Tipos de movimientos de caja (ingresos, retiros, transferencias, etc.)

**→ GET** `/api/publicapi/creasys/RelacionClienteConBanco` — Tipos de relación entre cliente y corredora según norma NCG 69

<br />

## Contratos y documentos

**→ GET** `/api/publicapi/creasys/ContratoOperacion/GetTipoContrato` — Tipos de contrato para operaciones financieras

**→ GET** `/api/publicapi/creasys/ContratoOperacion/GetTipoDocumento` — Tipos de documentos asociados a contratos

**→ GET** `/api/publicapi/creasys/CodigoTipoDocumento` — Tipos de documento requeridos para enrolamiento o identificación

**→ GET** `/api/publicapi/creasys/CertificadoCustodias` — Certificados de custodia asociados a clientes

<br />

## Cierre del sistema

**→ GET** `/api/publicapi/shared/Cierre/Sistema` — Última fecha de cierre del sistema

**→ GET** `/api/publicapi/shared/Cierre/Cuentas` — Cierre por cuenta

**→ GET** `/api/publicapi/shared/Cierre/MinComun` — Fecha mínima común de cierre aplicable a múltiples módulos

**→ GET** `/api/publicapi/shared/Cierre/DatosEnProceso` — Verifica si hay datos aún en actualización

<Callout icon="💡" theme="info">
  Usa `DatosEnProceso` para detectar si los saldos o movimientos están aún "en movimiento" antes de mostrar información al cliente.
</Callout>