---
title: Comparación entre los servicios gRPC y las API HTTP
author: jamesnk
description: Obtenga información sobre cómo compara gRPC con HTTP APIs y lo que tiene recomienda son escenarios.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/31/2019
uid: grpc/comparison
ms.openlocfilehash: 712010f62b418fc8964b48648e35698c7bd3b395
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376423"
---
# <a name="comparing-grpc-services-with-http-apis"></a>Comparación entre los servicios gRPC y las API HTTP

Por [James Newton-King](https://twitter.com/jamesnk)

Este artículo se explica cómo [gRPC servicios](https://grpc.io/docs/guides/) comparar a HTTP APIs (incluido ASP.NET Core [API Web](xref:web-api/index)). La tecnología utilizada para proporcionar una API para la aplicación es una opción importante y gRPC ofrece ventajas exclusivas en comparación con HTTP APIs. En este artículo se describe los puntos fuertes y débiles de gRPC y recomienda escenarios para usar gRPC a través de otras tecnologías.

#### <a name="overview"></a>Información general

|    Característica             |    gRPC                                                 |    API de HTTP con JSON                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Contrato            |    Necesario (`*.proto`)                                 |    Opcional (OpenAPI)                        |
|    Transporte           |    HTTP/2                                               |    HTTP                                      |
|    Payload             |    [Protobuf (binarios pequeños)](#performance)             |    JSON (grande, legible)              |
|    Prescriptiveness    |    [Especificación de Strict](#strict-specification)        |    Flexible. Cualquier HTTP es válido                  |
|    Streaming           |    [Cliente, servidor bidireccionales](#streaming)         |    Cliente, servidor                            |
|    Compatibilidad con exploradores     |    [No (requiere grpc web)](#limited-browser-support)   |    Sí                                       |
|    Seguridad            |    Transporte (HTTPS)                                    |    Transporte (HTTPS)                         |
|    Generación de código de cliente     |    [Sí](#code-generation)                              |    OpenAPI + las herramientas de terceros             |

## <a name="grpc-strengths"></a>puntos fuertes de gRPC

### <a name="performance"></a>Rendimiento

gRPC mensajes se serializan utilizando [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), un formato de mensaje binario eficaz. Protobuf serializa muy rápidamente en el servidor y cliente. Resultados de la serialización Protobuf en cargas de mensajes pequeños, importantes en escenarios de ancho de banda limitado, como las aplicaciones móviles.

gRPC está diseñado para HTTP/2, una revisión principal de HTTP que proporciona ventajas significativas de rendimiento a través de HTTP 1.x:

* Tramas binaria y la compresión. Protocolo HTTP/2 es compacto y eficaz tanto en el envío y recepción.
* Multiplexación de varias llamadas HTTP/2 a través de una única conexión TCP. Elimina la multiplexación [head de línea de bloqueo](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generación de código

Todos los marcos de gRPC proporcionan soporte técnico de primera generación de código. Es un archivo principal para desarrollo gRPC el [ `*.proto` archivo](https://developers.google.com/protocol-buffers/docs/proto3), que define el contrato de servicios de gRPC y mensajes. Desde este marcos de trabajo de archivo gRPC código generará una clase base del servicio, mensajes y un cliente completando.

Al compartir la `*.proto` archivo entre el servidor y cliente, los mensajes y código de cliente puede generarse desde final al final. Generación de código del cliente elimina la duplicación de los mensajes en el cliente y servidor y crea a un cliente fuertemente tipado para los usuarios. No hace falta escribir a un cliente ahorra el tiempo de desarrollo importante en las aplicaciones con muchos servicios.

### <a name="strict-specification"></a>Especificación de Strict

No existe una especificación formal de la API de HTTP con JSON. Los desarrolladores debatir el mejor formato de direcciones URL, los verbos HTTP y códigos de respuesta.

El [gRPC especificación](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) es preceptivo acerca del formato que debe seguir un servicio gRPC. gRPC elimina debate y ahorra tiempo al desarrollador porque gPRC es coherente en las plataformas y las implementaciones.

### <a name="streaming"></a>Streaming

HTTP/2 proporciona una base para las secuencias de comunicación de larga duración, en tiempo real. gRPC proporciona soporte de primera clase para streaming a través de HTTP/2.

Un servicio gRPC admite todas las combinaciones de streaming:

* Unario (ninguna transmisión por secuencias)
* Servidor a cliente de streaming
* Cliente a servidor de streaming
* Bidireccional de transmisión por secuencias

### <a name="deadlinetimeouts-and-cancellation"></a>Fecha límite o tiempos de espera y la cancelación

gRPC permite a los clientes especificar cuánto están dispuestos a esperar una llamada RPC a completar. El [fecha límite](https://grpc.io/blog/deadlines) se envía al servidor, y el servidor puede decidir qué acción realizar si supera la fecha límite. Por ejemplo, el servidor puede cancelar las solicitudes de gRPC/HTTP/base de datos en curso en tiempo de espera.

Propagación de la fecha límite y la cancelación mediante el elemento secundario gRPC llamadas ayuda a exigir límites de uso de recursos.

## <a name="grpc-recommended-scenarios"></a>gRPC recomendada escenarios

gRPC se adapta perfectamente a los siguientes escenarios:

* **Microservicios** &ndash; gRPC está diseñado para la comunicación de alto rendimiento y baja latencia. gRPC es excelente para microservicios ligera donde la eficiencia es fundamental.
* **Comunicación en tiempo real de punto a punto** &ndash; gRPC tiene excelente compatibilidad con streaming bidireccionales. gRPC services pueden insertar mensajes que en tiempo real sin sondeo.
* **Entornos políglotas** &ndash; gRPC herramientas es compatible con todos los lenguajes de desarrollo conocidas, lo gRPC una buena elección para entornos de varios idiomas.
* **Entornos restringidos de red** &ndash; gRPC mensajes se serializan con Protobuf, un formato de mensajes ligero. Un mensaje gRPC siempre es menor que un mensaje JSON equivalente.

## <a name="grpc-weaknesses"></a>gRPC débiles

### <a name="limited-browser-support"></a>Compatibilidad con exploradores limitado

No podrá llamar directamente a un servicio gRPC desde un explorador hoy en día. gRPC principalmente usa características HTTP/2 y ningún explorador proporciona el nivel de control necesario a través de las solicitudes web para admitir a un cliente gRPC. Por ejemplo, los exploradores no permiten un autor de llamada requerir el uso de HTTP/2 o proporcionar acceso a marcos HTTP/2 subyacentes.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) es una tecnología adicional desde el equipo gRPC que proporciona compatibilidad limitada gRPC en el explorador. gRPC Web consta de dos partes: un cliente de JavaScript que es compatible con todos los exploradores modernos y un proxy Web de gRPC en el servidor. El cliente gRPC Web llama al proxy y el servidor proxy reenviará en las solicitudes de gRPC al servidor gRPC.

No todas las características de gRPC son compatibles con Web gRPC. No se admite el cliente y bidireccionales de transmisión por secuencias, y hay compatibilidad limitada para el streaming server.

### <a name="not-human-readable"></a>No legible

Las solicitudes de API de HTTP se envían como texto y se pueden leer y creadas por los humanos.

gRPC mensajes se codifican con Protobuf de forma predeterminada. Aunque es eficaz para enviar y recibir Protobuf, su formato binario no es humano legible. Protobuf requiere la descripción de la interfaz del mensaje especificado en el `*.proto` archivo deserializar correctamente. Herramientas adicionales están necesaria para analizar cargas Protobuf en la conexión y componer las solicitudes a mano.

Características como [reflexión server](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) y [herramienta de línea de comandos gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) existe para ayudar a los mensajes binarios Protobuf. Además, los mensajes de soporte técnico Protobuf [conversión a y desde JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). La conversión de JSON integrada proporciona una manera eficaz para convertir Protobuf mensajes hacia y desde el formato de lenguaje natural al depurar.

## <a name="alternative-framework-scenarios"></a>Escenarios de marco de trabajo alternativo

Se recomiendan otros marcos sobre gRPC en los escenarios siguientes:

* **Las API de explorador accesible** &ndash; gRPC no se admite totalmente en el explorador. gRPC Web puede ofrecer compatibilidad con exploradores, pero tiene limitaciones y presenta a un servidor proxy.
* **Comunicación en tiempo real de difusión** &ndash; gRPC admite la comunicación en tiempo real a través de streaming, pero no existe el concepto de difundir un mensaje de salida para las conexiones registradas. Por ejemplo en un escenario de salón de chat que deben enviarse los mensajes de chat de nuevo a todos los clientes en el salón de chat, cada llamada gRPC es necesario transmitir individualmente los nuevos mensajes de chat para el cliente. [SignalR](xref:signalr/introduction) es un marco útil para este escenario. SignalR tiene el concepto de las conexiones persistentes y la compatibilidad integrada para los mensajes de difusión.
* **La comunicación entre procesos** &ndash; un proceso debe hospedar un servidor HTTP/2 para aceptar las llamadas entrantes gRPC. Para Windows, la comunicación entre procesos [canalizaciones](/dotnet/standard/io/pipe-operations) es un método rápido y ligero de comunicación.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
