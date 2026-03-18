---
title: Introducción a la API de Voultech
excerpt: >-
  Solución Investment-as-a-Service para fintechs: onboarding, cuentas,
  operaciones y eventos en tiempo real.
hidden: false
---
La API de Voultech ofrece una solución **Investment-as-a-Service** que permite a fintechs y empresas integrar funcionalidades clave para ofrecer productos de inversión: onboarding digital (KYC/compliance), apertura de cuentas, gestión de caja en múltiples divisas, compra/venta de instrumentos financieros y seguimiento de eventos en tiempo real.

<Callout icon="📧" theme="info">
  ¿Necesitas ayuda? Escríbenos a **soporte@voultech.com**
</Callout>

## ¿Qué puedes hacer con nuestra API?

<Cards>
  <Card title="Onboarding de clientes" icon="fa-duotone fa-user-plus">Registra usuarios, carga documentos de identidad y ejecuta validación KYC/Compliance.</Card>

  <Card title="Apertura de cuentas" icon="fa-duotone fa-wallet">Crea cuentas de inversión en distintas divisas y perfiles de riesgo.</Card>

  <Card title="Gestión de caja y fondos" icon="fa-duotone fa-money-bill-transfer">Recibe abonos en CLP, realiza retiros automáticos y opera en múltiples monedas.</Card>

  <Card title="Compra/venta de instrumentos" icon="fa-duotone fa-chart-line">Ejecuta órdenes de mercado directamente desde tu plataforma.</Card>

  <Card title="Cartera y movimientos" icon="fa-duotone fa-briefcase">Consulta portafolio, saldos, movimientos históricos y certificados de custodia.</Card>

  <Card title="Eventos en tiempo real" icon="fa-duotone fa-bell">Recibe notificaciones automáticas sobre KYC, movimientos de caja y precios.</Card>
</Cards>

<br />

## Requisitos previos

| Requisito | Detalle |
|---|---|
| **Credenciales** | Solicita `userName`, `password` y `abrAsesor` (código de asesor) al equipo de Voultech |
| **Autenticación** | `POST /api/publicapi/shared/auth/SignIn` para obtener tu Bearer token |
| **Sandbox** | `https://apiwebcbvoultechcertificacion.azurewebsites.net` |
| **Producción** | Acceso bajo coordinación directa con el equipo de Voultech |
| **Formato** | API RESTful, JSON, OpenAPI 3.0.1, vía HTTPS |

<Callout icon="⚠️" theme="warning">
  Las credenciales de Sandbox y Producción son independientes. Un token de un entorno **no funciona** en el otro.
</Callout>

<br />

## Empieza aquí

<Cards>
  <Card title="Guía de Inicio Rápido" href="#" icon="fa-duotone fa-rocket-launch">Tu primera integración en 5 pasos: autenticarte, crear cliente, abrir cuenta y hacer tu primer aporte.</Card>

  <Card title="Autenticación" href="#" icon="fa-duotone fa-key">Cómo obtener y renovar tu Bearer token para acceder a la API.</Card>

  <Card title="API Reference" href="#" icon="fa-duotone fa-code">Explora todos los endpoints disponibles con ejemplos interactivos.</Card>
</Cards>

<br />

## Guías de integración

<Cards>
  <Card kind="tile" title="Enrolamiento de Clientes" href="#" icon="fa-duotone fa-user-check">Crea clientes, sube documentos y completa el KYC</Card>

  <Card kind="tile" title="Gestión de Cuentas" href="#" icon="fa-duotone fa-building-columns">Cuentas de inversión, cuentas bancarias y comisiones</Card>

  <Card kind="tile" title="Movimientos y Operaciones" href="#" icon="fa-duotone fa-arrow-right-arrow-left">Aportes, retiros, órdenes y operaciones spot</Card>

  <Card kind="tile" title="Sistema de Eventos" href="#" icon="fa-duotone fa-bell">Notificaciones en tiempo real vía Service Bus</Card>

  <Card kind="tile" title="Cuentas Internacionales (Alpaca)" href="#" icon="fa-duotone fa-globe">Opera instrumentos en mercados internacionales</Card>

  <Card kind="tile" title="Cartola y Reportes" href="#" icon="fa-duotone fa-file-pdf">Genera cartolas y reportes en PDF</Card>
</Cards>

<br />

## Referencia

<Cards>
  <Card kind="tile" title="Listados del Sistema" href="#" icon="fa-duotone fa-list">Catálogos de bancos, comunas, monedas, perfiles y más</Card>

  <Card kind="tile" title="Respuestas y Errores" href="#" icon="fa-duotone fa-circle-exclamation">Códigos HTTP, paginación y catálogos de errores</Card>

  <Card kind="tile" title="Preguntas Frecuentes" href="#" icon="fa-duotone fa-circle-question">Respuestas a las dudas más comunes de integración</Card>

  <Card kind="tile" title="Rate Limits" href="#" icon="fa-duotone fa-gauge-high">Límites de carga, horarios y restricciones</Card>

  <Card kind="tile" title="Glosario" href="#" icon="fa-duotone fa-book">Términos técnicos y financieros</Card>

  <Card kind="tile" title="Entidades Principales" href="#" icon="fa-duotone fa-diagram-project">Cliente, Cuenta, Caja, Orden, Cartera y más</Card>
</Cards>