---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creación de una aplicación de cliente de OData v4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 4d62a64006e2a500e0379419dbebe7ddff16fba5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828531"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="c0746-102">Creación de una aplicación de cliente de OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="c0746-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="c0746-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c0746-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c0746-104">En el tutorial anterior, creó un servicio básico de OData que admite operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="c0746-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="c0746-105">Ahora vamos a crear un cliente para el servicio.</span><span class="sxs-lookup"><span data-stu-id="c0746-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="c0746-106">Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="c0746-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="c0746-107">En el **nuevo proyecto** cuadro de diálogo, seleccione **instalado** &gt; **plantillas** &gt; **Visual C#** &gt; **Windows Desktop**y seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="c0746-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="c0746-108">Denomine el proyecto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0746-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="c0746-109">También puede agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="c0746-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="c0746-110">Instalar el generador de código de cliente de OData</span><span class="sxs-lookup"><span data-stu-id="c0746-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="c0746-111">Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="c0746-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="c0746-112">Seleccione **Online** &gt; **Galería de Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c0746-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="c0746-113">En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0746-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="c0746-114">Haga clic en **descargar** para instalar la extensión VSIX.</span><span class="sxs-lookup"><span data-stu-id="c0746-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="c0746-115">Puede que deba reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0746-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="c0746-116">Ejecuta el servicio de OData localmente</span><span class="sxs-lookup"><span data-stu-id="c0746-116">Run the OData Service Locally</span></span>

<span data-ttu-id="c0746-117">Ejecute el proyecto ProductService desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c0746-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="c0746-118">De forma predeterminada, Visual Studio inicia un explorador en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0746-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="c0746-119">Tenga en cuenta el identificador URI; lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0746-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="c0746-120">Deje la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="c0746-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="c0746-121">Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración.</span><span class="sxs-lookup"><span data-stu-id="c0746-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="c0746-122">En el paso siguiente, deberá mantener el servicio se está ejecutando mientras modifica el proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="c0746-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="c0746-123">Generar al Proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="c0746-123">Generate the Service Proxy</span></span>

<span data-ttu-id="c0746-124">El proxy de servicio es una clase .NET que define los métodos para acceder al servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="c0746-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="c0746-125">El proxy traduce llamadas de método en las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c0746-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="c0746-126">Creará la clase de proxy mediante la ejecución de un [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="c0746-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="c0746-127">Haga clic en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c0746-127">Right-click the project.</span></span> <span data-ttu-id="c0746-128">Seleccione **agregar** &gt; **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="c0746-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="c0746-129">En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **elementos de Visual C#** &gt; **código** &gt; **cliente de OData**.</span><span class="sxs-lookup"><span data-stu-id="c0746-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="c0746-130">Nombre de la plantilla &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0746-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="c0746-131">Haga clic en **agregar** y haga clic en a través de la advertencia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c0746-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="c0746-132">En este momento, obtendrá un error, que puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="c0746-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="c0746-133">Visual Studio ejecuta automáticamente la plantilla, pero necesita algunos valores de configuración de la plantilla de primera.</span><span class="sxs-lookup"><span data-stu-id="c0746-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="c0746-134">Abra el archivo ProductClient.odata.config. En el `Parameter` elemento, pegue el URI desde el proyecto ProductService (paso anterior).</span><span class="sxs-lookup"><span data-stu-id="c0746-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="c0746-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c0746-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="c0746-136">Vuelva a ejecutar la plantilla.</span><span class="sxs-lookup"><span data-stu-id="c0746-136">Run the template again.</span></span> <span data-ttu-id="c0746-137">En el Explorador de soluciones, haga clic el archivo ProductClient.tt y seleccione **ejecutar herramienta personalizada**.</span><span class="sxs-lookup"><span data-stu-id="c0746-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="c0746-138">La plantilla crea un archivo de código denominado ProductClient.cs que define al proxy.</span><span class="sxs-lookup"><span data-stu-id="c0746-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="c0746-139">Al desarrollar la aplicación, si cambia el punto de conexión de OData, ejecutar la plantilla de nuevo para actualizar al servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="c0746-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="c0746-140">Usar al Proxy de servicio para llamar al servicio de OData</span><span class="sxs-lookup"><span data-stu-id="c0746-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="c0746-141">Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0746-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="c0746-142">Reemplace el valor de *serviceUri* con el URI del servicio de antes.</span><span class="sxs-lookup"><span data-stu-id="c0746-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="c0746-143">Al ejecutar la aplicación, debe mostrar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0746-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
