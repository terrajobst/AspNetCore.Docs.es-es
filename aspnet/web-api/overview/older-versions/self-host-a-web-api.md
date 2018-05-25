---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Autohospedaje ASP.NET Web API 1 (C#) | Documentos de Microsoft
author: MikeWasson
description: ASP.NET Web API no requiere IIS. Puede probarlo internamente una API web en su propio proceso de host. Este tutorial muestra cómo hospedar una API web dentro de una consola applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="2de9f-105">Autohospedaje ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="2de9f-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="2de9f-106">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2de9f-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2de9f-107">ASP.NET Web API no requiere IIS.</span><span class="sxs-lookup"><span data-stu-id="2de9f-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="2de9f-108">Puede probarlo internamente una API web en su propio proceso de host.</span><span class="sxs-lookup"><span data-stu-id="2de9f-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="2de9f-109">Este tutorial muestra cómo hospedar una API web dentro de una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="2de9f-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="2de9f-110">**Aplicaciones nuevas deben usar OWIN para probar internamente API Web.**</span><span class="sxs-lookup"><span data-stu-id="2de9f-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="2de9f-111">Vea [usar OWIN para probar internamente ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2de9f-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2de9f-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="2de9f-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2de9f-113">API Web 1</span><span class="sxs-lookup"><span data-stu-id="2de9f-113">Web API 1</span></span>
> - <span data-ttu-id="2de9f-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2de9f-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="2de9f-115">Crear el proyecto de aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="2de9f-115">Create the Console Application Project</span></span>

<span data-ttu-id="2de9f-116">Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="2de9f-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2de9f-117">O bien, en la **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2de9f-118">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="2de9f-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2de9f-119">En **Visual C#**, seleccione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="2de9f-120">En la lista de plantillas de proyecto, seleccione **aplicación de consola**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="2de9f-121">Denomine el proyecto &quot;SelfHost&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="2de9f-122">Establezca la plataforma de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="2de9f-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="2de9f-123">Si usa Visual Studio 2010, cambiar la plataforma de destino a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="2de9f-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2de9f-124">(De forma predeterminada, los destinos de la plantilla de proyecto la [perfil de .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="2de9f-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="2de9f-125">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2de9f-126">En el **.NET framework de destino** lista desplegable lista, cambie la plataforma de destino a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="2de9f-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2de9f-127">Cuando se le pida para aplicar el cambio, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="2de9f-128">Instalar el Administrador de paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="2de9f-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="2de9f-129">El Administrador de paquetes de NuGet es la manera más fácil de agregar los ensamblados de la API Web a un proyecto de ASP.NET no.</span><span class="sxs-lookup"><span data-stu-id="2de9f-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="2de9f-130">Para comprobar si el Administrador de paquetes de NuGet está instalado, haga clic en el **herramientas** menú en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2de9f-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="2de9f-131">Si ve un elemento de menú denominado **Administrador de paquetes de biblioteca**, tendrá que Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="2de9f-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="2de9f-132">Para instalar el Administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="2de9f-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="2de9f-133">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2de9f-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="2de9f-134">Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="2de9f-135">En el **extensiones y actualizaciones** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2de9f-136">Si no ve "Administrador de paquetes de NuGet", escriba "Administrador de paquetes de nuget" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="2de9f-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="2de9f-137">Seleccione el Administrador de paquetes de NuGet y haga clic en **descargar**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="2de9f-138">Una vez finalizada la descarga, se le pedirá que lo instale.</span><span class="sxs-lookup"><span data-stu-id="2de9f-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="2de9f-139">Una vez finalizada la instalación, es posible que deba reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2de9f-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="2de9f-140">Agregue el paquete de NuGet para la API de Web</span><span class="sxs-lookup"><span data-stu-id="2de9f-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="2de9f-141">Después de instalar el Administrador de paquetes de NuGet, agregue el paquete de Web API Self-Host al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2de9f-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="2de9f-142">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="2de9f-143">*Tenga en cuenta*: si no se ve este menú de elemento, asegúrese de que dicho administrador de paquetes de NuGet ha instalado correctamente.</span><span class="sxs-lookup"><span data-stu-id="2de9f-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="2de9f-144">Seleccione **administrar paquetes de NuGet para solución...**</span><span class="sxs-lookup"><span data-stu-id="2de9f-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="2de9f-145">En el **administrar paquetes de dato valioso** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2de9f-146">En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="2de9f-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="2de9f-147">Seleccione el paquete de ASP.NET Web API Self Host y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="2de9f-148">Una vez instalado el paquete, haga clic en **cerrar** para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="2de9f-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="2de9f-149">Asegúrese de instalar el paquete denominado Microsoft.AspNet.WebApi.SelfHost, no AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2de9f-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="2de9f-150">Crear el modelo y controlador</span><span class="sxs-lookup"><span data-stu-id="2de9f-150">Create the Model and Controller</span></span>

<span data-ttu-id="2de9f-151">Este tutorial utiliza las mismas clases del modelo y controlador como el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="2de9f-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="2de9f-152">Agregue una clase pública denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="2de9f-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="2de9f-153">Agregue una clase pública denominada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="2de9f-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="2de9f-154">Derivar de esta clase desde **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="2de9f-155">Para obtener más información sobre el código de este controlador, consulte el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="2de9f-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="2de9f-156">Este controlador define tres acciones GET:</span><span class="sxs-lookup"><span data-stu-id="2de9f-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="2de9f-157">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="2de9f-157">URI</span></span> | <span data-ttu-id="2de9f-158">Descripción</span><span class="sxs-lookup"><span data-stu-id="2de9f-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2de9f-159">productos/api /</span><span class="sxs-lookup"><span data-stu-id="2de9f-159">/api/products</span></span> | <span data-ttu-id="2de9f-160">Obtener una lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="2de9f-160">Get a list of all products.</span></span> |
| <span data-ttu-id="2de9f-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2de9f-161">/api/products/*id*</span></span> | <span data-ttu-id="2de9f-162">Obtener un producto por identificador.</span><span class="sxs-lookup"><span data-stu-id="2de9f-162">Get a product by ID.</span></span> |
| <span data-ttu-id="2de9f-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2de9f-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="2de9f-164">Obtener una lista de productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="2de9f-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="2de9f-165">Hospedar la API Web</span><span class="sxs-lookup"><span data-stu-id="2de9f-165">Host the Web API</span></span>

<span data-ttu-id="2de9f-166">Abra el archivo Program.cs y agregue las siguientes instrucciones using:</span><span class="sxs-lookup"><span data-stu-id="2de9f-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="2de9f-167">Agregue el código siguiente a la **programa** clase.</span><span class="sxs-lookup"><span data-stu-id="2de9f-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="2de9f-168">(Opcional) Agregar una reserva de Namespace de direcciones URL de HTTP</span><span class="sxs-lookup"><span data-stu-id="2de9f-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="2de9f-169">Esta aplicación de escucha a `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="2de9f-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="2de9f-170">De forma predeterminada, escucha en una dirección HTTP concreta requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="2de9f-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="2de9f-171">Al ejecutar el tutorial, por lo tanto, puede obtener este error: "HTTP no pudo registrar la dirección URL http://+:8080/" hay dos formas de evitar este error:</span><span class="sxs-lookup"><span data-stu-id="2de9f-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="2de9f-172">Ejecutar Visual Studio con permisos de administrador con permisos elevados, o</span><span class="sxs-lookup"><span data-stu-id="2de9f-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="2de9f-173">Utilice Netsh.exe para conceder los permisos de cuenta para reservar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="2de9f-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="2de9f-174">Para utilizar Netsh.exe, abra un símbolo del sistema con privilegios de administrador y escriba el comando siguiente: comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="2de9f-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="2de9f-175">donde *equipo* es la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="2de9f-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="2de9f-176">Cuando haya terminado de autohospedaje, asegúrese de eliminar la reserva:</span><span class="sxs-lookup"><span data-stu-id="2de9f-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="2de9f-177">Llamar a la API Web desde una aplicación de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="2de9f-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="2de9f-178">Vamos a escribir una aplicación de consola simple que llama a la API web.</span><span class="sxs-lookup"><span data-stu-id="2de9f-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="2de9f-179">Agregue un nuevo proyecto de aplicación de consola a la solución:</span><span class="sxs-lookup"><span data-stu-id="2de9f-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="2de9f-180">En el Explorador de soluciones, haga clic en la solución y seleccione **Agregar nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="2de9f-181">Crear una nueva aplicación de consola denominada &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="2de9f-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="2de9f-182">Use el Administrador de paquetes de NuGet para agregar el paquete de ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="2de9f-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="2de9f-183">En el menú Herramientas, seleccione **Administrador de paquetes de biblioteca**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="2de9f-184">Seleccione **administrar paquetes de NuGet para solución...**</span><span class="sxs-lookup"><span data-stu-id="2de9f-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="2de9f-185">En el **administrar paquetes de NuGet** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="2de9f-186">En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="2de9f-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="2de9f-187">Seleccione el paquete de bibliotecas de cliente de Microsoft ASP.NET Web API y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="2de9f-188">Agregue una referencia en ClientApp al proyecto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="2de9f-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="2de9f-189">En el Explorador de soluciones, haga clic en el proyecto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="2de9f-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="2de9f-190">Seleccione **Agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="2de9f-191">En el **Administrador de referencias** cuadro de diálogo, en **solución**, seleccione **proyectos**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="2de9f-192">Seleccione el proyecto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2de9f-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="2de9f-193">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="2de9f-194">Abra el archivo Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="2de9f-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="2de9f-195">Agregue el siguiente **con** instrucción:</span><span class="sxs-lookup"><span data-stu-id="2de9f-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="2de9f-196">Agregar una variable static **HttpClient** instancia:</span><span class="sxs-lookup"><span data-stu-id="2de9f-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="2de9f-197">Agregue los métodos siguientes para obtener una lista de todos los productos, un producto por el identificador de la lista y lista de productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="2de9f-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="2de9f-198">Cada uno de estos métodos sigue el mismo patrón:</span><span class="sxs-lookup"><span data-stu-id="2de9f-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="2de9f-199">Llame a **HttpClient.GetAsync** para enviar una solicitud GET al URI adecuado.</span><span class="sxs-lookup"><span data-stu-id="2de9f-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="2de9f-200">Llame a **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="2de9f-201">Este método produce una excepción si el estado de respuesta HTTP es un código de error.</span><span class="sxs-lookup"><span data-stu-id="2de9f-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="2de9f-202">Llame a **ReadAsAsync&lt;T&gt;**  para deserializar un tipo CLR de la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2de9f-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="2de9f-203">Este método es un método de extensión definido en **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="2de9f-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="2de9f-204">El **GetAsync** y **ReadAsAsync** son métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="2de9f-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="2de9f-205">Devuelven **tarea** objetos que representan la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="2de9f-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="2de9f-206">Obtener la **resultado** propiedad bloquea el subproceso hasta que se complete la operación.</span><span class="sxs-lookup"><span data-stu-id="2de9f-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="2de9f-207">Para obtener más información acerca del uso de HttpClient, incluida la forma de realizar llamadas sin bloqueo, vea [al llamar a un Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2de9f-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="2de9f-208">Antes de llamar a estos métodos, establezca la propiedad BaseAddress en la instancia HttpClient para "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="2de9f-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="2de9f-209">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2de9f-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="2de9f-210">Esto debe devolver la siguiente.</span><span class="sxs-lookup"><span data-stu-id="2de9f-210">This should output the following.</span></span> <span data-ttu-id="2de9f-211">(Recuerde que se ejecute la aplicación SelfHost primero).</span><span class="sxs-lookup"><span data-stu-id="2de9f-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
