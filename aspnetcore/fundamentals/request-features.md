---
title: "Características de la solicitud de ASP.NET Core"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: a10aefe3819fb03019575c36274dd164faf7086c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="request-features-in-aspnet-core"></a>Características de la solicitud de ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Detalles de implementación de servidor Web relacionados con las solicitudes HTTP y las respuestas se definen en las interfaces. Estas interfaces son usadas por las implementaciones del servidor y middleware para crear y modificar la canalización de hospedaje de la aplicación.

## <a name="feature-interfaces"></a>Interfaces de característica

Define el número de interfaces de la característica HTTP en ASP.NET Core `Microsoft.AspNetCore.Http.Features` servidores que se usan para identificar las características que admiten. Las siguientes interfaces de característica controlen las solicitudes y devuelven respuestas:

`IHttpRequestFeature`Define la estructura de una solicitud HTTP, incluidos el protocolo, ruta de acceso, cadena de consulta, encabezados y el cuerpo.

`IHttpResponseFeature`Define la estructura de una respuesta HTTP, incluido el código de estado, los encabezados y cuerpo de la respuesta.

`IHttpAuthenticationFeature`Define la compatibilidad para identificar a los usuarios en función de un `ClaimsPrincipal` y la especificación de un controlador de autenticación.

`IHttpUpgradeFeature`Define la compatibilidad con [HTTP actualizaciones](https://tools.ietf.org/html/rfc2616.html#section-14.42), que permiten al cliente para especificar que otros protocolos le gustaría utilizar si el servidor que desea cambiar los protocolos.

`IHttpBufferingFeature`Define métodos para deshabilitar el almacenamiento en búfer de solicitudes o respuestas.

`IHttpConnectionFeature`Define propiedades de puertos y las direcciones locales y remotas.

`IHttpRequestLifetimeFeature`Define la compatibilidad para anular las conexiones o detectar si una solicitud ha finalizado antes de tiempo, tal como una desconexión de cliente.

`IHttpSendFileFeature`Define un método para enviar archivos de forma asincrónica.

`IHttpWebSocketFeature`Define una API para admitir los sockets web.

`IHttpRequestIdentifierFeature`Agrega una propiedad que se puede implementar para identificar de forma exclusiva las solicitudes.

`ISessionFeature`Define `ISessionFactory` y `ISession` abstracciones para admitir las sesiones de usuario.

`ITlsConnectionFeature`Define una API para recuperar los certificados de cliente.

`ITlsTokenBindingFeature`Define métodos para trabajar con parámetros de enlace del token TLS.

> [!NOTE]
> `ISessionFeature`no es una característica de servidor, pero se implementa mediante el `SessionMiddleware` (consulte [administrar el estado de aplicación](app-state.md)).

## <a name="feature-collections"></a>Colecciones de característica

El `Features` propiedad de `HttpContext` proporciona una interfaz para obtener y establecer las características HTTP disponibles para la solicitud actual. Puesto que la colección de características es mutable incluso en el contexto de una solicitud, middleware puede usarse para modificar la colección y agregar compatibilidad con características adicionales.

## <a name="middleware-and-request-features"></a>Características de middleware y solicitud

Mientras que los servidores son responsables de crear la colección de características, middleware puede agregar a esta colección y utilizar las características de la colección. Por ejemplo, el `StaticFileMiddleware` tiene acceso a la `IHttpSendFileFeature` característica. Si la característica no existe, se utiliza para enviar el archivo estático solicitado desde su ruta de acceso física. En caso contrario, se usa un método alternativo más lento para enviar el archivo. Si está disponible, el `IHttpSendFileFeature` permite al sistema operativo abrir el archivo y realizar una copia de modo kernel directa a la tarjeta de red.

Además, puede agregar middleware a la colección de características establecida por el servidor. Las características existentes incluso pueden reemplazarse por el middleware, lo que permite el middleware para aumentar la funcionalidad del servidor. Características agregadas a la colección están disponibles inmediatamente para otro middleware o la propia aplicación subyacente más adelante en la canalización de solicitudes.

Mediante la combinación de las implementaciones de servidor personalizado y mejoras de middleware específico, se puede construir el conjunto de características que requiere una aplicación. Esto permite que falta características se agreguen sin necesidad de un cambio en el servidor y se asegura de que se exponen solo la mínima cantidad de características, lo que permite limitar ataque expuesta área y mejorar el rendimiento.

## <a name="summary"></a>Resumen

Interfaces de característica definen características específicas de HTTP que puede admitir una solicitud determinada. Servidores definen colecciones de características y el conjunto inicial de las características admitidas por el servidor, pero middleware puede usarse para mejorar estas características.

## <a name="additional-resources"></a>Recursos adicionales

* [Servidores](servers/index.md)

* [Software intermedio](middleware.md)

* [Abrir la interfaz Web para .NET (OWIN)](owin.md)
