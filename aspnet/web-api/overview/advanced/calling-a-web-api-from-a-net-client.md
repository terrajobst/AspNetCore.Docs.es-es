---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Llamar a una API Web desde un cliente .NET (C#) | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: a243eeb982ba581e237263c4e31e130d634aff0e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a>Llamar a una API Web desde un cliente .NET (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[Descargar el proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Este tutorial muestra cómo llamar a una API web desde una aplicación. NET, mediante [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

En este tutorial, una aplicación cliente se escribe que utiliza la API de web siguiente:

| Acción | Método HTTP | URI relativo |
| --- | --- | --- |
| Obtener un producto por Id. | GET | /api/products/*id* |
| Crear un nuevo producto | EXPONER | productos/api / |
| Actualizar un producto | PUT | /api/products/*id* |
| Eliminar un producto | SUPRIMIR | /api/products/*id* |

Para obtener información sobre cómo implementar esta API con ASP.NET Web API, consulte [crear una API Web que admite las operaciones CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Para simplificar, la aplicación de cliente en este tutorial es una aplicación de consola de Windows. **HttpClient** también se admite para las aplicaciones de Windows Phone y tienda Windows. Para obtener más información, vea [escritura de código de cliente de Web API para varias plataformas utilizando bibliotecas portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Crear la aplicación de consola

En Visual Studio, cree una nueva aplicación de consola de Windows denominada **HttpClientSample** y pegue el código siguiente:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

El código anterior es la aplicación cliente completa.

`RunAsync` se ejecuta y se bloquea hasta que se complete. La mayoría **HttpClient** métodos son asincrónicos, ya que realizan E/S de red. Todas las tareas asincrónicas se realizan dentro de `RunAsync`. Normalmente, una aplicación no bloquea el subproceso principal, pero esta aplicación no permite ninguna interacción.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalar las bibliotecas de cliente de la API de Web

Use el Administrador de paquetes de NuGet para instalar el paquete de Web API Client Libraries.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. En la consola de administrador de paquete (PMC), escriba el siguiente comando:

`Install-Package Microsoft.AspNet.WebApi.Client`

El comando anterior agrega los siguientes paquetes de NuGet al proyecto:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET es un marco JSON popular de alto rendimiento para. NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Agregar una clase de modelo

Examine la `Product` clase:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Esta clase coincide con el modelo de datos utilizado por la API web. Puede usar una aplicación **HttpClient** para leer un `Product` instancia a partir de una respuesta HTTP. La aplicación no tiene que escribir ningún código de deserialización.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Cree e inicialice HttpClient

Examine el método estático **HttpClient** propiedad:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** está diseñado para ejecutarse una vez y volver a utilizar durante toda la vida de una aplicación. Las condiciones siguientes pueden producir en **SocketException** errores:

* Crear un nuevo **HttpClient** instancia por solicitud.
* Servidor con mucha carga.

Crear un nuevo **HttpClient** instancia por solicitud puede agotar los sockets disponibles.

El siguiente código inicializa el **HttpClient** instancia:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

El código anterior:

* Establece el URI base para las solicitudes HTTP. Cambiar el número de puerto para el puerto que se utiliza en la aplicación de servidor. La aplicación no funcionará a menos que se utiliza el puerto para la aplicación de servidor.
* Establece el encabezado Accept en "application/json". Establecer este encabezado indica al servidor para enviar datos en formato JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Envía una solicitud GET para recuperar un recurso

El código siguiente envía una solicitud GET para un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

El **GetAsync** método envía la solicitud HTTP GET. Cuando se completa el método, devuelve un **HttpResponseMessage** que contiene la respuesta HTTP. Si el código de estado en la respuesta es un código correcto, el cuerpo de respuesta contiene la representación JSON de un producto. Llame a **ReadAsAsync** para deserializar la carga de JSON para un `Product` instancia. El **ReadAsAsync** método es asincrónico porque el cuerpo de respuesta puede ser arbitrariamente grande.

**HttpClient** no produce una excepción cuando la respuesta HTTP contiene un código de error. En su lugar, el **IsSuccessStatusCode** propiedad es **false** si el estado es un código de error. Si desea tratar los códigos de error HTTP como excepciones, llame a [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) en el objeto de respuesta. `EnsureSuccessStatusCode` produce una excepción si el código de estado está fuera del intervalo 200&ndash;299. Tenga en cuenta que **HttpClient** puede producir excepciones por otras razones &mdash; por ejemplo, si se agota el tiempo de espera de la solicitud.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formateadores de tipo de medio para deserializar

Cuando **ReadAsAsync** se llama sin parámetros, utiliza un conjunto predeterminado de *formateadores de contenido multimedia* para leer el cuerpo de respuesta. Los formateadores de manera predeterminada admiten datos codificados de dirección url de formulario, XML y JSON.

En lugar de utilizar los formateadores de manera predeterminada, puede proporcionar una lista de formateadores para la **ReadAsAsync** método.  Con una lista de formateadores es útil si tiene un formateador de tipo de medio personalizado:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Para obtener más información, vea [formateadores de contenido multimedia en ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Enviar una solicitud POST para crear un recurso

El código siguiente envía una solicitud POST que contiene un `Product` instancia en formato JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

El **PostAsJsonAsync** método:

* Serializa un objeto a JSON.
* Envía la carga de JSON en una solicitud POST.

Si la solicitud se realiza correctamente:

* Debe devolver respuesta 201 (creado).
* La respuesta debe incluir la dirección URL de los recursos creados en el encabezado Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Enviar una solicitud PUT para actualizar un recurso

El código siguiente envía una solicitud PUT para actualizar un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

El **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, excepto en que envía una solicitud PUT en lugar de POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Enviar una solicitud de eliminación para eliminar un recurso

El código siguiente envía una solicitud de eliminación para eliminar un producto:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Como GET, una solicitud de eliminación no tiene un cuerpo de solicitud. No es necesario especificar el formato JSON o XML con la eliminación.

## <a name="test-the-sample"></a>Probar el ejemplo

Para probar la aplicación cliente:

1. [Descargar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) y ejecutar la aplicación de servidor. [Instrucciones de descarga](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample). Comprobar que funciona la aplicación de servidor. Para exaxmple, `http://localhost:64195/api/products` debe devolver una lista de productos.
2. Establecer el URI base para las solicitudes HTTP. Cambiar el número de puerto para el puerto que se utiliza en la aplicación de servidor.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Ejecutar la aplicación cliente. Se produce el siguiente resultado:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
