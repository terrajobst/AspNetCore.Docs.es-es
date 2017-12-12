---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Llamar a una API Web desde un cliente .NET (C#) | Documentos de Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8fcc5e7c6bc39f961931589128a7a5863482aa4e
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="0eee7-102">Llamar a una API Web desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="0eee7-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="0eee7-103">por [Mike Wasson](https://github.com/MikeWasson) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0eee7-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="0eee7-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="0eee7-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="0eee7-105">Este tutorial muestra cómo llamar a una API web desde una aplicación. NET, mediante [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="0eee7-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="0eee7-106">En este tutorial, una aplicación cliente se escribe que utiliza la API de web siguiente:</span><span class="sxs-lookup"><span data-stu-id="0eee7-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="0eee7-107">Acción</span><span class="sxs-lookup"><span data-stu-id="0eee7-107">Action</span></span> | <span data-ttu-id="0eee7-108">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="0eee7-108">HTTP method</span></span> | <span data-ttu-id="0eee7-109">URI relativo</span><span class="sxs-lookup"><span data-stu-id="0eee7-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0eee7-110">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="0eee7-110">Get a product by ID</span></span> | <span data-ttu-id="0eee7-111">GET</span><span class="sxs-lookup"><span data-stu-id="0eee7-111">GET</span></span> | <span data-ttu-id="0eee7-112">/API/products/*Id.*</span><span class="sxs-lookup"><span data-stu-id="0eee7-112">/api/products/*id*</span></span> |
| <span data-ttu-id="0eee7-113">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="0eee7-113">Create a new product</span></span> | <span data-ttu-id="0eee7-114">EXPONER</span><span class="sxs-lookup"><span data-stu-id="0eee7-114">POST</span></span> | <span data-ttu-id="0eee7-115">productos/api /</span><span class="sxs-lookup"><span data-stu-id="0eee7-115">/api/products</span></span> |
| <span data-ttu-id="0eee7-116">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="0eee7-116">Update a product</span></span> | <span data-ttu-id="0eee7-117">PUT</span><span class="sxs-lookup"><span data-stu-id="0eee7-117">PUT</span></span> | <span data-ttu-id="0eee7-118">/API/products/*Id.*</span><span class="sxs-lookup"><span data-stu-id="0eee7-118">/api/products/*id*</span></span> |
| <span data-ttu-id="0eee7-119">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="0eee7-119">Delete a product</span></span> | <span data-ttu-id="0eee7-120">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="0eee7-120">DELETE</span></span> | <span data-ttu-id="0eee7-121">/API/products/*Id.*</span><span class="sxs-lookup"><span data-stu-id="0eee7-121">/api/products/*id*</span></span> |

<span data-ttu-id="0eee7-122">Para obtener información sobre cómo implementar esta API con ASP.NET Web API, consulte [crear una API Web que admite las operaciones CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="0eee7-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="0eee7-123">Para simplificar, la aplicación de cliente en este tutorial es una aplicación de consola de Windows.</span><span class="sxs-lookup"><span data-stu-id="0eee7-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="0eee7-124">**HttpClient** también se admite para las aplicaciones de Windows Phone y tienda Windows.</span><span class="sxs-lookup"><span data-stu-id="0eee7-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="0eee7-125">Para obtener más información, vea [escritura de código de cliente de Web API para varias plataformas utilizando bibliotecas portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="0eee7-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="0eee7-126">Crear la aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="0eee7-126">Create the Console Application</span></span>

<span data-ttu-id="0eee7-127">En Visual Studio, cree una nueva aplicación de consola de Windows denominada **HttpClientSample** y pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0eee7-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="0eee7-128">El código anterior es la aplicación cliente completa.</span><span class="sxs-lookup"><span data-stu-id="0eee7-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="0eee7-129">`RunAsync`se ejecuta y se bloquea hasta que se complete.</span><span class="sxs-lookup"><span data-stu-id="0eee7-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="0eee7-130">La mayoría **HttpClient** métodos son asincrónicos, ya que realizan E/S de red.</span><span class="sxs-lookup"><span data-stu-id="0eee7-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="0eee7-131">Todas las tareas asincrónicas se realizan dentro de `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="0eee7-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="0eee7-132">Normalmente, una aplicación no bloquea el subproceso principal, pero esta aplicación no permite ninguna interacción.</span><span class="sxs-lookup"><span data-stu-id="0eee7-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="0eee7-133">Instalar las bibliotecas de cliente de la API de Web</span><span class="sxs-lookup"><span data-stu-id="0eee7-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="0eee7-134">Use el Administrador de paquetes de NuGet para instalar el paquete de Web API Client Libraries.</span><span class="sxs-lookup"><span data-stu-id="0eee7-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="0eee7-135">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="0eee7-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="0eee7-136">En la consola de administrador de paquete (PMC), escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="0eee7-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="0eee7-137">El comando anterior agrega los siguientes paquetes de NuGet al proyecto:</span><span class="sxs-lookup"><span data-stu-id="0eee7-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="0eee7-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="0eee7-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="0eee7-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="0eee7-139">Newtonsoft.Json</span></span>

<span data-ttu-id="0eee7-140">Json.NET es un marco JSON popular de alto rendimiento para. NET.</span><span class="sxs-lookup"><span data-stu-id="0eee7-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="0eee7-141">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="0eee7-141">Add a Model Class</span></span>

<span data-ttu-id="0eee7-142">Examine la `Product` clase:</span><span class="sxs-lookup"><span data-stu-id="0eee7-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="0eee7-143">Esta clase coincide con el modelo de datos utilizado por la API web.</span><span class="sxs-lookup"><span data-stu-id="0eee7-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="0eee7-144">Puede usar una aplicación **HttpClient** para leer un `Product` instancia a partir de una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0eee7-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="0eee7-145">La aplicación no tiene que escribir ningún código de deserialización.</span><span class="sxs-lookup"><span data-stu-id="0eee7-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="0eee7-146">Cree e inicialice HttpClient</span><span class="sxs-lookup"><span data-stu-id="0eee7-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="0eee7-147">Examine el método estático **HttpClient** propiedad:</span><span class="sxs-lookup"><span data-stu-id="0eee7-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="0eee7-148">**HttpClient** está diseñado para ejecutarse una vez y volver a utilizar durante toda la vida de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0eee7-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="0eee7-149">Las condiciones siguientes pueden producir en **SocketException** errores:</span><span class="sxs-lookup"><span data-stu-id="0eee7-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="0eee7-150">Crear un nuevo **HttpClient** instancia por solicitud.</span><span class="sxs-lookup"><span data-stu-id="0eee7-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="0eee7-151">Servidor con mucha carga.</span><span class="sxs-lookup"><span data-stu-id="0eee7-151">Server under heavy load.</span></span>

<span data-ttu-id="0eee7-152">Crear un nuevo **HttpClient** instancia por solicitud puede agotar los sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="0eee7-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="0eee7-153">El siguiente código inicializa el **HttpClient** instancia:</span><span class="sxs-lookup"><span data-stu-id="0eee7-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="0eee7-154">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="0eee7-154">The preceding code:</span></span>

* <span data-ttu-id="0eee7-155">Establece el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0eee7-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="0eee7-156">Cambiar el número de puerto para el puerto que se utiliza en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0eee7-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="0eee7-157">La aplicación no funcionará a menos que se utiliza el puerto para la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0eee7-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="0eee7-158">Establece el encabezado Accept en "application/json".</span><span class="sxs-lookup"><span data-stu-id="0eee7-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="0eee7-159">Establecer este encabezado indica al servidor para enviar datos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="0eee7-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="0eee7-160">Envía una solicitud GET para recuperar un recurso</span><span class="sxs-lookup"><span data-stu-id="0eee7-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="0eee7-161">El código siguiente envía una solicitud GET para un producto:</span><span class="sxs-lookup"><span data-stu-id="0eee7-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="0eee7-162">El **GetAsync** método envía la solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="0eee7-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="0eee7-163">Cuando se completa el método, devuelve un **HttpResponseMessage** que contiene la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0eee7-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="0eee7-164">Si el código de estado en la respuesta es un código correcto, el cuerpo de respuesta contiene la representación JSON de un producto.</span><span class="sxs-lookup"><span data-stu-id="0eee7-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="0eee7-165">Llame a **ReadAsAsync** para deserializar la carga de JSON para un `Product` instancia.</span><span class="sxs-lookup"><span data-stu-id="0eee7-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="0eee7-166">El **ReadAsAsync** método es asincrónico porque el cuerpo de respuesta puede ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="0eee7-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="0eee7-167">**HttpClient** no produce una excepción cuando la respuesta HTTP contiene un código de error.</span><span class="sxs-lookup"><span data-stu-id="0eee7-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="0eee7-168">En su lugar, el **IsSuccessStatusCode** propiedad es **false** si el estado es un código de error.</span><span class="sxs-lookup"><span data-stu-id="0eee7-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="0eee7-169">Si desea tratar los códigos de error HTTP como excepciones, llame a [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) en el objeto de respuesta.</span><span class="sxs-lookup"><span data-stu-id="0eee7-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="0eee7-170">`EnsureSuccessStatusCode`produce una excepción si el código de estado está fuera del intervalo 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="0eee7-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="0eee7-171">Tenga en cuenta que **HttpClient** puede producir excepciones por otras razones &mdash; por ejemplo, si se agota el tiempo de espera de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="0eee7-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="0eee7-172">Formateadores de tipo de medio para deserializar</span><span class="sxs-lookup"><span data-stu-id="0eee7-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="0eee7-173">Cuando **ReadAsAsync** se llama sin parámetros, utiliza un conjunto predeterminado de *formateadores de contenido multimedia* para leer el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="0eee7-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="0eee7-174">Los formateadores de manera predeterminada admiten datos codificados de dirección url de formulario, XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="0eee7-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="0eee7-175">En lugar de utilizar los formateadores de manera predeterminada, puede proporcionar una lista de formateadores para la **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="0eee7-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="0eee7-176">Mediante una una lista de formateadores es útil si tiene un formateador de tipo de medio personalizado:</span><span class="sxs-lookup"><span data-stu-id="0eee7-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="0eee7-177">Para obtener más información, vea [formateadores de contenido multimedia en ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="0eee7-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="0eee7-178">Enviar una solicitud POST para crear un recurso</span><span class="sxs-lookup"><span data-stu-id="0eee7-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="0eee7-179">El código siguiente envía una solicitud POST que contiene un `Product` instancia en formato JSON:</span><span class="sxs-lookup"><span data-stu-id="0eee7-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="0eee7-180">El **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="0eee7-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="0eee7-181">Serializa un objeto a JSON.</span><span class="sxs-lookup"><span data-stu-id="0eee7-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="0eee7-182">Envía la carga de JSON en una solicitud POST.</span><span class="sxs-lookup"><span data-stu-id="0eee7-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="0eee7-183">Si la solicitud se realiza correctamente:</span><span class="sxs-lookup"><span data-stu-id="0eee7-183">If the request succeeds:</span></span>

* <span data-ttu-id="0eee7-184">Debe devolver respuesta 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="0eee7-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="0eee7-185">La respuesta debe incluir la dirección URL de los recursos creados en el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="0eee7-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="0eee7-186">Enviar una solicitud PUT para actualizar un recurso</span><span class="sxs-lookup"><span data-stu-id="0eee7-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="0eee7-187">El código siguiente envía una solicitud PUT para actualizar un producto:</span><span class="sxs-lookup"><span data-stu-id="0eee7-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="0eee7-188">El **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, excepto en que envía una solicitud PUT en lugar de POST.</span><span class="sxs-lookup"><span data-stu-id="0eee7-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="0eee7-189">Enviar una solicitud de eliminación para eliminar un recurso</span><span class="sxs-lookup"><span data-stu-id="0eee7-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="0eee7-190">El código siguiente envía una solicitud de eliminación para eliminar un producto:</span><span class="sxs-lookup"><span data-stu-id="0eee7-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="0eee7-191">Como GET, una solicitud de eliminación no tiene un cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="0eee7-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="0eee7-192">No es necesario especificar el formato JSON o XML con la eliminación.</span><span class="sxs-lookup"><span data-stu-id="0eee7-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="0eee7-193">Probar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0eee7-193">Test the sample</span></span>

<span data-ttu-id="0eee7-194">Para probar la aplicación cliente:</span><span class="sxs-lookup"><span data-stu-id="0eee7-194">To test the client app:</span></span>

1. <span data-ttu-id="0eee7-195">[Descargar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) y ejecutar la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0eee7-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/samples/server) and run the server app.</span></span> <span data-ttu-id="0eee7-196">[Instrucciones de descarga](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="0eee7-196">[Download instructions](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="0eee7-197">Comprobar que funciona la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0eee7-197">Verify the server app is working.</span></span> <span data-ttu-id="0eee7-198">Para exaxmple, `http://localhost:64195/api/products` debe devolver una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="0eee7-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="0eee7-199">Establecer el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="0eee7-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="0eee7-200">Cambiar el número de puerto para el puerto que se utiliza en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="0eee7-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="0eee7-201">Ejecutar la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="0eee7-201">Run the client app.</span></span> <span data-ttu-id="0eee7-202">Se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="0eee7-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
