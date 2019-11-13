---
title: Comparación entre los servicios gRPC y las API HTTP
author: jamesnk
description: Obtenga información sobre cómo se compara gRPC con las API de HTTP y cuáles son los escenarios recomendados.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/comparison
ms.openlocfilehash: ceb24d656827548492a6fa326681922297fc481b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963653"
---
# <a name="compare-grpc-services-with-http-apis"></a>Comparación entre los servicios gRPC y las API HTTP

Por [James Newton-King](https://twitter.com/jamesnk)

En este artículo se explica cómo se comparan los [servicios de gRPC](https://grpc.io/docs/guides/) con las API de http (incluidas ASP.net Core [API Web](xref:web-api/index)). La tecnología que se usa para proporcionar una API para la aplicación es una opción importante, y gRPC ofrece ventajas exclusivas en comparación con las API HTTP. En este artículo se describen los puntos fuertes y débiles de gRPC y se recomiendan escenarios para usar gRPC sobre otras tecnologías.

## <a name="high-level-comparison"></a>Comparación de alto nivel

En la tabla siguiente se ofrece una comparación de alto nivel de las características entre las API de gRPC y HTTP con JSON.

| Característica          | gRPC                                               | API HTTP con JSON           |
| ---------------- | -------------------------------------------------- | ----------------------------- |
| Contrato         | Requerido (*. proto*)                                | Opcional (OpenAPI)            |
| Protocolo         | HTTP/2                                             | HTTP                          |
| Payload          | [Protobuf (pequeño, binario)](#performance)           | JSON (grande, legible para usuarios)  |
| Preceptivo | [Especificación estricta](#strict-specification)      | Combinados. Cualquier HTTP es válido.     |
| Streaming        | [Cliente, servidor, bidireccional](#streaming)       | Cliente, servidor                |
| Compatibilidad con exploradores  | [No (requiere GRPC-Web)](#limited-browser-support) | Sí                           |
| Seguridad         | Transporte (TLS)                                    | Transporte (TLS)               |
| Generación de código de cliente | [Sí](#code-generation)                      | OpenAPI + herramientas de terceros |

## <a name="grpc-strengths"></a>gRPC fortalezas

### <a name="performance"></a>Rendimiento

los mensajes gRPC se serializan mediante [protobuf](https://developers.google.com/protocol-buffers/docs/overview), un formato de mensaje binario eficaz. Protobuf serializa muy rápidamente en el servidor y el cliente. La serialización de protobuf produce cargas de mensajes pequeñas, importante en escenarios de ancho de banda limitado como aplicaciones móviles.

gRPC está diseñado para HTTP/2, una revisión principal de HTTP que proporciona importantes ventajas de rendimiento sobre HTTP 1. x:

* Tramas binarias y compresión. El protocolo HTTP/2 es compacto y eficaz tanto para el envío como para la recepción.
* Multiplexación de varias llamadas HTTP/2 a través de una única conexión TCP. La multiplexación elimina el [bloqueo del cabezal de línea](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generación de código

Todos los marcos de trabajo de gRPC proporcionan compatibilidad de primera clase para la generación de código. Un archivo principal para el desarrollo de gRPC es el [archivo *. proto* ](https://developers.google.com/protocol-buffers/docs/proto3), que define el contrato de servicios y mensajes de gRPC. En este archivo, gRPC frameworks generará una clase base de servicio, mensajes y un cliente completo.

Al compartir el archivo *. proto* entre el servidor y el cliente, los mensajes y el código de cliente se pueden generar de extremo a extremo. La generación de código del cliente elimina la duplicación de mensajes en el cliente y el servidor, y crea un cliente fuertemente tipado. No tener que escribir un cliente ahorra tiempo de desarrollo significativo en aplicaciones con muchos servicios.

### <a name="strict-specification"></a>Especificación estricta

No existe una especificación formal para la API HTTP con JSON. Los desarrolladores debaten el mejor formato de las direcciones URL, los verbos HTTP y los códigos de respuesta.

La [especificación gRPC](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) es preceptiva sobre el formato que debe seguir un servicio gRPC. gRPC elimina el debate y ahorra tiempo de desarrollador porque gRPC es coherente entre las plataformas y las implementaciones.

### <a name="streaming"></a>Streaming

HTTP/2 proporciona una base para las secuencias de comunicación en tiempo real de larga duración. gRPC proporciona compatibilidad de primera clase para la transmisión por secuencias a través de HTTP/2.

Un servicio gRPC admite todas las combinaciones de streaming:

* Unario (sin transmisión por secuencias)
* Streaming de servidor a cliente
* Streaming de cliente a servidor
* Streaming bidireccional

### <a name="deadlinetimeouts-and-cancellation"></a>Fecha límite/tiempos de espera y cancelación

gRPC permite a los clientes especificar cuánto tiempo están dispuestos a esperar a que se complete una RPC. La [fecha límite](https://grpc.io/blog/deadlines) se envía al servidor y el servidor puede decidir qué acción realizar si supera la fecha límite. Por ejemplo, el servidor puede cancelar las solicitudes de gRPC/HTTP/Database en curso en el tiempo de espera.

La propagación de la fecha límite y la cancelación mediante llamadas secundarias de gRPC ayuda a exigir los límites de uso de recursos.

## <a name="grpc-recommended-scenarios"></a>gRPC escenarios recomendados

gRPC es idóneo para los siguientes escenarios:

* Los **microservicios** &ndash; gRPC están diseñados para una baja latencia y una comunicación de alto rendimiento. gRPC es ideal para microservicios ligeros en los que la eficacia es crítica.
* La **comunicación en tiempo real de punto a punto** &ndash; gRPC tiene una excelente compatibilidad para el streaming bidireccional. gRPC Services puede enviar mensajes en tiempo real sin sondeos.
* Los **entornos de Polyglot** &ndash; gRPC Tooling admiten todos los lenguajes de desarrollo más populares, lo que convierte a gRPC en una buena opción para entornos de varios idiomas.
* Los **entornos con restricción de red** &ndash; mensajes gRPC se serializan con protobuf, un formato de mensaje ligero. Un mensaje de gRPC siempre es más pequeño que un mensaje JSON equivalente.

## <a name="grpc-weaknesses"></a>debilidades de gRPC

### <a name="limited-browser-support"></a>Compatibilidad con exploradores limitados

Hoy no es posible llamar directamente a un servicio gRPC desde un explorador. gRPC usa intensamente características HTTP/2 y ningún explorador proporciona el nivel de control requerido a través de solicitudes web para admitir un cliente de gRPC. Por ejemplo, los exploradores no permiten que un llamador requiera que se use HTTP/2 o que proporcione acceso a Marcos HTTP/2 subyacentes.

[gRPC-web](https://grpc.io/docs/tutorials/basic/web.html) es una tecnología adicional del equipo de gRPC que proporciona compatibilidad limitada con gRPC en el explorador. gRPC-web consta de dos partes: un cliente de JavaScript que admite todos los exploradores modernos y un proxy Web de gRPC en el servidor. El cliente web gRPC llama al proxy y el proxy se reenviará a las solicitudes de gRPC al servidor gRPC.

No todas las características de gRPC son compatibles con gRPC-Web. No se admite el streaming de cliente y bidireccional y existe compatibilidad limitada para la transmisión por secuencias del servidor.

### <a name="not-human-readable"></a>No legible

Las solicitudes de la API HTTP se envían como texto y se pueden leer y crear por parte de los usuarios.

de forma predeterminada, los mensajes gRPC se codifican con protobuf. Aunque protobuf es eficaz para el envío y la recepción, su formato binario no es legible. Protobuf requiere la descripción de la interfaz del mensaje especificada en el archivo *. proto* para deserializar correctamente. Se requieren herramientas adicionales para analizar las cargas de protobuf en la conexión y componer las solicitudes a mano.

Existen características como la [reflexión del servidor](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) y la herramienta de línea de [comandos gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) para ayudar con los mensajes protobuf binarios. Además, los mensajes protobuf admiten la [conversión a y desde JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). La conversión de JSON integrada proporciona una manera eficaz de convertir los mensajes protobuf a y desde el formato legible durante la depuración.

## <a name="alternative-framework-scenarios"></a>Escenarios de marco alternativos

Se recomiendan otros marcos de trabajo en gRPC en los siguientes escenarios:

* Las **API accesibles del explorador** &ndash; gRPC no son totalmente compatibles con el explorador. gRPC-web puede ofrecer compatibilidad con exploradores, pero tiene limitaciones e introduce un proxy de servidor.
* **Difundir la comunicación en tiempo real** &ndash; gRPC admite la comunicación en tiempo real a través de streaming, pero no existe el concepto de difusión de un mensaje a las conexiones registradas. Por ejemplo, en un escenario de salón de chat en el que se deben enviar nuevos mensajes de chat a todos los clientes del salón de chat, se requiere cada llamada gRPC para transmitir por separado los nuevos mensajes de chat al cliente. [SignalR](xref:signalr/introduction) es un marco útil para este escenario. SignalR tiene el concepto de conexiones persistentes y compatibilidad integrada para difundir mensajes.
* La **comunicación entre procesos** &ndash; un proceso debe hospedar un servidor http/2 para aceptar llamadas gRPC entrantes. En Windows, las [canalizaciones](/dotnet/standard/io/pipe-operations) de comunicación entre procesos son un método rápido y ligero de comunicación.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
