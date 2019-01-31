---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitación de solicitudes entre orígenes en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Muestra cómo admitir el uso compartido de recursos entre orígenes (CORS) en ASP.NET Web API.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97a0027194b019b09e220493dcb593e682027fe3
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428452"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="98108-103">Habilitar solicitudes entre orígenes en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="98108-103">Enable cross-origin requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="98108-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="98108-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="98108-105">La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio.</span><span class="sxs-lookup"><span data-stu-id="98108-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="98108-106">Esta restricción se denomina *"directiva de mismo origen"* e impide que un sitio malintencionado lea los datos confidenciales desde otro sitio.</span><span class="sxs-lookup"><span data-stu-id="98108-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="98108-107">Sin embargo, a veces es posible que desee permitir que otros sitios de llamada a la API web.</span><span class="sxs-lookup"><span data-stu-id="98108-107">However, sometimes you might want to let other sites call your web API.</span></span>
>
> <span data-ttu-id="98108-108">El [uso compartido de recursos entre orígenes](http://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite que un servidor modere la directiva de mismo origen.</span><span class="sxs-lookup"><span data-stu-id="98108-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="98108-109">Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras.</span><span class="sxs-lookup"><span data-stu-id="98108-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="98108-110">CORS es más seguro y flexible que técnicas anteriores como [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="98108-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="98108-111">Este tutorial muestra cómo se habilita la CORS en la aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="98108-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
>
> ## <a name="software-used-in-the-tutorial"></a><span data-ttu-id="98108-112">Software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="98108-112">Software used in the tutorial</span></span>
>
> - [<span data-ttu-id="98108-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98108-113">Visual Studio</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="98108-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="98108-114">Web API 2.2</span></span>

## <a name="introduction"></a><span data-ttu-id="98108-115">Introducción</span><span class="sxs-lookup"><span data-stu-id="98108-115">Introduction</span></span>

<span data-ttu-id="98108-116">Este tutorial se muestra la compatibilidad con CORS en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="98108-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="98108-117">Comenzaremos creando dos proyectos ASP.NET: uno llamado "WebService", que hospeda un controlador Web API, y otra llamada "WebClient", que llama al servicio Web.</span><span class="sxs-lookup"><span data-stu-id="98108-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="98108-118">Dado que las dos aplicaciones se hospedan en dominios diferentes, una solicitud de AJAX de WebClient para el servicio Web es una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="98108-119">¿Qué es el "mismo origen"?</span><span class="sxs-lookup"><span data-stu-id="98108-119">What is "same origin"?</span></span>

<span data-ttu-id="98108-120">Dos direcciones URL tienen el mismo origen si tienen puertos, hosts y esquemas idénticos.</span><span class="sxs-lookup"><span data-stu-id="98108-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="98108-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="98108-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="98108-122">Estas dos direcciones URL tienen el mismo origen:</span><span class="sxs-lookup"><span data-stu-id="98108-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="98108-123">Estas direcciones URL tienen orígenes diferentes de los dos anteriores:</span><span class="sxs-lookup"><span data-stu-id="98108-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="98108-124">`http://example.net`: dominio diferente</span><span class="sxs-lookup"><span data-stu-id="98108-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="98108-125">`http://example.com:9000/foo.html`: puerto diferente</span><span class="sxs-lookup"><span data-stu-id="98108-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="98108-126">`https://example.com/foo.html`: esquema diferente</span><span class="sxs-lookup"><span data-stu-id="98108-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="98108-127">`http://www.example.com/foo.html`: subdominio diferente</span><span class="sxs-lookup"><span data-stu-id="98108-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="98108-128">Internet Explorer no tiene en cuenta el puerto al comparar orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-128">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="create-the-webservice-project"></a><span data-ttu-id="98108-129">Crear el proyecto WebService</span><span class="sxs-lookup"><span data-stu-id="98108-129">Create the WebService project</span></span>

> [!NOTE]
> <span data-ttu-id="98108-130">En esta sección se da por supuesto que ya sabe cómo crear proyectos de Web API.</span><span class="sxs-lookup"><span data-stu-id="98108-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="98108-131">Si no, consulte [Introducción a ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="98108-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>

1. <span data-ttu-id="98108-132">Inicie Visual Studio y cree un nuevo **aplicación Web ASP.NET (.NET Framework)** proyecto.</span><span class="sxs-lookup"><span data-stu-id="98108-132">Start Visual Studio and create a new **ASP.NET Web Application (.NET Framework)** project.</span></span>
2. <span data-ttu-id="98108-133">En el **nueva aplicación Web ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="98108-133">In the **New ASP.NET Web Application** dialog box, select the **Empty** project template.</span></span> <span data-ttu-id="98108-134">En **agregar carpetas y referencias centrales para**, seleccione el **API Web** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="98108-134">Under **Add folders and core references for**, select the **Web API** checkbox.</span></span>

   ![Nuevo cuadro de diálogo de proyecto ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. <span data-ttu-id="98108-136">Agregar un controlador de Web API llamado `TestController` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="98108-136">Add a Web API controller named `TestController` with the following code:</span></span>

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. <span data-ttu-id="98108-137">Puede ejecutar la aplicación localmente o implementar en Azure.</span><span class="sxs-lookup"><span data-stu-id="98108-137">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="98108-138">(Para las capturas de pantalla en este tutorial, la aplicación se implementa en Azure App Service Web Apps.) Para comprobar que funciona la API web, vaya a `http://hostname/api/test/`, donde *hostname* es el dominio donde se implementa la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98108-138">(For the screenshots in this tutorial, the app deploys to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="98108-139">Debería ver el texto de respuesta, &quot;obtener: Mensaje de prueba&quot;.</span><span class="sxs-lookup"><span data-stu-id="98108-139">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

   ![Mensaje de prueba de que se muestra de explorador Web](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a><span data-ttu-id="98108-141">Crear el proyecto de WebClient</span><span class="sxs-lookup"><span data-stu-id="98108-141">Create the WebClient project</span></span>

1. <span data-ttu-id="98108-142">Crear otro **aplicación Web ASP.NET (.NET Framework)** del proyecto y seleccione el **MVC** plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="98108-142">Create another **ASP.NET Web Application (.NET Framework)** project and select the **MVC** project template.</span></span> <span data-ttu-id="98108-143">Si lo desea, seleccione **Cambiar autenticación** > **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="98108-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="98108-144">No necesita autenticación para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="98108-144">You don't need authentication for this tutorial.</span></span>

   ![Plantilla MVC en el cuadro de diálogo nuevo proyecto de ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. <span data-ttu-id="98108-146">En **el Explorador de soluciones**, abra el archivo *Views/Home/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="98108-146">In **Solution Explorer**, open the file *Views/Home/Index.cshtml*.</span></span> <span data-ttu-id="98108-147">Reemplace el código de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98108-147">Replace the code in this file with the following:</span></span>

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   <span data-ttu-id="98108-148">Para el *serviceUrl* variable, use el identificador URI de la aplicación de servicio Web.</span><span class="sxs-lookup"><span data-stu-id="98108-148">For the *serviceUrl* variable, use the URI of the WebService app.</span></span>

3. <span data-ttu-id="98108-149">Ejecutar la aplicación de WebClient localmente o publicarla en otro sitio Web.</span><span class="sxs-lookup"><span data-stu-id="98108-149">Run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="98108-150">Al hacer clic en el botón "Pruébelo", se envía una solicitud AJAX a la aplicación de servicio Web mediante el método HTTP que se muestran en el cuadro de lista desplegable (GET, POST o PUT).</span><span class="sxs-lookup"><span data-stu-id="98108-150">When you click the "Try It" button, an AJAX request is submitted to the WebService app using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="98108-151">Esto le permite examinar las solicitudes entre orígenes diferentes.</span><span class="sxs-lookup"><span data-stu-id="98108-151">This lets you examine different cross-origin requests.</span></span> <span data-ttu-id="98108-152">Actualmente, la aplicación de servicio Web no admite la CORS, por lo que si hace clic en el botón aparece un error.</span><span class="sxs-lookup"><span data-stu-id="98108-152">Currently, the WebService app does not support CORS, so if you click the button you'll get an error.</span></span>

!['Try' error en el explorador](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="98108-154">Si observa que el tráfico HTTP en una herramienta como [Fiddler](https://www.telerik.com/fiddler), verá que el explorador envía la solicitud GET y la solicitud se realiza correctamente, pero la llamada AJAX devuelve un error.</span><span class="sxs-lookup"><span data-stu-id="98108-154">If you watch the HTTP traffic in a tool like [Fiddler](https://www.telerik.com/fiddler), you'll see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="98108-155">Es importante comprender que la directiva de mismo origen no evita que el Explorador de *enviar* la solicitud.</span><span class="sxs-lookup"><span data-stu-id="98108-155">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="98108-156">En su lugar, impide que la aplicación vean el *respuesta*.</span><span class="sxs-lookup"><span data-stu-id="98108-156">Instead, it prevents the application from seeing the *response*.</span></span>

![Depurador de Fiddler web que muestra las solicitudes web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a><span data-ttu-id="98108-158">Habilitar la CORS</span><span class="sxs-lookup"><span data-stu-id="98108-158">Enable CORS</span></span>

<span data-ttu-id="98108-159">Ahora vamos a habilitar la CORS en la aplicación de servicio Web.</span><span class="sxs-lookup"><span data-stu-id="98108-159">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="98108-160">En primer lugar, agregue el paquete NuGet de la CORS.</span><span class="sxs-lookup"><span data-stu-id="98108-160">First, add the CORS NuGet package.</span></span> <span data-ttu-id="98108-161">En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="98108-161">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="98108-162">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="98108-162">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="98108-163">Este comando instala el paquete más reciente y actualiza todas las dependencias, incluidas las bibliotecas de core Web API.</span><span class="sxs-lookup"><span data-stu-id="98108-163">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="98108-164">Use el `-Version` marca como destino una versión específica.</span><span class="sxs-lookup"><span data-stu-id="98108-164">Use the `-Version` flag to target a specific version.</span></span> <span data-ttu-id="98108-165">El paquete de la CORS necesita Web API 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="98108-165">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="98108-166">Abra el archivo *aplicación\_Start/WebApiConfig.cs*.</span><span class="sxs-lookup"><span data-stu-id="98108-166">Open the file *App\_Start/WebApiConfig.cs*.</span></span> <span data-ttu-id="98108-167">Agregue el código siguiente a la **WebApiConfig.Register** método:</span><span class="sxs-lookup"><span data-stu-id="98108-167">Add the following code to the **WebApiConfig.Register** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="98108-168">A continuación, agregue el **[EnableCors]** atributo a la `TestController` clase:</span><span class="sxs-lookup"><span data-stu-id="98108-168">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="98108-169">Para el *orígenes* parámetro, use el identificador URI que se implementó la aplicación de WebClient.</span><span class="sxs-lookup"><span data-stu-id="98108-169">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="98108-170">Esto permite solicitudes entre orígenes de WebClient, mientras todavía no se permiten todas las demás solicitudes entre dominios.</span><span class="sxs-lookup"><span data-stu-id="98108-170">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="98108-171">Más adelante, describiré los parámetros de **[EnableCors]** con más detalle.</span><span class="sxs-lookup"><span data-stu-id="98108-171">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="98108-172">No incluya una barra diagonal al final de la *orígenes* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="98108-172">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="98108-173">Volver a implementar la aplicación de servicio Web actualizada.</span><span class="sxs-lookup"><span data-stu-id="98108-173">Redeploy the updated WebService application.</span></span> <span data-ttu-id="98108-174">No es necesario actualizar WebClient.</span><span class="sxs-lookup"><span data-stu-id="98108-174">You don't need to update WebClient.</span></span> <span data-ttu-id="98108-175">Ahora debería realizarse correctamente la solicitud de AJAX de WebClient.</span><span class="sxs-lookup"><span data-stu-id="98108-175">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="98108-176">Todos los métodos GET, PUT y POST se permiten.</span><span class="sxs-lookup"><span data-stu-id="98108-176">The GET, PUT, and POST methods are all allowed.</span></span>

![Mensaje de prueba correcta de mostrando de explorador Web](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a><span data-ttu-id="98108-178">Cómo funciona la CORS</span><span class="sxs-lookup"><span data-stu-id="98108-178">How CORS Works</span></span>

<span data-ttu-id="98108-179">En esta sección se describe lo que sucede en una solicitud CORS, en el nivel de los mensajes HTTP.</span><span class="sxs-lookup"><span data-stu-id="98108-179">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="98108-180">Es importante entender cómo funciona la CORS, lo que puede configurar el **[EnableCors]** atributo correctamente y solucionar problemas si algo no funciona según lo esperado.</span><span class="sxs-lookup"><span data-stu-id="98108-180">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="98108-181">La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-181">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="98108-182">Si un explorador es compatible con la CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes; no es necesario hacer nada especial en el código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="98108-182">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="98108-183">Este es un ejemplo de una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-183">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="98108-184">El encabezado de "Origen" proporciona el dominio del sitio que está realizando la solicitud.</span><span class="sxs-lookup"><span data-stu-id="98108-184">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="98108-185">Si el servidor permite la solicitud, Establece el encabezado de Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="98108-185">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="98108-186">El valor de este encabezado coincide con el encabezado de origen, o es el valor de carácter comodín "\*", lo que significa que se permite cualquier origen.</span><span class="sxs-lookup"><span data-stu-id="98108-186">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="98108-187">Si la respuesta no incluye el encabezado de Access-Control-Allow-Origin, se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="98108-187">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="98108-188">En concreto, el explorador no permite la solicitud.</span><span class="sxs-lookup"><span data-stu-id="98108-188">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="98108-189">Incluso si el servidor devuelve una respuesta correcta, el explorador no disponer de la respuesta a la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="98108-189">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="98108-190">**Solicitudes preparatorias**</span><span class="sxs-lookup"><span data-stu-id="98108-190">**Preflight Requests**</span></span>

<span data-ttu-id="98108-191">Para algunas solicitudes CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria," antes de enviar la solicitud para el recurso real.</span><span class="sxs-lookup"><span data-stu-id="98108-191">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="98108-192">El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="98108-192">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="98108-193">El método de solicitud es GET, HEAD o POST, *y*</span><span class="sxs-lookup"><span data-stu-id="98108-193">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="98108-194">La aplicación no establece los encabezados de solicitud que no sean Accept, Accept-Language, Content-Language, Content-Type o último-Event-ID, *y*</span><span class="sxs-lookup"><span data-stu-id="98108-194">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="98108-195">El encabezado Content-Type (si se establece) es uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="98108-195">The Content-Type header (if set) is one of the following:</span></span>

    - <span data-ttu-id="98108-196">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="98108-196">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="98108-197">varias partes/datos de formulario</span><span class="sxs-lookup"><span data-stu-id="98108-197">multipart/form-data</span></span>
    - <span data-ttu-id="98108-198">text/plain</span><span class="sxs-lookup"><span data-stu-id="98108-198">text/plain</span></span>

<span data-ttu-id="98108-199">Se aplica la regla acerca de los encabezados de solicitud a los encabezados de la aplicación se establece mediante una llamada a **setRequestHeader** en el **XMLHttpRequest** objeto.</span><span class="sxs-lookup"><span data-stu-id="98108-199">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="98108-200">(La especificación de CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados de la *explorador* puede establecer como User-Agent, Host o Content-Length.</span><span class="sxs-lookup"><span data-stu-id="98108-200">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="98108-201">Este es un ejemplo de una solicitud de preflight:</span><span class="sxs-lookup"><span data-stu-id="98108-201">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="98108-202">La solicitud preparatoria usa el método HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="98108-202">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="98108-203">Incluye dos encabezados especiales:</span><span class="sxs-lookup"><span data-stu-id="98108-203">It includes two special headers:</span></span>

- <span data-ttu-id="98108-204">Acceso Access-Control-Request-Method: El método HTTP que se usará para la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="98108-204">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="98108-205">Access-Control-Request-Headers: Una lista de encabezados de solicitud que el *aplicación* establecida en la solicitud real.</span><span class="sxs-lookup"><span data-stu-id="98108-205">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="98108-206">(Nuevamente, esto no incluye los encabezados que establece el explorador.)</span><span class="sxs-lookup"><span data-stu-id="98108-206">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="98108-207">Este es un ejemplo de respuesta, suponiendo que el servidor permite la solicitud:</span><span class="sxs-lookup"><span data-stu-id="98108-207">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="98108-208">La respuesta incluye un encabezado Access-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-Allow-Headers, que muestra los encabezados permitidos.</span><span class="sxs-lookup"><span data-stu-id="98108-208">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="98108-209">Si la solicitud de comprobaciones preparatorias se realiza correctamente, el explorador envía la solicitud real, como se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="98108-209">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<span data-ttu-id="98108-210">Herramientas usadas normalmente para probar los puntos de conexión con las solicitudes preparatorias OPTIONS (por ejemplo, [Fiddler](https://www.telerik.com/fiddler) y [Postman](https://www.getpostman.com/)) no enviar los encabezados necesarios de las opciones de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="98108-210">Tools commonly used to test endpoints with preflight OPTIONS requests (for example, [Fiddler](https://www.telerik.com/fiddler) and [Postman](https://www.getpostman.com/)) don't send the required OPTIONS headers by default.</span></span> <span data-ttu-id="98108-211">Confirme que la `Access-Control-Request-Method` y `Access-Control-Request-Headers` encabezados se envían con la solicitud y que los encabezados de las opciones llegue a la aplicación a través de IIS.</span><span class="sxs-lookup"><span data-stu-id="98108-211">Confirm that the `Access-Control-Request-Method` and `Access-Control-Request-Headers` headers are sent with the request and that OPTIONS headers reach the app through IIS.</span></span>

<span data-ttu-id="98108-212">Para configurar IIS para permitir que una aplicación ASP.NET recibir y controlar las solicitudes de opción, agregue la siguiente configuración a la aplicación *web.config* de archivos en el `<system.webServer><handlers>` sección:</span><span class="sxs-lookup"><span data-stu-id="98108-212">To configure IIS to allow an ASP.NET app to receive and handle OPTION requests, add the following configuration to the app's *web.config* file in the `<system.webServer><handlers>` section:</span></span>

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

<span data-ttu-id="98108-213">La eliminación de `OPTIONSVerbHandler` evita que IIS controle las solicitudes de las opciones.</span><span class="sxs-lookup"><span data-stu-id="98108-213">The removal of `OPTIONSVerbHandler` prevents IIS from handling OPTIONS requests.</span></span> <span data-ttu-id="98108-214">La sustitución de `ExtensionlessUrlHandler-Integrated-4.0` permite que las solicitudes de las opciones llegar a la aplicación, ya que el registro de módulo predeterminada sólo permite las solicitudes GET, HEAD, POST y depuración con direcciones URL sin extensión.</span><span class="sxs-lookup"><span data-stu-id="98108-214">The replacement of `ExtensionlessUrlHandler-Integrated-4.0` allows OPTIONS requests to reach the app because the default module registration only allows GET, HEAD, POST, and DEBUG requests with extensionless URLs.</span></span>

## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="98108-215">Reglas de ámbito para [EnableCors]</span><span class="sxs-lookup"><span data-stu-id="98108-215">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="98108-216">Puede habilitar la CORS por cada acción, por controlador o globalmente para todos los controladores de API Web en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98108-216">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="98108-217">**Por acción**</span><span class="sxs-lookup"><span data-stu-id="98108-217">**Per Action**</span></span>

<span data-ttu-id="98108-218">Para habilitar CORS para una única acción, establezca el **[EnableCors]** atributo en el método de acción.</span><span class="sxs-lookup"><span data-stu-id="98108-218">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="98108-219">En el ejemplo siguiente se habilita la CORS para el `GetItem` solo método.</span><span class="sxs-lookup"><span data-stu-id="98108-219">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="98108-220">**Por cada controlador**</span><span class="sxs-lookup"><span data-stu-id="98108-220">**Per Controller**</span></span>

<span data-ttu-id="98108-221">Si establece **[EnableCors]** en la clase de controlador, se aplica a todas las acciones en el controlador.</span><span class="sxs-lookup"><span data-stu-id="98108-221">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="98108-222">Para deshabilitar CORS para una acción, agregue el **[DisableCors]** atributo a la acción.</span><span class="sxs-lookup"><span data-stu-id="98108-222">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="98108-223">El ejemplo siguiente habilita la CORS para todos los métodos excepto `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="98108-223">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="98108-224">**Globally**</span><span class="sxs-lookup"><span data-stu-id="98108-224">**Globally**</span></span>

<span data-ttu-id="98108-225">Para habilitar CORS para todos los controladores de API Web en la aplicación, pase un **EnableCorsAttribute** de instancia para el **EnableCors** método:</span><span class="sxs-lookup"><span data-stu-id="98108-225">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="98108-226">Si establece el atributo en más de un ámbito, el orden de prioridad es:</span><span class="sxs-lookup"><span data-stu-id="98108-226">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="98108-227">Acción</span><span class="sxs-lookup"><span data-stu-id="98108-227">Action</span></span>
2. <span data-ttu-id="98108-228">Controlador</span><span class="sxs-lookup"><span data-stu-id="98108-228">Controller</span></span>
3. <span data-ttu-id="98108-229">Global</span><span class="sxs-lookup"><span data-stu-id="98108-229">Global</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="98108-230">Establecer los orígenes permitidos</span><span class="sxs-lookup"><span data-stu-id="98108-230">Set the allowed origins</span></span>

<span data-ttu-id="98108-231">El *orígenes* parámetro de la **[EnableCors]** atributo especifica qué orígenes tienen permiso para acceder al recurso.</span><span class="sxs-lookup"><span data-stu-id="98108-231">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="98108-232">El valor es una lista separada por comas de los orígenes permitidos.</span><span class="sxs-lookup"><span data-stu-id="98108-232">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="98108-233">También puede usar el valor de carácter comodín "\*" para permitir las solicitudes de los orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-233">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="98108-234">Considere detenidamente antes de permitir las solicitudes desde cualquier origen.</span><span class="sxs-lookup"><span data-stu-id="98108-234">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="98108-235">Significa que literalmente cualquier sitio Web puede realizar llamadas de AJAX a la API web.</span><span class="sxs-lookup"><span data-stu-id="98108-235">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="98108-236">Establecer los métodos HTTP permitidos</span><span class="sxs-lookup"><span data-stu-id="98108-236">Set the allowed HTTP methods</span></span>

<span data-ttu-id="98108-237">El *métodos* parámetro de la **[EnableCors]** atributo especifica qué métodos HTTP pueden tener acceso al recurso.</span><span class="sxs-lookup"><span data-stu-id="98108-237">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="98108-238">Para permitir que todos los métodos, use el valor de carácter comodín "\*".</span><span class="sxs-lookup"><span data-stu-id="98108-238">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="98108-239">El ejemplo siguiente permite solo las solicitudes GET y POST.</span><span class="sxs-lookup"><span data-stu-id="98108-239">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="98108-240">Establecer los encabezados de solicitudes permitidos</span><span class="sxs-lookup"><span data-stu-id="98108-240">Set the allowed request headers</span></span>

<span data-ttu-id="98108-241">En este artículo se describe cómo una solicitud de preflight podría incluir un encabezado de Access-Control-Request-Headers, enumerar los encabezados HTTP establecidos por la aplicación (el llamado "author encabezados de solicitud").</span><span class="sxs-lookup"><span data-stu-id="98108-241">This article described earlier how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="98108-242">El *encabezados* parámetro de la **[EnableCors]** atributo especifica qué encabezados de solicitud del autor se permiten.</span><span class="sxs-lookup"><span data-stu-id="98108-242">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="98108-243">Para permitir que todos los encabezados, establezca *encabezados* a "\*".</span><span class="sxs-lookup"><span data-stu-id="98108-243">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="98108-244">A encabezados específicos de la lista de permitidos, establezca *encabezados* a una lista separada por comas de los encabezados permitidos:</span><span class="sxs-lookup"><span data-stu-id="98108-244">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="98108-245">Sin embargo, los exploradores no son totalmente coherentes en forma de conjunto de Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="98108-245">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="98108-246">Por ejemplo, Chrome actualmente incluye "origin".</span><span class="sxs-lookup"><span data-stu-id="98108-246">For example, Chrome currently includes "origin".</span></span> <span data-ttu-id="98108-247">FireFox no incluye encabezados estándar, como "Accept", incluso cuando la aplicación establece en la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="98108-247">FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="98108-248">Si establece *encabezados* para que no sea "\*", debe incluir al menos "accept", "content-type" y "origen", además de los encabezados personalizados que desee admitir.</span><span class="sxs-lookup"><span data-stu-id="98108-248">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="98108-249">Establecer los encabezados de respuesta permitidos</span><span class="sxs-lookup"><span data-stu-id="98108-249">Set the allowed response headers</span></span>

<span data-ttu-id="98108-250">De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="98108-250">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="98108-251">Los encabezados de respuesta que están disponibles de forma predeterminada son:</span><span class="sxs-lookup"><span data-stu-id="98108-251">The response headers that are available by default are:</span></span>

- <span data-ttu-id="98108-252">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="98108-252">Cache-Control</span></span>
- <span data-ttu-id="98108-253">Idioma del contenido</span><span class="sxs-lookup"><span data-stu-id="98108-253">Content-Language</span></span>
- <span data-ttu-id="98108-254">Content-Type</span><span class="sxs-lookup"><span data-stu-id="98108-254">Content-Type</span></span>
- <span data-ttu-id="98108-255">Expires</span><span class="sxs-lookup"><span data-stu-id="98108-255">Expires</span></span>
- <span data-ttu-id="98108-256">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="98108-256">Last-Modified</span></span>
- <span data-ttu-id="98108-257">Pragma</span><span class="sxs-lookup"><span data-stu-id="98108-257">Pragma</span></span>

<span data-ttu-id="98108-258">La especificación CORS llama a estos [encabezados de respuesta simple](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="98108-258">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="98108-259">Para que otros encabezados disponibles para la aplicación, establezca el *exposedHeaders* parámetro de **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="98108-259">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="98108-260">En del ejemplo siguiente, el controlador `Get` método establece un encabezado personalizado denominado "X-Custom-Header".</span><span class="sxs-lookup"><span data-stu-id="98108-260">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="98108-261">De forma predeterminada, el explorador no expondrá este encabezado en una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-261">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="98108-262">Para que esté disponible el encabezado, incluir "X-Custom-Header" en *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="98108-262">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a><span data-ttu-id="98108-263">Pase las credenciales en solicitudes entre orígenes</span><span class="sxs-lookup"><span data-stu-id="98108-263">Pass credentials in cross-origin requests</span></span>

<span data-ttu-id="98108-264">Las credenciales requieren un tratamiento especial en una solicitud de CORS.</span><span class="sxs-lookup"><span data-stu-id="98108-264">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="98108-265">De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-265">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="98108-266">Las credenciales incluyen las cookies, así como los esquemas de autenticación HTTP.</span><span class="sxs-lookup"><span data-stu-id="98108-266">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="98108-267">Para enviar las credenciales con una solicitud entre orígenes, el cliente debe establecer **XMLHttpRequest.withCredentials** en true.</span><span class="sxs-lookup"><span data-stu-id="98108-267">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="98108-268">Uso de **XMLHttpRequest** directamente:</span><span class="sxs-lookup"><span data-stu-id="98108-268">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="98108-269">En jQuery:</span><span class="sxs-lookup"><span data-stu-id="98108-269">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="98108-270">Además, el servidor debe permitir las credenciales.</span><span class="sxs-lookup"><span data-stu-id="98108-270">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="98108-271">Para permitir que las credenciales de origen cruzado en la API Web, establezca el **SupportsCredentials** propiedad en true en el **[EnableCors]** atributo:</span><span class="sxs-lookup"><span data-stu-id="98108-271">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="98108-272">Si esta propiedad es true, la respuesta HTTP incluirá un encabezado de Access-Control-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="98108-272">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="98108-273">Este encabezado indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.</span><span class="sxs-lookup"><span data-stu-id="98108-273">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="98108-274">Si el explorador envía credenciales, pero la respuesta no incluye un encabezado válido de Access-Control-Allow-Credentials, el explorador no expondrá la respuesta a la aplicación y se produce un error en la solicitud de AJAX.</span><span class="sxs-lookup"><span data-stu-id="98108-274">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="98108-275">Tenga cuidado sobre la configuración de **SupportsCredentials** en true, ya que significa que un sitio Web en otro dominio puede enviar credenciales ha iniciado la sesión de un usuario a la API Web en el nombre de usuario, sin que el usuario que se va a tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="98108-275">Be careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="98108-276">La especificación CORS también establece ese valor *orígenes* a &quot; \* &quot; no es válido si **SupportsCredentials** es true.</span><span class="sxs-lookup"><span data-stu-id="98108-276">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

## <a name="custom-cors-policy-providers"></a><span data-ttu-id="98108-277">Proveedores personalizados de directiva CORS</span><span class="sxs-lookup"><span data-stu-id="98108-277">Custom CORS policy providers</span></span>

<span data-ttu-id="98108-278">El **[EnableCors]** atributo implementa el **ICorsPolicyProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="98108-278">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="98108-279">Puede proporcionar su propia implementación mediante la creación de una clase que derive de **atributo** e implementa **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="98108-279">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="98108-280">Ahora puede aplicar el atributo de cualquier lugar que tendría que poner **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="98108-280">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="98108-281">Por ejemplo, un proveedor personalizado de directiva CORS pudo leer la configuración desde un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="98108-281">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="98108-282">Como alternativa al uso de atributos, puede registrar un **ICorsPolicyProviderFactory** objeto que crea **ICorsPolicyProvider** objetos.</span><span class="sxs-lookup"><span data-stu-id="98108-282">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="98108-283">Para establecer el **ICorsPolicyProviderFactory**, llame a la **SetCorsPolicyProviderFactory** método de extensión en el inicio, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="98108-283">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a><span data-ttu-id="98108-284">Compatibilidad con exploradores</span><span class="sxs-lookup"><span data-stu-id="98108-284">Browser support</span></span>

<span data-ttu-id="98108-285">El paquete de Web API CORS es una tecnología del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="98108-285">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="98108-286">El explorador del usuario también debe admitir la CORS.</span><span class="sxs-lookup"><span data-stu-id="98108-286">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="98108-287">Afortunadamente, las versiones actuales de todos los exploradores principales incluyen [compatibilidad con CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="98108-287">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>
