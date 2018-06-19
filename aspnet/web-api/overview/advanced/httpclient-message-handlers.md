---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes de HttpClient en ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506794"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Controladores de mensajes de HttpClient en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

A *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP.

Normalmente, una serie de controladores de mensajes están encadenados entre sí. El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y le ofrece la solicitud al siguiente controlador. En algún momento, la respuesta se crea y vuelve a la cadena. Este patrón se denomina un *delegar* controlador.

![](httpclient-message-handlers/_static/image1.png)

En el lado del cliente, el **HttpClient** clase utiliza un controlador de mensajes para procesar solicitudes. El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y obtenga la respuesta del servidor. Puede insertar controladores de mensajes personalizados en la canalización de cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API también utiliza los controladores de mensajes en el servidor. Para obtener más información, consulte [controladores de mensajes HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Controladores de mensajes personalizado

Para escribir un controlador de mensajes personalizado, derive de **System.Net.Http.DelegatingHandler** e invalide el **SendAsync** método. Aquí se muestra la signatura de método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**. Una implementación típica hace lo siguiente:

1. Procesar el mensaje de solicitud.
2. Llame a `base.SendAsync` para enviar la solicitud al controlador interno.
3. El controlador interno devuelve un mensaje de respuesta. (Este paso es asíncrono).
4. Procesar la respuesta y devolver al llamador.

En el ejemplo siguiente se muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud de salida:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La llamada a `base.SendAsync` es asincrónica. Si el controlador no hace nada después de esta llamada, use la **await** palabra clave que se va a reanudar la ejecución una vez finalizado el método. En el ejemplo siguiente se muestra un controlador que registra los códigos de error. El registro de sí mismo no es muy interesante, pero en el ejemplo se muestra cómo obtener la respuesta dentro del controlador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Agregar controladores de mensajes a la canalización de cliente

Para agregar controladores personalizados para **HttpClient**, use la **HttpClientFactory.Create** método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Se denominan controladores de mensajes en el orden en que se pasan en el **Create** método. Porque los controladores están anidados, el mensaje de respuesta se transmite en la otra dirección. Es decir, el último controlador es el primero que se va a obtener el mensaje de respuesta.
