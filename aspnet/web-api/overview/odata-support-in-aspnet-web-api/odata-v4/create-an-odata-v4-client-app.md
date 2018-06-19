---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Crear una aplicación de cliente de OData v4 (C#) | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036705"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="286d3-102">Crear una aplicación de cliente de OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="286d3-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="286d3-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="286d3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="286d3-104">En el tutorial anterior, creó un servicio básico de OData que admite operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="286d3-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="286d3-105">Ahora vamos a crear un cliente para el servicio.</span><span class="sxs-lookup"><span data-stu-id="286d3-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="286d3-106">Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="286d3-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="286d3-107">En el **nuevo proyecto** cuadro de diálogo, seleccione **instalado** &gt; **plantillas** &gt; **Visual C#** &gt; **Windows Desktop**y seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="286d3-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="286d3-108">Denomine el proyecto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="286d3-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="286d3-109">También puede agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="286d3-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="286d3-110">Instalar el generador de código de cliente de OData</span><span class="sxs-lookup"><span data-stu-id="286d3-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="286d3-111">Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="286d3-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="286d3-112">Seleccione **en línea** &gt; **Galería de Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="286d3-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="286d3-113">En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="286d3-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="286d3-114">Haga clic en **descargar** para instalar la extensión VSIX.</span><span class="sxs-lookup"><span data-stu-id="286d3-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="286d3-115">Puede que deba reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="286d3-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="286d3-116">Ejecutar el servicio de OData localmente</span><span class="sxs-lookup"><span data-stu-id="286d3-116">Run the OData Service Locally</span></span>

<span data-ttu-id="286d3-117">Ejecute el proyecto ProductService desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="286d3-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="286d3-118">De forma predeterminada, Visual Studio inicia un explorador a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="286d3-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="286d3-119">Tenga en cuenta el identificador URI; lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="286d3-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="286d3-120">Deje la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="286d3-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="286d3-121">Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración.</span><span class="sxs-lookup"><span data-stu-id="286d3-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="286d3-122">En el paso siguiente, debe mantener el servicio se ejecuta mientras modifica el proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="286d3-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="286d3-123">Generar al Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="286d3-123">Generate the Service Proxy</span></span>

<span data-ttu-id="286d3-124">El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="286d3-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="286d3-125">El proxy convierte las llamadas de método en las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="286d3-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="286d3-126">Se creará la clase de proxy mediante la ejecución de un [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="286d3-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="286d3-127">Haga clic en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="286d3-127">Right-click the project.</span></span> <span data-ttu-id="286d3-128">Seleccione **agregar** &gt; **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="286d3-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="286d3-129">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **elementos de Visual C#** &gt; **código** &gt; **cliente de OData**.</span><span class="sxs-lookup"><span data-stu-id="286d3-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="286d3-130">Nombre de la plantilla &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="286d3-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="286d3-131">Haga clic en **agregar** y haga clic en a través de la advertencia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="286d3-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="286d3-132">En este momento, obtendrá un error, que puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="286d3-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="286d3-133">Visual Studio ejecuta automáticamente la plantilla, pero la plantilla necesita algunos valores de configuración primero.</span><span class="sxs-lookup"><span data-stu-id="286d3-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="286d3-134">Abra el archivo ProductClient.odata.config. En el `Parameter` elemento, pegar el URI desde el proyecto ProductService (paso anterior).</span><span class="sxs-lookup"><span data-stu-id="286d3-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="286d3-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="286d3-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="286d3-136">Vuelva a ejecutar la plantilla.</span><span class="sxs-lookup"><span data-stu-id="286d3-136">Run the template again.</span></span> <span data-ttu-id="286d3-137">En el Explorador de soluciones, haga clic en el archivo de ProductClient.tt y seleccione **ejecutar herramienta personalizada**.</span><span class="sxs-lookup"><span data-stu-id="286d3-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="286d3-138">La plantilla crea un archivo de código denominado ProductClient.cs que define al servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="286d3-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="286d3-139">Cuando desarrolle la aplicación, si cambia el extremo de OData, ejecutar la plantilla de nuevo para actualizar al servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="286d3-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="286d3-140">Usar al Proxy de servicio para llamar al servicio de OData</span><span class="sxs-lookup"><span data-stu-id="286d3-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="286d3-141">Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="286d3-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="286d3-142">Sustituya el valor de *serviceUri* con el URI del servicio de antes.</span><span class="sxs-lookup"><span data-stu-id="286d3-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="286d3-143">Cuando se ejecuta la aplicación, debe devolver el siguiente:</span><span class="sxs-lookup"><span data-stu-id="286d3-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
