---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Llamar a una API Web desde un cliente .NET (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838745"
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="2a180-102">Llamar a una API Web desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="2a180-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="2a180-103">por [Mike Wasson](https://github.com/MikeWasson) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2a180-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a180-104">[Descargue el proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="2a180-104">[Download Completed Project](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="2a180-105">[Instrucciones de descarga](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2a180-105">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="2a180-106">Este tutorial muestra cómo llamar a una API web desde una aplicación de .NET mediante [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="2a180-106">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="2a180-107">En este tutorial, una aplicación cliente se escribe que consume la API web siguiente:</span><span class="sxs-lookup"><span data-stu-id="2a180-107">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="2a180-108">Acción</span><span class="sxs-lookup"><span data-stu-id="2a180-108">Action</span></span> | <span data-ttu-id="2a180-109">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="2a180-109">HTTP method</span></span> | <span data-ttu-id="2a180-110">URI relativo</span><span class="sxs-lookup"><span data-stu-id="2a180-110">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2a180-111">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="2a180-111">Get a product by ID</span></span> | <span data-ttu-id="2a180-112">GET</span><span class="sxs-lookup"><span data-stu-id="2a180-112">GET</span></span> | <span data-ttu-id="2a180-113">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="2a180-113">/api/products/*id*</span></span> |
| <span data-ttu-id="2a180-114">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="2a180-114">Create a new product</span></span> | <span data-ttu-id="2a180-115">EXPONER</span><span class="sxs-lookup"><span data-stu-id="2a180-115">POST</span></span> | <span data-ttu-id="2a180-116">productos/api /</span><span class="sxs-lookup"><span data-stu-id="2a180-116">/api/products</span></span> |
| <span data-ttu-id="2a180-117">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="2a180-117">Update a product</span></span> | <span data-ttu-id="2a180-118">PUT</span><span class="sxs-lookup"><span data-stu-id="2a180-118">PUT</span></span> | <span data-ttu-id="2a180-119">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="2a180-119">/api/products/*id*</span></span> |
| <span data-ttu-id="2a180-120">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="2a180-120">Delete a product</span></span> | <span data-ttu-id="2a180-121">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="2a180-121">DELETE</span></span> | <span data-ttu-id="2a180-122">/API/productos/*Id.*</span><span class="sxs-lookup"><span data-stu-id="2a180-122">/api/products/*id*</span></span> |

<span data-ttu-id="2a180-123">Para obtener información sobre cómo implementar esta API con ASP.NET Web API, consulte [crear una API Web que admite operaciones de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="2a180-123">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="2a180-124">Por motivos de simplicidad, la aplicación cliente en este tutorial es una aplicación de consola de Windows.</span><span class="sxs-lookup"><span data-stu-id="2a180-124">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="2a180-125">**HttpClient** también se admite para las aplicaciones de Windows Phone y Windows Store.</span><span class="sxs-lookup"><span data-stu-id="2a180-125">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="2a180-126">Para obtener más información, consulte [escribir el código de cliente de API de Web para varias plataformas utilizando las bibliotecas portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="2a180-126">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="2a180-127">Crear la aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="2a180-127">Create the Console Application</span></span>

<span data-ttu-id="2a180-128">En Visual Studio, cree una nueva aplicación de consola de Windows denominada **HttpClientSample** y pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2a180-128">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="2a180-129">El código anterior es la aplicación cliente completa.</span><span class="sxs-lookup"><span data-stu-id="2a180-129">The preceding code is the complete client app.</span></span>

<span data-ttu-id="2a180-130">`RunAsync` ejecuciones y bloques hasta que se complete.</span><span class="sxs-lookup"><span data-stu-id="2a180-130">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="2a180-131">La mayoría **HttpClient** métodos son asincrónicos, ya que realizan operaciones de E/S de red.</span><span class="sxs-lookup"><span data-stu-id="2a180-131">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="2a180-132">Todas las tareas asincrónicas se realizan dentro de `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a180-132">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="2a180-133">Normalmente, una aplicación no bloquea el subproceso principal, pero esta aplicación no permite ninguna interacción.</span><span class="sxs-lookup"><span data-stu-id="2a180-133">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="2a180-134">Instalar las bibliotecas de cliente de API Web</span><span class="sxs-lookup"><span data-stu-id="2a180-134">Install the Web API Client Libraries</span></span>

<span data-ttu-id="2a180-135">Use el Administrador de paquetes de NuGet para instalar el paquete de bibliotecas de cliente de API Web.</span><span class="sxs-lookup"><span data-stu-id="2a180-135">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="2a180-136">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="2a180-136">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="2a180-137">En la consola de administrador de paquetes (PMC), escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2a180-137">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="2a180-138">El comando anterior agrega los siguientes paquetes NuGet al proyecto:</span><span class="sxs-lookup"><span data-stu-id="2a180-138">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="2a180-139">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="2a180-139">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="2a180-140">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="2a180-140">Newtonsoft.Json</span></span>

<span data-ttu-id="2a180-141">Json.NET es un popular marco JSON de alto rendimiento para. NET.</span><span class="sxs-lookup"><span data-stu-id="2a180-141">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="2a180-142">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="2a180-142">Add a Model Class</span></span>

<span data-ttu-id="2a180-143">Examine el `Product` clase:</span><span class="sxs-lookup"><span data-stu-id="2a180-143">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="2a180-144">Esta clase coincide con el modelo de datos utilizado por la API web.</span><span class="sxs-lookup"><span data-stu-id="2a180-144">This class matches the data model used by the web API.</span></span> <span data-ttu-id="2a180-145">Puede usar una aplicación **HttpClient** para leer un `Product` instancia de una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a180-145">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="2a180-146">La aplicación no tiene que escribir ningún código de deserialización.</span><span class="sxs-lookup"><span data-stu-id="2a180-146">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="2a180-147">Crear e inicializar HttpClient</span><span class="sxs-lookup"><span data-stu-id="2a180-147">Create and Initialize HttpClient</span></span>

<span data-ttu-id="2a180-148">Examinar estático **HttpClient** propiedad:</span><span class="sxs-lookup"><span data-stu-id="2a180-148">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="2a180-149">**HttpClient** está diseñado para ejecutarse una vez y volver a usar durante la vida de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2a180-149">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="2a180-150">Las condiciones siguientes pueden producir en **SocketException** errores:</span><span class="sxs-lookup"><span data-stu-id="2a180-150">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="2a180-151">Crear un nuevo **HttpClient** instancia por solicitud.</span><span class="sxs-lookup"><span data-stu-id="2a180-151">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="2a180-152">Servidor con mucha carga.</span><span class="sxs-lookup"><span data-stu-id="2a180-152">Server under heavy load.</span></span>

<span data-ttu-id="2a180-153">Crear un nuevo **HttpClient** instancia por solicitud puede agotar los sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="2a180-153">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="2a180-154">El siguiente código inicializa el **HttpClient** instancia:</span><span class="sxs-lookup"><span data-stu-id="2a180-154">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="2a180-155">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="2a180-155">The preceding code:</span></span>

* <span data-ttu-id="2a180-156">Establece el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a180-156">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="2a180-157">Cambiar el número de puerto para el puerto usado en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2a180-157">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="2a180-158">La aplicación no funcionará a menos que se usa el puerto para la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2a180-158">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="2a180-159">Establece el encabezado Accept en "application/json".</span><span class="sxs-lookup"><span data-stu-id="2a180-159">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="2a180-160">Establecer este encabezado indica al servidor para enviar datos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="2a180-160">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="2a180-161">Envía una solicitud GET para recuperar un recurso</span><span class="sxs-lookup"><span data-stu-id="2a180-161">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="2a180-162">El código siguiente envía una solicitud GET para un producto:</span><span class="sxs-lookup"><span data-stu-id="2a180-162">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="2a180-163">El **GetAsync** método envía la solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="2a180-163">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="2a180-164">Cuando finalice el método, devuelve un **HttpResponseMessage** que contiene la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a180-164">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="2a180-165">Si el código de estado en la respuesta es un código correcto, el cuerpo de respuesta contiene la representación JSON de un producto.</span><span class="sxs-lookup"><span data-stu-id="2a180-165">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="2a180-166">Llame a **ReadAsAsync** para deserializar la carga de JSON para un `Product` instancia.</span><span class="sxs-lookup"><span data-stu-id="2a180-166">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="2a180-167">El **ReadAsAsync** método es asincrónico porque el cuerpo de respuesta puede ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="2a180-167">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="2a180-168">**HttpClient** no produce una excepción cuando la respuesta HTTP contiene un código de error.</span><span class="sxs-lookup"><span data-stu-id="2a180-168">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="2a180-169">En su lugar, el **IsSuccessStatusCode** propiedad es **false** si el estado es un código de error.</span><span class="sxs-lookup"><span data-stu-id="2a180-169">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="2a180-170">Si lo prefiere tratar los códigos de error HTTP como excepciones, llame a [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) en el objeto de respuesta.</span><span class="sxs-lookup"><span data-stu-id="2a180-170">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="2a180-171">`EnsureSuccessStatusCode` produce una excepción si el código de estado está fuera del intervalo 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="2a180-171">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="2a180-172">Tenga en cuenta que **HttpClient** puede producir excepciones por otras razones &mdash; por ejemplo, si la solicitud agota el tiempo.</span><span class="sxs-lookup"><span data-stu-id="2a180-172">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="2a180-173">Formateadores de tipo de medio para deserializar</span><span class="sxs-lookup"><span data-stu-id="2a180-173">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="2a180-174">Cuando **ReadAsAsync** se llama sin parámetros, usa un conjunto predeterminado de *formateadores de contenido multimedia* para leer el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="2a180-174">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="2a180-175">Los formateadores predeterminados admiten JSON, XML y datos codificados de dirección url de formulario.</span><span class="sxs-lookup"><span data-stu-id="2a180-175">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="2a180-176">En lugar de utilizar los formateadores de forma predeterminada, puede proporcionar una lista de formateadores para la **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="2a180-176">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="2a180-177">Con una lista de formateadores es útil si tiene un formateador de tipo de medio personalizado:</span><span class="sxs-lookup"><span data-stu-id="2a180-177">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="2a180-178">Para obtener más información, consulte [formateadores de contenido multimedia en ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="2a180-178">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="2a180-179">Enviar una solicitud POST para crear un recurso</span><span class="sxs-lookup"><span data-stu-id="2a180-179">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="2a180-180">El código siguiente envía una solicitud POST que contiene un `Product` instancia en formato JSON:</span><span class="sxs-lookup"><span data-stu-id="2a180-180">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="2a180-181">El **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="2a180-181">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="2a180-182">Serializa un objeto a JSON.</span><span class="sxs-lookup"><span data-stu-id="2a180-182">Serializes an object to JSON.</span></span>
* <span data-ttu-id="2a180-183">Envía la carga de JSON en una solicitud POST.</span><span class="sxs-lookup"><span data-stu-id="2a180-183">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="2a180-184">Si la solicitud se realiza correctamente:</span><span class="sxs-lookup"><span data-stu-id="2a180-184">If the request succeeds:</span></span>

* <span data-ttu-id="2a180-185">Debe devolver una respuesta de 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="2a180-185">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="2a180-186">La respuesta debe incluir la dirección URL de los recursos creados en el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="2a180-186">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="2a180-187">Enviar una solicitud PUT para actualizar un recurso</span><span class="sxs-lookup"><span data-stu-id="2a180-187">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="2a180-188">El código siguiente envía una solicitud PUT para actualizar un producto:</span><span class="sxs-lookup"><span data-stu-id="2a180-188">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="2a180-189">El **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, excepto en que envía una solicitud PUT en lugar de POST.</span><span class="sxs-lookup"><span data-stu-id="2a180-189">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="2a180-190">Enviar una solicitud DELETE para eliminar un recurso</span><span class="sxs-lookup"><span data-stu-id="2a180-190">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="2a180-191">El código siguiente envía una solicitud DELETE para eliminar un producto:</span><span class="sxs-lookup"><span data-stu-id="2a180-191">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="2a180-192">Una solicitud de eliminación, como GET, no tiene un cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="2a180-192">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="2a180-193">No es necesario especificar el formato JSON o XML con la eliminación.</span><span class="sxs-lookup"><span data-stu-id="2a180-193">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="2a180-194">Probar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="2a180-194">Test the sample</span></span>

<span data-ttu-id="2a180-195">Para probar la aplicación cliente:</span><span class="sxs-lookup"><span data-stu-id="2a180-195">To test the client app:</span></span>

1. <span data-ttu-id="2a180-196">[Descargar](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) y ejecutar la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2a180-196">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="2a180-197">[Instrucciones de descarga](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2a180-197">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="2a180-198">Comprobar que funciona la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2a180-198">Verify the server app is working.</span></span> <span data-ttu-id="2a180-199">Para exaxmple, `http://localhost:64195/api/products` debe devolver una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="2a180-199">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="2a180-200">Establecer el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="2a180-200">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="2a180-201">Cambiar el número de puerto para el puerto usado en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="2a180-201">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="2a180-202">Ejecute la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="2a180-202">Run the client app.</span></span> <span data-ttu-id="2a180-203">Se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="2a180-203">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
