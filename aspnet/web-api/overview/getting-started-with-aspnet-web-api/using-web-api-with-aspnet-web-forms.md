---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso de API Web con ASP.NET Web Forms | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840893"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="45c36-102">Uso de API Web con ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="45c36-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="45c36-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="45c36-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="45c36-104">Aunque ASP.NET Web API se empaqueta con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="45c36-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="45c36-105">Este tutorial le guiará a través de los pasos.</span><span class="sxs-lookup"><span data-stu-id="45c36-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="45c36-106">Información general</span><span class="sxs-lookup"><span data-stu-id="45c36-106">Overview</span></span>

<span data-ttu-id="45c36-107">Para usar la API Web en una aplicación de formularios Web Forms, hay dos pasos principales:</span><span class="sxs-lookup"><span data-stu-id="45c36-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="45c36-108">Agregar un controlador de API Web que se deriva de la **ApiController** clase.</span><span class="sxs-lookup"><span data-stu-id="45c36-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="45c36-109">Agregar una tabla de rutas para el **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="45c36-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="45c36-110">Crear un proyecto de Web Forms</span><span class="sxs-lookup"><span data-stu-id="45c36-110">Create a Web Forms Project</span></span>

<span data-ttu-id="45c36-111">Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="45c36-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="45c36-112">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="45c36-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="45c36-113">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="45c36-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="45c36-114">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="45c36-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="45c36-115">En la lista de plantillas de proyecto, seleccione **aplicación de ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="45c36-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="45c36-116">Escriba un nombre para el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="45c36-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="45c36-117">Crear el modelo y controlador</span><span class="sxs-lookup"><span data-stu-id="45c36-117">Create the Model and Controller</span></span>

<span data-ttu-id="45c36-118">Este tutorial usa las mismas clases de modelo y controlador como el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="45c36-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="45c36-119">En primer lugar, agregue una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="45c36-119">First, add a model class.</span></span> <span data-ttu-id="45c36-120">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **Agregar clase**.</span><span class="sxs-lookup"><span data-stu-id="45c36-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="45c36-121">Nombre de la clase de producto y agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="45c36-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="45c36-122">A continuación, agregue un controlador Web API al proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para API Web.</span><span class="sxs-lookup"><span data-stu-id="45c36-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="45c36-123">En **el Explorador de soluciones**, haga clic en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="45c36-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="45c36-124">Seleccione **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="45c36-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="45c36-125">En **plantillas instaladas**, expanda **Visual C#** y seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="45c36-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="45c36-126">A continuación, en la lista de plantillas, seleccione **clase de controlador de Web API**.</span><span class="sxs-lookup"><span data-stu-id="45c36-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="45c36-127">Asigne al controlador el "ProductsController" y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="45c36-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="45c36-128">El **Agregar nuevo elemento** asistente creará un archivo denominado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="45c36-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="45c36-129">Elimine los métodos que incluye el asistente y agregue los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="45c36-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="45c36-130">Para obtener más información sobre el código de este controlador, consulte el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="45c36-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="45c36-131">Agregar información de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="45c36-131">Add Routing Information</span></span>

<span data-ttu-id="45c36-132">A continuación, vamos a agregar una ruta URI así que esa URI del formulario &quot;/API/productos/&quot; se enrutan al controlador.</span><span class="sxs-lookup"><span data-stu-id="45c36-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="45c36-133">En **el Explorador de soluciones**, haga doble clic en Global.asax para abrir el archivo de código subyacente Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="45c36-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="45c36-134">Agregue el siguiente **mediante** instrucción.</span><span class="sxs-lookup"><span data-stu-id="45c36-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="45c36-135">A continuación, agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="45c36-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="45c36-136">Para obtener más información acerca de las tablas de enrutamientos, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="45c36-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="45c36-137">Agregar cliente AJAX</span><span class="sxs-lookup"><span data-stu-id="45c36-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="45c36-138">Eso es todo lo que necesita para crear una API web que pueden tener acceso los clientes.</span><span class="sxs-lookup"><span data-stu-id="45c36-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="45c36-139">Ahora vamos a agregar una página HTML que usa jQuery para llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="45c36-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="45c36-140">Asegúrese de que la página maestra (por ejemplo, *Site.Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="45c36-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="45c36-141">Abra el archivo Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="45c36-141">Open the file Default.aspx.</span></span> <span data-ttu-id="45c36-142">Reemplace el texto reutilizable que se encuentra en la sección de contenido principal, como se muestra:</span><span class="sxs-lookup"><span data-stu-id="45c36-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="45c36-143">A continuación, agregue una referencia al archivo de código fuente de jQuery en la `HeaderContent` sección:</span><span class="sxs-lookup"><span data-stu-id="45c36-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="45c36-144">Nota: Puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **el Explorador de soluciones** en la ventana del editor de código.</span><span class="sxs-lookup"><span data-stu-id="45c36-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="45c36-145">A continuación de la etiqueta de script de jQuery, agregue el siguiente bloque de script:</span><span class="sxs-lookup"><span data-stu-id="45c36-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="45c36-146">Cuando se carga el documento, este script realiza una solicitud de AJAX para &quot;api/products&quot;.</span><span class="sxs-lookup"><span data-stu-id="45c36-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="45c36-147">La solicitud devuelve una lista de productos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="45c36-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="45c36-148">El script agrega la información del producto a la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="45c36-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="45c36-149">Al ejecutar la aplicación, debe ver así:</span><span class="sxs-lookup"><span data-stu-id="45c36-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
