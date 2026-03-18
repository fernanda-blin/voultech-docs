---
title: Sistema de Eventos
excerpt: >-
  Notificaciones en tiempo real vía Azure Service Bus: movimientos bancarios,
  Shinkansen y validaciones KYC.
deprecated: false
hidden: false
metadata:
  robots: index
---
Recibe notificaciones automáticas sobre movimientos bancarios, ejecuciones de pagos (Shinkansen) y validaciones de KYC a través de una cola de mensajería Azure Service Bus.

## Conexión

La conexión se realiza mediante una cadena de conexión a Service Bus:

```
sb://voultech.servicebus.windows.net/{NombreCola}
```

<Callout icon="⚠️" theme="warning">
  El nombre de la cola es entregado por Voultech a cada fintech. Solicítalo al equipo de soporte.
</Callout>

**Horarios de procesamiento:**
- Los mensajes de **aportes automáticos** se reciben entre **09:00 y 14:55 hrs**
- Los aportes realizados fuera de este horario se encolan para el día siguiente

<br />

## Suscripción a eventos

Ejemplo en **C#** para suscribirte a la cola y procesar mensajes:

```csharp title="Ejemplo de suscripción — C#"
using Azure.Messaging.ServiceBus;
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        string connectionString = "<cadena_de_conexión>";
        string queueName = "<nombre_de_cola>";

        ServiceBusClient client = new ServiceBusClient(connectionString);
        ServiceBusProcessor processor = client.CreateProcessor(queueName);

        processor.ProcessMessageAsync += MessageHandler;
        processor.ProcessErrorAsync += ErrorHandler;

        await processor.StartProcessingAsync();
        Console.WriteLine("Presiona ENTER para detener...");
        Console.ReadLine();

        await processor.StopProcessingAsync();
        await processor.DisposeAsync();
        await client.DisposeAsync();
    }

    static async Task MessageHandler(ProcessMessageEventArgs args)
    {
        string body = args.Message.Body.ToString();
        Console.WriteLine($"Recibido: {body}");
        await args.CompleteMessageAsync(args.Message);
    }

    static Task ErrorHandler(ProcessErrorEventArgs args)
    {
        Console.WriteLine($"Error: {args.Exception.Message}");
        return Task.CompletedTask;
    }
}
```

<br />

## Mensajes

Los mensajes se envían en formato **JSON** con una frecuencia de **1 minuto**. Cada mensaje contiene un campo `Topic` que identifica el tipo de evento.

### Movimiento bancario

**Topic:** `MOVBANCO`

```json title="Ejemplo"
{
  "Topic": "MOVBANCO",
  "TransactionId": "236",
  "Banco": "",
  "BookingDate": "2024-01-16T00:00:00",
  "TransactionTime": "2024-01-15T14:15:39.527",
  "AbonoCargo": "A",
  "DebtorUserId": "151837115",
  "TransactionInformation": "TRANSFERENCIA DESDE Security DE RAMIRO JOSE EXEQUIEL GAR | Fondo de liquidez",
  "Amount": 250000.0,
  "IdMovCaja": "0",
  "TransactionIdCartola": "123",
  "Result": ""
}
```

### Reversa de movimiento bancario

**Topic:** `REVERSAMOVBANCO`

```json title="Ejemplo"
{
  "Topic": "REVERSAMOVBANCO",
  "TransactionId": "236",
  "Banco": "",
  "BookingDate": "2024-01-16T00:00:00",
  "TransactionTime": "2024-01-15T14:15:39.527",
  "AbonoCargo": "A",
  "DebtorUserId": "151837115",
  "TransactionInformation": "TRANSFERENCIA DESDE Security DE RAMIRO JOSE EXEQUIEL GAR | Fondo de liquidez",
  "Amount": 250000.0,
  "IdMovCaja": "0",
  "TransactionIdCartola": "123",
  "Result": ""
}
```

### Movimiento Shinkansen

**Topic:** `MOVSHINKANSEN`

```json title="Ejemplo"
{
  "Topic": "MOVSHINKANSEN",
  "TransactionId": "4",
  "MovId": 10811975,
  "Date": "2024-09-09T12:04:30.757",
  "State": "OK",
  "Information": ""
}
```

### Validación de KYC

**Topic:** `KYCVALIDATION`

<Tabs>
  <Tab title="Aprobada">

```json
{
  "Topic": "KYCVALIDATION",
  "Date": "{fecha}",
  "State": "OK",
  "Code": "001",
  "UserId": "{identificador}",
  "Information": "AML OK",
  "Detail": {}
}
```

  </Tab>
  <Tab title="Rechazada">

```json
{
  "Topic": "KYCVALIDATION",
  "Date": "{fecha}",
  "State": "Error",
  "Code": "{código error}",
  "UserId": "{identificador}",
  "Information": "{tipo error}",
  "Detail": {}
}
```

  </Tab>
</Tabs>

**Códigos de error KYC:**

| Code | Information | Descripción |
|---|---|---|
| 002 | AML High Risk | Cliente de riesgo alto en listas de vigilancia |
| 003 | Cannot get document data | No se puede extraer la información del documento de identidad |
| 004 | Missing data | Falta información del cliente (RUT, codDocumento, etc.) |
| 005 | Document rejected | Documento rechazado por proveedor |
| 006 | Code not found | No se encuentra codIdentificación |

<Callout icon="💡" theme="info">
  El sistema de eventos está diseñado para notificaciones **en tiempo real** (pub/sub). Si pierdes un mensaje, deberás consultar el estado manualmente vía API (por ejemplo: consultar saldo o estado KYC directamente).
</Callout>