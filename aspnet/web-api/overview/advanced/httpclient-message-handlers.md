---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes HttpClient en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831377"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Controladores de mensajes HttpClient en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP.

Normalmente, una serie de controladores de mensajes están conectados entre sí. El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador. En algún momento, la respuesta se crea y se vuelve a la cadena. Este patrón se denomina un *delegar* controlador.

![](httpclient-message-handlers/_static/image1.png)

En el lado cliente, el **HttpClient** clase utiliza un controlador de mensajes para procesar las solicitudes. El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y se obtiene la respuesta del servidor. Puede insertar controladores de mensajes personalizados en la canalización de cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API también usa controladores de mensajes en el servidor. Para obtener más información, consulte [controladores de mensajes HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Controladores de mensajes personalizado

Para escribir un controlador de mensajes personalizado, que se derivan de **System.Net.Http.DelegatingHandler** e invalidar la **SendAsync** método. Esta es la firma de método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**. Una implementación típica hace lo siguiente:

1. Procesar el mensaje de solicitud.
2. Llame a `base.SendAsync` para enviar la solicitud al controlador interno.
3. El controlador interno devuelve un mensaje de respuesta. (Este paso es asíncrono).
4. Procesar la respuesta y devolverlo al autor de llamada.

El ejemplo siguiente muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud de salida:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La llamada a `base.SendAsync` es asincrónica. Si el controlador no hace ningún trabajo después de esta llamada, use la **await** palabra clave para reanudar la ejecución después de completarse el método. El ejemplo siguiente muestra un controlador que registra los códigos de error. El registro en sí mismo no es muy interesante, pero el ejemplo muestra cómo obtener la respuesta dentro del controlador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Agregar controladores de mensajes a la canalización de cliente

Para agregar controladores personalizados para **HttpClient**, utilice el **HttpClientFactory.Create** método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Se denominan controladores de mensajes en el orden en que se pasan en el **Create** método. Porque los controladores están anidados, transmite el mensaje de respuesta en la otra dirección. Es decir, el último controlador es el primero en recibir el mensaje de respuesta.
