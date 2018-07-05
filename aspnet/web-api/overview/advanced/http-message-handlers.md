---
uid: web-api/overview/advanced/http-message-handlers
title: Controladores de mensajes HTTP en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: daf5bac8203beca3be0036b798188e373b02212a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394526"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>Controladores de mensajes HTTP en ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP. Controladores de mensajes se derivan de la clase abstracta **HttpMessageHandler** clase.

Normalmente, una serie de controladores de mensajes están conectados entre sí. El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador. En algún momento, la respuesta se crea y se vuelve a la cadena. Este patrón se denomina un *delegar* controlador.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Controladores de mensajes del servidor

En el servidor, la canalización de Web API usa algunos controladores de mensajes integrada:

- **Servidor HTTP** Obtiene la solicitud desde el host.
- **HttpRoutingDispatcher** envía la solicitud según la ruta.
- **HttpControllerDispatcher** envía la solicitud a un controlador Web API.

Puede agregar controladores personalizados a la canalización. Controladores de mensajes son buenos para cuestiones transversales que operan en el nivel de HTTP los mensajes (en lugar de las acciones de controlador). Por ejemplo, es posible que un controlador de mensajes:

- Leer o modificar los encabezados de solicitud.
- Agregar un encabezado de respuesta a las respuestas.
- Validar las solicitudes antes de alcanzar el controlador.

Este diagrama muestra dos controladores personalizados que se inserta en la canalización:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> En el lado cliente, HttpClient también usa controladores de mensajes. Para obtener más información, consulte [controladores de mensajes HttpClient](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Controladores de mensajes personalizado

Para escribir un controlador de mensajes personalizado, que se derivan de **System.Net.Http.DelegatingHandler** e invalidar la **SendAsync** método. Este método tiene la siguiente firma:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**. Una implementación típica hace lo siguiente:

1. Procesar el mensaje de solicitud.
2. Llame a `base.SendAsync` para enviar la solicitud al controlador interno.
3. El controlador interno devuelve un mensaje de respuesta. (Este paso es asíncrono).
4. Procesar la respuesta y devolverlo al autor de llamada.

Este es un ejemplo muy simple:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> La llamada a `base.SendAsync` es asincrónica. Si el controlador no hace ningún trabajo después de esta llamada, use la **await** palabra clave, como se muestra.


Un controlador de delegación puede omitir también el controlador interno y crear directamente la respuesta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Si la delegación de un controlador crea la respuesta sin llamar a `base.SendAsync`, la solicitud omite el resto de la canalización. Esto puede ser útil para un controlador que valida la solicitud (creación de una respuesta de error).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Agregar un controlador a la canalización

Para agregar un controlador de mensajes en el servidor, agregue el controlador a la **HttpConfiguration.MessageHandlers** colección. Si usa la plantilla "Aplicación Web de ASP.NET MVC 4" para crear el proyecto, puede hacer dentro la **WebApiConfig** clase:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Se denominan controladores de mensajes en el mismo orden que aparecen en **MessageHandlers** colección. Porque están anidadas, el mensaje de respuesta se transmite en la otra dirección. Es decir, el último controlador es el primero en recibir el mensaje de respuesta.

Tenga en cuenta que no es necesario establecer los controladores internos; el marco API Web conecta automáticamente a los controladores de mensajes.

Si estás [autohospedaje](../older-versions/self-host-a-web-api.md), cree una instancia de la **HttpSelfHostConfiguration** de clases y agregue los controladores para la **MessageHandlers** colección.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Ahora Echemos un vistazo a algunos ejemplos de los controladores de mensajes personalizados.

## <a name="example-x-http-method-override"></a>Ejemplo: X-HTTP-Method-Override

X-HTTP-Method-Override es un encabezado HTTP no estándar. Está diseñado para los clientes que no se pueden enviar ciertos tipos de solicitud HTTP, por ejemplo, PUT o DELETE. En su lugar, el cliente envía una solicitud POST y establece el encabezado X-HTTP-Method-Override para el método que desee. Por ejemplo:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Este es un controlador de mensajes que agrega compatibilidad con X-HTTP-Method-Override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

En el **SendAsync** método, el controlador comprueba si el mensaje de solicitud es una solicitud POST, y si contiene el encabezado X-HTTP-Method-Override. Si es así, valida el valor del encabezado y, a continuación, modifica el método de solicitud. Por último, el controlador llama `base.SendAsync` para pasar el mensaje al siguiente controlador.

Cuando la solicitud alcanza el **HttpControllerDispatcher** (clase), **HttpControllerDispatcher** enrutará la solicitud en función del método de solicitud actualizada.

## <a name="example-adding-a-custom-response-header"></a>Ejemplo: Agregar un encabezado de respuesta personalizado

Este es un controlador de mensajes que se agrega un encabezado personalizado a cada mensaje de respuesta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

En primer lugar, el controlador llama `base.SendAsync` para pasar la solicitud al controlador de mensajes interna. El controlador interno devuelve un mensaje de respuesta, pero lo hace de forma asincrónica utilizando un **tarea&lt;T&gt;**  objeto. No está disponible hasta que el mensaje de respuesta `base.SendAsync` finaliza de forma asincrónica.

Este ejemplo se usa el **await** palabra clave para realizar el trabajo asincrónicamente después `SendAsync` se complete. Si tiene como destino .NET Framework 4.0, utilice el **tarea**&lt;T&gt;**. ContinueWith** método:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Ejemplo: Comprobación de una clave de API

Algunos servicios web requieren que los clientes incluir una clave de API en su solicitud. El ejemplo siguiente muestra cómo un controlador de mensajes puede comprobar las solicitudes de una clave de API válida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Este controlador busca la clave de API en la cadena de consulta URI. (En este ejemplo, se supone que la clave es una cadena estática. Una implementación real probablemente usaría una validación más compleja.) Si la cadena de consulta contiene la clave, el controlador pasa la solicitud al controlador interno.

Si la solicitud no tiene una clave válida, el controlador crea un mensaje de respuesta con código de estado 403, prohibido. En este caso, el controlador no llama a `base.SendAsync`, por lo tanto, el controlador interno nunca recibe la solicitud, ni tampoco el controlador. Por tanto, el controlador puede suponer que todas las solicitudes entrantes tienen una clave de API válida.

> [!NOTE]
> Si la clave de API solo se aplica a determinadas acciones de controlador, considere la posibilidad de utilizar un filtro de acción en lugar de un controlador de mensajes. Los filtros de acción se ejecutan después de realiza el enrutamiento de URI.


## <a name="per-route-message-handlers"></a>Controladores de mensajes por ruta

Los controladores en el **HttpConfiguration.MessageHandlers** colección se aplican globalmente.

Como alternativa, puede agregar un controlador de mensajes a una ruta específica al definir la ruta:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

En este ejemplo, si el URI de solicitud coincide con "Route2", la solicitud se envía al `MessageHandler2`. El siguiente diagrama muestra la canalización para estas dos rutas:

![](http-message-handlers/_static/image4.png)

Tenga en cuenta que `MessageHandler2` reemplaza el valor predeterminado **HttpControllerDispatcher**. En este ejemplo, `MessageHandler2` crea la respuesta y las solicitudes que coinciden con "Route2" nunca se vaya a un controlador. Esto le permite reemplazar todo el mecanismo de controlador de API Web con su propio punto de conexión personalizado.

Como alternativa, puede delegar un controlador de mensajes por ruta en **HttpControllerDispatcher**, que luego se envía a un controlador.

![](http-message-handlers/_static/image5.png)

El código siguiente muestra cómo configurar esta ruta:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
