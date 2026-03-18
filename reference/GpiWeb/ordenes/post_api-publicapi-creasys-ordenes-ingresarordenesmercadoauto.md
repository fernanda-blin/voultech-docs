---
title: Ingreso de órdenes directo a mercado OMS (versión automática con modalidades).
excerpt: "Este endpoint permite ingresar órdenes al mercado basándose en los parámetros especificados en la solicitud.\r\nSe realiza una serie de validaciones para asegurar que la orden pueda ser procesada:\r\n<list type=\"bullet\"><item><description>Validación de que se proporcionaron órdenes y que su cantidad no exceda el límite permitido.</description></item><item><description>Verificación de que no haya UUIDs duplicados en la lista de órdenes.</description></item><item><description>Validación de la existencia de la cuenta asociada con la orden.</description></item><item><description>Comprobación de la existencia del instrumento financiero especificado.</description></item><item><description>Verificación de que el UUID proporcionado no haya sido utilizado previamente para evitar duplicaciones.</description></item></list>"
api:
  file: swagger.json
  operationId: post_api-publicapi-creasys-ordenes-ingresarordenesmercadoauto
hidden: false
---