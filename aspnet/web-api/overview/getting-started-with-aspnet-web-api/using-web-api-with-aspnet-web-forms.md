---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usar Web API con ASP.NET Web Forms | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="3c00b-102">Usar Web API con ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="3c00b-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="3c00b-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3c00b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3c00b-104">Aunque ASP.NET Web API se empaqueta con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="3c00b-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="3c00b-105">Este tutorial le guía por los pasos necesarios.</span><span class="sxs-lookup"><span data-stu-id="3c00b-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="3c00b-106">Información general</span><span class="sxs-lookup"><span data-stu-id="3c00b-106">Overview</span></span>

<span data-ttu-id="3c00b-107">Para usar la API Web en una aplicación de formularios Web Forms, hay dos pasos principales:</span><span class="sxs-lookup"><span data-stu-id="3c00b-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="3c00b-108">Agregar un controlador de API Web que se deriva de la **ApiController** clase.</span><span class="sxs-lookup"><span data-stu-id="3c00b-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="3c00b-109">Agregar una tabla de rutas para la **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="3c00b-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="3c00b-110">Cree un proyecto de formularios Web</span><span class="sxs-lookup"><span data-stu-id="3c00b-110">Create a Web Forms Project</span></span>

<span data-ttu-id="3c00b-111">Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="3c00b-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="3c00b-112">O bien, en la **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="3c00b-113">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="3c00b-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="3c00b-114">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="3c00b-115">En la lista de plantillas de proyecto, seleccione **aplicación de ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="3c00b-116">Escriba un nombre para el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="3c00b-117">Crear el modelo y controlador</span><span class="sxs-lookup"><span data-stu-id="3c00b-117">Create the Model and Controller</span></span>

<span data-ttu-id="3c00b-118">Este tutorial utiliza las mismas clases del modelo y controlador como el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="3c00b-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="3c00b-119">En primer lugar, agregue una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="3c00b-119">First, add a model class.</span></span> <span data-ttu-id="3c00b-120">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **Agregar clase**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="3c00b-121">Nombre de la clase de producto y agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="3c00b-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="3c00b-122">A continuación, agregue un controlador de Web API para el proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para la API Web.</span><span class="sxs-lookup"><span data-stu-id="3c00b-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="3c00b-123">En **el Explorador de soluciones**, haga clic en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3c00b-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="3c00b-124">Seleccione **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="3c00b-125">En **plantillas instaladas**, expanda **Visual C#** y seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="3c00b-126">A continuación, en la lista de plantillas, seleccione **clase de controlador de Web API**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="3c00b-127">"ProductsController" al controlador el nombre y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="3c00b-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="3c00b-128">El **Agregar nuevo elemento** asistente creará un archivo denominado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="3c00b-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="3c00b-129">Elimine los métodos que incluye el asistente y agregue los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3c00b-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="3c00b-130">Para obtener más información sobre el código de este controlador, consulte el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="3c00b-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="3c00b-131">Agregar información de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="3c00b-131">Add Routing Information</span></span>

<span data-ttu-id="3c00b-132">A continuación, vamos a agregar una ruta URI por lo que ese URI del formulario &quot;/API/products/&quot; se enrutan al controlador.</span><span class="sxs-lookup"><span data-stu-id="3c00b-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="3c00b-133">En **el Explorador de soluciones**, haga doble clic en Global.asax para abrir el archivo de código subyacente Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="3c00b-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="3c00b-134">Agregue el siguiente **con** instrucción.</span><span class="sxs-lookup"><span data-stu-id="3c00b-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="3c00b-135">A continuación, agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="3c00b-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="3c00b-136">Para obtener más información acerca de las tablas de enrutamientos, consulte [enrutamiento de ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3c00b-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="3c00b-137">Agregar AJAX del lado cliente</span><span class="sxs-lookup"><span data-stu-id="3c00b-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="3c00b-138">Que es todo lo que necesita para crear una API que pueden tener acceso los clientes web.</span><span class="sxs-lookup"><span data-stu-id="3c00b-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="3c00b-139">Ahora vamos a agregar una página HTML que utiliza jQuery para llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="3c00b-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="3c00b-140">Asegúrese de que la página principal (por ejemplo, *Site.Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="3c00b-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="3c00b-141">Abra el archivo Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="3c00b-141">Open the file Default.aspx.</span></span> <span data-ttu-id="3c00b-142">Reemplace el texto reutilizable que se encuentra en la sección de contenido principal, tal como se muestra:</span><span class="sxs-lookup"><span data-stu-id="3c00b-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="3c00b-143">A continuación, agregue una referencia al archivo de origen de jQuery en la `HeaderContent` sección:</span><span class="sxs-lookup"><span data-stu-id="3c00b-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="3c00b-144">Nota: Puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **el Explorador de soluciones** en la ventana del editor de código.</span><span class="sxs-lookup"><span data-stu-id="3c00b-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="3c00b-145">A continuación de la etiqueta de script jQuery, agregue el siguiente bloque de script:</span><span class="sxs-lookup"><span data-stu-id="3c00b-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="3c00b-146">Cuando se carga el documento, este script realiza una solicitud AJAX a &quot;api/productos&quot;.</span><span class="sxs-lookup"><span data-stu-id="3c00b-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="3c00b-147">La solicitud devuelve una lista de productos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="3c00b-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="3c00b-148">El script agrega la información del producto a la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="3c00b-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="3c00b-149">Al ejecutar la aplicación, debería ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3c00b-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
