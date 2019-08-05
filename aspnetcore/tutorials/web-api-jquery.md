---
title: 'Tutorial: Llamada a una API Web con jQuery mediante ASP.NET Core'
author: rick-anderson
description: Obtenga información sobre cómo llamar a una API web de ASP.NET Core con jQuery.
ms.author: riande
ms.custom: mvc
ms.date: 07/20/2019
uid: tutorials/web-api-jquery
ms.openlocfilehash: eb8b2453fd037170a49f531fea4c3ef1c056292d
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602574"
---
# <a name="tutorial-call-an-aspnet-core-web-api-with-jquery"></a><span data-ttu-id="cffd5-103">Tutorial: Llamada a una API web de ASP.NET Core con jQuery</span><span class="sxs-lookup"><span data-stu-id="cffd5-103">Tutorial: Call an ASP.NET Core web API with jQuery</span></span>

<span data-ttu-id="cffd5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cffd5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cffd5-105">En este tutorial se muestra cómo llamar a una API web de ASP.NET Core con jQuery.</span><span class="sxs-lookup"><span data-stu-id="cffd5-105">This tutorial shows how to call an ASP.NET Core web API with jQuery</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cffd5-106">Para ASP.net Core 2.2, consulte la versión 2.2 de [Llamada a la API web con jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span><span class="sxs-lookup"><span data-stu-id="cffd5-106">For ASP.NET Core 2.2, see the 2.2 version of [Call the Web API with jQuery](xref:tutorials/first-web-api#call-the-api-with-jquery).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="cffd5-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="cffd5-107">Prerequisites</span></span>

<span data-ttu-id="cffd5-108">Finalización del [Tutorial: Creación de una API web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="cffd5-108">Complete [Tutorial: Create a web API](xref:tutorials/first-web-api)</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="cffd5-109">Llamada a la API con jQuery</span><span class="sxs-lookup"><span data-stu-id="cffd5-109">Call the API with jQuery</span></span>

<span data-ttu-id="cffd5-110">En esta sección, se agrega una página HTML que usa jQuery para llamar a la API web.</span><span class="sxs-lookup"><span data-stu-id="cffd5-110">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="cffd5-111">jQuery inicia la solicitud y actualiza la página con los detalles de la respuesta de la API.</span><span class="sxs-lookup"><span data-stu-id="cffd5-111">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="cffd5-112">Configure la aplicación para [atender archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) y [habilitar la asignación de archivos predeterminada](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) mediante la actualización de *Startup.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="cffd5-112">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/StartupJquery.cs?highlight=8-9&name=snippet_configure)]

<span data-ttu-id="cffd5-113">Cree una carpeta *wwwroot* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="cffd5-113">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="cffd5-114">Agregue un archivo HTML denominado *index.html* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="cffd5-114">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="cffd5-115">Reemplace el contenido por el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="cffd5-115">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/3.0/TodoApi/wwwroot/index.html)]

<span data-ttu-id="cffd5-116">Agregue un archivo JavaScript denominado *site.js* al directorio *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="cffd5-116">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="cffd5-117">Reemplace el contenido por el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="cffd5-117">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="cffd5-118">Puede que sea necesario realizar un cambio en la configuración de inicio del proyecto de ASP.NET Core para probar la página HTML localmente:</span><span class="sxs-lookup"><span data-stu-id="cffd5-118">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="cffd5-119">Abra *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="cffd5-119">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="cffd5-120">Quite la propiedad `launchUrl` para forzar a la aplicación a abrirse en *index.html*, esto es, el archivo predeterminado del proyecto.</span><span class="sxs-lookup"><span data-stu-id="cffd5-120">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="cffd5-121">Existen varias formas de obtener jQuery.</span><span class="sxs-lookup"><span data-stu-id="cffd5-121">There are several ways to get jQuery.</span></span> <span data-ttu-id="cffd5-122">En el fragmento de código anterior, la biblioteca se carga desde una red CDN.</span><span class="sxs-lookup"><span data-stu-id="cffd5-122">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="cffd5-123">En este ejemplo se llama a todos los métodos CRUD de la API.</span><span class="sxs-lookup"><span data-stu-id="cffd5-123">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="cffd5-124">A continuación, encontrará algunas explicaciones de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="cffd5-124">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="cffd5-125">Obtención de una lista de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="cffd5-125">Get a list of to-do items</span></span>

<span data-ttu-id="cffd5-126">La función de JQuery [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `GET` a la API, que devuelve código JSON que representa una matriz de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="cffd5-126">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="cffd5-127">La función de devolución de llamada `success` se invoca si la solicitud se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="cffd5-127">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="cffd5-128">En la devolución de llamada, el DOM se actualiza con la información de la tarea pendiente.</span><span class="sxs-lookup"><span data-stu-id="cffd5-128">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="cffd5-129">Incorporación de una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="cffd5-129">Add a to-do item</span></span>

<span data-ttu-id="cffd5-130">La función [ajax](https://api.jquery.com/jquery.ajax/) envía una solicitud `POST` con la tarea pendiente en su cuerpo.</span><span class="sxs-lookup"><span data-stu-id="cffd5-130">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="cffd5-131">Las opciones `accepts` y `contentType` se establecen en `application/json` para especificar el tipo de medio que se va a recibir y a enviar.</span><span class="sxs-lookup"><span data-stu-id="cffd5-131">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="cffd5-132">La tarea pendiente se convierte en JSON mediante [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="cffd5-132">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="cffd5-133">Cuando la API devuelve un código de estado correcto, se invoca la función `getData` para actualizar la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="cffd5-133">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="cffd5-134">Actualizar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="cffd5-134">Update a to-do item</span></span>

<span data-ttu-id="cffd5-135">El hecho de actualizar una tarea pendiente es similar al de agregar una.</span><span class="sxs-lookup"><span data-stu-id="cffd5-135">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="cffd5-136">El valor `url` cambia para agregar el identificador único del elemento, y `type` es `PUT`.</span><span class="sxs-lookup"><span data-stu-id="cffd5-136">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="cffd5-137">Eliminar una tarea pendiente</span><span class="sxs-lookup"><span data-stu-id="cffd5-137">Delete a to-do item</span></span>

<span data-ttu-id="cffd5-138">Para eliminar una tarea pendiente, hay que establecer el valor `type` de la llamada de AJAX en `DELETE` y especificar el identificador único de la tarea en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="cffd5-138">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/3.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

<span data-ttu-id="cffd5-139">Pase al siguiente tutorial para obtener información sobre cómo generar páginas de ayuda de API:</span><span class="sxs-lookup"><span data-stu-id="cffd5-139">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
::: moniker-end