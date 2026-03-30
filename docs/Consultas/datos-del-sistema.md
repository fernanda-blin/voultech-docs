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
Consulta los catálogos del sistema para obtener valores válidos que te permitan poblar menús, validar información y construir solicitudes a otros endpoints.

## Categorías disponibles

<Cards columns={4}>
  <Card title="Datos geográficos" href="#" icon="fa-earth-americas">
    Consulta países, regiones y comunas para datos de residencia, nacionalidad y domicilio.
  </Card>
  <Card title="Datos financieros" href="#" icon="fa-coins">
    Consulta monedas, bancos, perfiles de riesgo, contrapartes y precios publicados.
  </Card>
  <Card title="Cuentas y entidades" href="#" icon="fa-folder-open">
    Revisa tipos de cuenta, administración, entidad, identificación y sociedad.
  </Card>
  <Card title="Operación y control" href="#" icon="fa-list-check">
    Consulta medios de pago, movimientos, contratos, documentos y cierres del sistema.
  </Card>
</Cards>

**Formato general de las URLs:**

```
GET /api/publicapi/creasys/{NombreLista}
```

**Resultado esperado:** obtendrás los valores de referencia necesarios para construir requests válidos en el resto de la integración.

<br />

## Datos geográficos

<Accordion title="Ver listados geográficos" icon="fa-earth-americas">

**→ GET** `/api/publicapi/creasys/Pais` — Países disponibles para residencia o nacionalidad

**→ GET** `/api/publicapi/creasys/Region` — Regiones del país

**→ GET** `/api/publicapi/creasys/Comuna` — Comunas, utilizadas en la definición de domicilios

</Accordion>

<br />

## Datos financieros

<Accordion title="Ver listados financieros" icon="fa-coins">

**→ GET** `/api/publicapi/creasys/Moneda` — Monedas admitidas (`CLP`, `USD`, `EUR`, etc.)

**→ GET** `/api/publicapi/creasys/Banco` — Bancos disponibles para operaciones de pago o cobro

**→ GET** `/api/publicapi/creasys/PerfilRiesgo` — Perfiles de riesgo configurados en la corredora

<Callout icon="💡" theme="info">
  El perfil de riesgo se utiliza al crear una cuenta, ya que se debe asignar un perfil al titular.
</Callout>

**→ GET** `/api/publicapi/creasys/Contraparte` — Contrapartes registradas en operaciones

**→ GET** `/api/publicapi/creasys/PublicadorPrecio` — Precios publicados de instrumentos financieros

</Accordion>

<br />

## Tipos de cuenta y administración

<Accordion title="Ver listados de cuentas y administración" icon="fa-folder-open">

**→ GET** `/api/publicapi/creasys/TipoCuenta` — Tipos de cuenta disponibles (corriente, inversión, custodia, etc.)

**→ GET** `/api/publicapi/creasys/TipoCuentaBanco` — Tipos de cuenta bancaria (vista, corriente, ahorro, etc.)

**→ GET** `/api/publicapi/creasys/TipoAdministracion` — Categorías de administración aplicables a cuentas o carteras

</Accordion>

<br />

## Tipos de entidad y persona

<Accordion title="Ver listados de entidad y persona" icon="fa-users">

**→ GET** `/api/publicapi/creasys/TipoEntidad` — Categorías de entidad jurídica o natural

**→ GET** `/api/publicapi/creasys/TipoIdentificacion` — Tipos de documento de identidad (RUT, pasaporte, DNI extranjero, etc.)

**→ GET** `/api/publicapi/creasys/TipoSociedad` — Tipos societarios aplicables a personas jurídicas

**→ GET** `/api/publicapi/creasys/EstadoCivil` — Estados civiles disponibles para personas naturales

**→ GET** `/api/publicapi/creasys/ContratoMatrimonial` — Regímenes matrimoniales (sociedad conyugal, separación, participación, etc.)

</Accordion>

<br />

## Tipos de contacto y dirección

<Accordion title="Ver listados de contacto y dirección" icon="fa-address-book">

**→ GET** `/api/publicapi/creasys/TipoContacto` — Tipos de contacto asociados a una persona

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoDireccion` — Tipos de dirección (comercial, personal, tributaria, etc.)

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoTelefono` — Tipos de teléfono (celular, fijo, laboral)

**→ GET** `/api/publicapi/creasys/TipoDireccion/GetTipoMail` — Tipos de email (personal, corporativo, principal)

**→ GET** `/api/publicapi/creasys/Contacto` — Contactos asociados a clientes, contrapartes o personas naturales

</Accordion>

<br />

## Operaciones y movimientos

<Accordion title="Ver listados operativos" icon="fa-money-bill-transfer">

**→ GET** `/api/publicapi/creasys/TipoMedioPagoCobro` — Medios de pago o cobro habilitados (`P` = Pago/abonos, `C` = Cobro/retiros)

**→ GET** `/api/publicapi/creasys/TipoMovCaja` — Tipos de movimientos de caja (ingresos, retiros, transferencias, etc.)

**→ GET** `/api/publicapi/creasys/RelacionClienteConBanco` — Tipos de relación entre cliente y corredora según norma NCG 69

</Accordion>

<br />

## Contratos y documentos

<Accordion title="Ver listados de contratos y documentos" icon="fa-file-signature">

**→ GET** `/api/publicapi/creasys/ContratoOperacion/GetTipoContrato` — Tipos de contrato para operaciones financieras

**→ GET** `/api/publicapi/creasys/ContratoOperacion/GetTipoDocumento` — Tipos de documentos asociados a contratos

**→ GET** `/api/publicapi/creasys/CodigoTipoDocumento` — Tipos de documento requeridos para enrolamiento o identificación

**→ GET** `/api/publicapi/creasys/CertificadoCustodias` — Certificados de custodia asociados a clientes

</Accordion>

<br />

## Cierre del sistema

<Accordion title="Ver endpoints de cierre del sistema" icon="fa-calendar-check">

**→ GET** `/api/publicapi/shared/Cierre/Sistema` — Última fecha de cierre del sistema

**→ GET** `/api/publicapi/shared/Cierre/Cuentas` — Cierre por cuenta

**→ GET** `/api/publicapi/shared/Cierre/MinComun` — Fecha mínima común de cierre aplicable a múltiples módulos

**→ GET** `/api/publicapi/shared/Cierre/DatosEnProceso` — Verifica si hay datos aún en actualización

</Accordion>

<Callout icon="💡" theme="info">
  Usa `DatosEnProceso` para detectar si los saldos o movimientos están aún "en movimiento" antes de mostrar información al cliente.
</Callout>