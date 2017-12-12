---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Seguimiento en ASP.NET Web API 2 | Documentos de Microsoft
author: MikeWasson
description: "Muestra cómo habilitar el seguimiento en ASP.NET Web API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="e6dbd-103">Seguimiento en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e6dbd-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e6dbd-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6dbd-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e6dbd-105">Cuando se intenta depurar una aplicación basada en web, no hay ningún sustituto de un buen conjunto de registros de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="e6dbd-106">Este tutorial muestra cómo habilitar el seguimiento en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="e6dbd-107">Puede usar esta característica para realizar un seguimiento lo que hace el marco Web API antes y después invoca el controlador.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="e6dbd-108">También puede usar para realizar el seguimiento de su propio código.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e6dbd-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="e6dbd-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e6dbd-110">[Visual Studio de 2017](https://www.visualstudio.com/downloads/) (también funciona con Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="e6dbd-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="e6dbd-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="e6dbd-111">Web API 2</span></span>
> - [<span data-ttu-id="e6dbd-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="e6dbd-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="e6dbd-113">Habilitar System.Diagnostics seguimiento de API Web</span><span class="sxs-lookup"><span data-stu-id="e6dbd-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="e6dbd-114">En primer lugar, vamos a crear un nuevo proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="e6dbd-115">En Visual Studio, desde la **archivo** menú, seleccione **New**, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="e6dbd-116">En **plantillas**, **Web**, seleccione **aplicación Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e6dbd-117">Elija la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="e6dbd-118">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, **consola Administrar del paquete**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="e6dbd-119">En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="e6dbd-120">El primer comando instala el paquete de seguimiento de API Web más reciente.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="e6dbd-121">También actualiza los paquetes de Web API core.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="e6dbd-122">El segundo comando actualiza el paquete de WebApi.WebHost a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="e6dbd-123">Si desea tener como destino una versión específica de la API Web, use-marca de versión cuando se instala el paquete de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="e6dbd-124">Abra el archivo WebApiConfig.cs en la aplicación\_carpeta de inicio.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="e6dbd-125">Agregue el código siguiente a la **registrar** método.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="e6dbd-126">Este código agrega el [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) clase a la canalización de Web API.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="e6dbd-127">El **SystemDiagnosticsTraceWriter** clase escribe seguimientos a [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="e6dbd-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="e6dbd-128">Para ver los seguimientos, ejecute la aplicación en el depurador.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="e6dbd-129">En el explorador, vaya a `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="e6dbd-130">Las instrucciones de seguimiento se escriben en la ventana Resultados de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="e6dbd-131">(Desde el **vista** menú, seleccione **salida**).</span><span class="sxs-lookup"><span data-stu-id="e6dbd-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="e6dbd-132">Dado que **SystemDiagnosticsTraceWriter** escribe seguimientos a **System.Diagnostics.Trace**, puede registrar los agentes de escucha de seguimiento adicionales; por ejemplo escribir seguimientos en un archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="e6dbd-133">Para obtener más información acerca de los escritores de seguimiento, consulte el [los agentes de escucha de seguimiento](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) tema en MSDN.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="e6dbd-134">Configuración de SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="e6dbd-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="e6dbd-135">El código siguiente muestra cómo configurar el sistema de escritura de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e6dbd-136">Hay dos opciones de configuración que se pueden controlar:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="e6dbd-137">IsVerbose: Si es false, cada seguimiento contiene cierta información mínima.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="e6dbd-138">Si es true, los seguimientos incluir más información.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-138">If true, traces include more information.</span></span>
- <span data-ttu-id="e6dbd-139">MinimumLevel: Establece el nivel de seguimiento mínimo.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="e6dbd-140">Niveles de seguimiento, en orden, son depurar, información, advertencia, Error y Fatal.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="e6dbd-141">Agrega seguimientos a la aplicación de API Web</span><span class="sxs-lookup"><span data-stu-id="e6dbd-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="e6dbd-142">Agregar un autor de seguimiento proporciona acceso inmediato a los seguimientos creados por la canalización Web API.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="e6dbd-143">También puede usar el escritor de seguimiento para realizar el seguimiento de su propio código:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="e6dbd-144">Para obtener el escritor de seguimiento, llame a **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="e6dbd-145">Desde un controlador, este método es accesible a través de la **ApiController.Configuration** propiedad.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="e6dbd-146">Para escribir un seguimiento, puede llamar a la **ITraceWriter.Trace** método directamente, pero la [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) clase define algunos métodos de extensión que están más descriptivos.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="e6dbd-147">Por ejemplo, el **información** método mostrado anteriormente, crea un seguimiento con nivel de seguimiento **información**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="e6dbd-148">Infraestructura de seguimiento de API Web</span><span class="sxs-lookup"><span data-stu-id="e6dbd-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="e6dbd-149">En esta sección se describe cómo escribir un escritor de seguimiento personalizado de API Web.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="e6dbd-150">El paquete Microsoft.AspNet.WebApi.Tracing se basa en una infraestructura de seguimiento más general en Web API.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="e6dbd-151">En lugar de usar Microsoft.AspNet.WebApi.Tracing, también puede conectar en alguna otra biblioteca de seguimiento y registro, como [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="e6dbd-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="e6dbd-152">Para recopilar seguimientos, implementar la **ITraceWriter** interfaz.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="e6dbd-153">Este es un ejemplo simple:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e6dbd-154">El **ITraceWriter.Trace** método crea un seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="e6dbd-155">El autor de la llamada especifica un nivel de categoría y seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="e6dbd-156">La categoría puede ser cualquier cadena definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-156">The category can be any user-defined string.</span></span> <span data-ttu-id="e6dbd-157">La implementación de **seguimiento** debería hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="e6dbd-158">Crear un nuevo **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="e6dbd-159">Inicializar con la solicitud, la categoría y el nivel de seguimiento, tal como se muestra.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="e6dbd-160">Estos valores se proporcionan por el llamador.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="e6dbd-161">Invocar la *traceAction* delegar.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="e6dbd-162">Dentro de este delegado, se espera que el llamador para rellenar el resto de la **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="e6dbd-163">Escribir el **TraceRecord**, usando cualquier técnica de registro que desee.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="e6dbd-164">En el ejemplo se muestra aquí simplemente llama a **System.Diagnostics.Trace**.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="e6dbd-165">Establecer el escritor de seguimiento</span><span class="sxs-lookup"><span data-stu-id="e6dbd-165">Setting the Trace Writer</span></span>

<span data-ttu-id="e6dbd-166">Para habilitar el seguimiento, debe configurar la API de Web que se utilizará la **ITraceWriter** implementación.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="e6dbd-167">Lo hace a través de la **HttpConfiguration** de objeto, como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e6dbd-168">Escritor de seguimiento solo una puede estar activa.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-168">Only one trace writer can be active.</span></span> <span data-ttu-id="e6dbd-169">De forma predeterminada, conjuntos de API de Web un &quot;inefectiva&quot; seguimiento que no hace nada.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="e6dbd-170">(El &quot;inefectiva&quot; seguimiento existe para que no tengan código de seguimiento comprobar si el autor de seguimiento es **null** antes de escribir un seguimiento.)</span><span class="sxs-lookup"><span data-stu-id="e6dbd-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="e6dbd-171">Cómo Web API Tracing funciona</span><span class="sxs-lookup"><span data-stu-id="e6dbd-171">How Web API Tracing Works</span></span>

<span data-ttu-id="e6dbd-172">Seguimiento en usos de las API Web utiliza una API Web en un *fachada* patrón: cuando el seguimiento está habilitado, Web API salta distintas partes de la canalización de solicitudes con las clases que realizan llamadas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="e6dbd-173">Por ejemplo, al seleccionar un controlador, la canalización usa el **IHttpControllerSelector** interfaz.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="e6dbd-174">Con el seguimiento habilitado, la canalización de inserta una clase que implementa **IHttpControllerSelector** pero llamadas a través de la implementación real:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Seguimiento de API Web usa el patrón de fachada.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="e6dbd-176">Las ventajas de este diseño incluyen:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-176">The benefits of this design include:</span></span>

- <span data-ttu-id="e6dbd-177">Si no se agrega un escritor de seguimiento, los componentes de seguimiento no se crean instancias y no tienen ningún impacto de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="e6dbd-178">Si reemplaza servicios predeterminados como **IHttpControllerSelector** con su propia implementación personalizada, el seguimiento no se ve afectado, porque el seguimiento se realiza mediante el objeto de contenedor.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="e6dbd-179">También puede reemplazar el marco de trabajo de seguimiento de API Web completo con su propio marco de trabajo personalizado, sustituyendo el valor predeterminado **ITraceManager** servicio:</span><span class="sxs-lookup"><span data-stu-id="e6dbd-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="e6dbd-180">Implemente **ITraceManager.Initialize** para inicializar el sistema de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="e6dbd-181">Tenga en cuenta que esto reemplazará la *todo* framework de seguimiento, incluido todo el código de seguimiento que se integra en API Web.</span><span class="sxs-lookup"><span data-stu-id="e6dbd-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
