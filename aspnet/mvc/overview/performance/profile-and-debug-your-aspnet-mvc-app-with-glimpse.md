---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Generar perfiles y depurar la aplicación de ASP.NET MVC con Glimpse | Microsoft Docs
author: Rick-Anderson
description: Glimpse es un próspero y aumentando la familia de paquetes de NuGet de código abierto que proporciona detallados de rendimiento, depuración e información de diagnóstico para ASP.NET un...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577293"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="0d266-103">Generar perfiles y depurar la aplicación de ASP.NET MVC con Glimpse</span><span class="sxs-lookup"><span data-stu-id="0d266-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="0d266-104">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="0d266-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="0d266-105">Glimpse es un próspero y aumentando la familia de paquetes de NuGet de código abierto que proporciona detallados de rendimiento, depuración e información de diagnóstico para aplicaciones de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0d266-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="0d266-106">Es muy fácil instalar, ultrarrápidas ligero y muestra las métricas clave de rendimiento en la parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="0d266-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="0d266-107">Permite explorar en profundidad de la aplicación cuando necesite averiguar qué está ocurriendo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="0d266-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="0d266-108">Glimpse proporciona tanto información valiosa y que se recomienda que utilizarlo durante su ciclo de desarrollo, incluidos el entorno de prueba de Azure.</span><span class="sxs-lookup"><span data-stu-id="0d266-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="0d266-109">Mientras [Fiddler](http://www.telerik.com/fiddler) y [herramientas de desarrollo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan un cliente, vista de Glimpse proporciona una vista detallada del servidor.</span><span class="sxs-lookup"><span data-stu-id="0d266-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="0d266-110">En este tutorial se centrará en utilizar los paquetes EF y un vistazo a ASP.NET MVC, pero hay muchos otros paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="0d266-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="0d266-111">Siempre que sea posible se vinculará a la correspondiente [vislumbrar docs](http://getglimpse.com/Docs/) que ayudan a mantener.</span><span class="sxs-lookup"><span data-stu-id="0d266-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="0d266-112">Glimpse es un proyecto de código abierto, también puede contribuir en el código fuente y los documentos.</span><span class="sxs-lookup"><span data-stu-id="0d266-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="0d266-113">Instalación de Glimpse</span><span class="sxs-lookup"><span data-stu-id="0d266-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="0d266-114">Habilitar visión para el host local</span><span class="sxs-lookup"><span data-stu-id="0d266-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="0d266-115">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="0d266-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="0d266-116">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="0d266-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="0d266-117">Rutas</span><span class="sxs-lookup"><span data-stu-id="0d266-117">Routes</span></span>](#route)
- [<span data-ttu-id="0d266-118">Uso de Glimpse en Azure</span><span class="sxs-lookup"><span data-stu-id="0d266-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="0d266-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0d266-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="0d266-120">Instalación de Glimpse</span><span class="sxs-lookup"><span data-stu-id="0d266-120">Installing Glimpse</span></span>

<span data-ttu-id="0d266-121">Puede instalar Glimpse desde la consola del Administrador de paquetes de NuGet o desde el **administrar paquetes de NuGet** consola.</span><span class="sxs-lookup"><span data-stu-id="0d266-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="0d266-122">Para esta demostración, deberá instalar los paquetes Mvc5 y EF6:</span><span class="sxs-lookup"><span data-stu-id="0d266-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalar Glimpse desde NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="0d266-124">Busque *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="0d266-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF desde el cuadro de diálogo de instalación de NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="0d266-126">Seleccionando **paquetes instalados**, puede ver los módulos dependientes Glimpse instalados:</span><span class="sxs-lookup"><span data-stu-id="0d266-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Instalar paquetes de Glimpse desde DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="0d266-128">El siguiente comando instala los módulos de Glimpse MVC5 y EF6 desde la consola del Administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="0d266-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="0d266-129">Habilitar visión para el host local</span><span class="sxs-lookup"><span data-stu-id="0d266-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="0d266-130">Vaya a http://localhost:&lt; número de puerto&gt;/glimpse.axd y haga clic en el <strong>activar Glimpse</strong> botón.</span><span class="sxs-lookup"><span data-stu-id="0d266-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Página de glimpse axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="0d266-132">Si tienes que muestra la barra de favoritos, puede arrastrar y colocar los botones de Glimpse y agregarlas como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="0d266-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE con Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="0d266-134">Ahora puede desplazarse la aplicación y el **cabezas de presentación** (HUD) se muestra en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="0d266-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Póngase en contacto con el administrador página con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="0d266-136">El [página HUD de Glimpse](http://getglimpse.com/Docs/Heads-up-Display) detalla la información de tiempo mostrada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="0d266-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="0d266-137">La muestra de datos el HUD rendimiento discreta puede avisarle de un problema inmediatamente - antes de obtener el ciclo de prueba.</span><span class="sxs-lookup"><span data-stu-id="0d266-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="0d266-138">Al hacer clic en el &quot;g&quot; abre el panel de Glimpse en la esquina inferior derecha:</span><span class="sxs-lookup"><span data-stu-id="0d266-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel de glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="0d266-140">En la imagen anterior, el [pestaña ejecución](http://getglimpse.com/Docs/Execution-Tab) está seleccionada, que muestra detalles de tiempo de las acciones y los filtros de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0d266-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="0d266-141">Puede ver mi [temporizador de inspección Detener filtro](http://www.nuget.org/packages/StopWatch/) a partir de la fase 6 de la canalización.</span><span class="sxs-lookup"><span data-stu-id="0d266-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="0d266-142">Si bien puede proporcionar mi temporizador ligero útil perfil o datos de control de tiempo, se pierde todo el tiempo invertido en la autorización y la representación de la vista.</span><span class="sxs-lookup"><span data-stu-id="0d266-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="0d266-143">Puede leer sobre mi temporizador en [de perfil y la hora de la aplicación de ASP.NET MVC hasta Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d266-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="0d266-144">El [pestañas](http://getglimpse.com/Docs/Tabs) página proporciona vínculos a información detallada sobre cada pestaña.</span><span class="sxs-lookup"><span data-stu-id="0d266-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="0d266-145">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="0d266-145">The Timeline tab</span></span>

<span data-ttu-id="0d266-146">He modificado de Tom Dykstra pendientes [tutorial EF 6 y MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con el código siguiente, cambie el controlador de instructors:</span><span class="sxs-lookup"><span data-stu-id="0d266-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="0d266-147">El código anterior me permite introducir en la cadena de consulta (`eager`) al control explícito o diligente la carga de datos.</span><span class="sxs-lookup"><span data-stu-id="0d266-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="0d266-148">En la imagen siguiente, se usa la carga explícita y la página de control de tiempo muestra cada inscripción que se cargan en el `Index` método de acción:</span><span class="sxs-lookup"><span data-stu-id="0d266-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="0d266-150">En el código siguiente, se especifica diligente y se captura cada inscripción después de la `Index` se denomina vista:</span><span class="sxs-lookup"><span data-stu-id="0d266-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![diligente se especifica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="0d266-152">Puede mantener el puntero sobre un segmento de tiempo para obtener información de tiempo detallada:</span><span class="sxs-lookup"><span data-stu-id="0d266-152">You can hover over a time segment to get detailed timing information:</span></span>

![al mantener el mouse para ver el control de tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="0d266-154">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="0d266-154">Model Binding</span></span>

<span data-ttu-id="0d266-155">El [pestaña modelo de enlace](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información que le ayudarán a comprender cómo se enlazan las variables de formulario y por qué algunos no están enlazados como cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="0d266-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="0d266-156">¿La imagen siguiente muestra el **?**</span><span class="sxs-lookup"><span data-stu-id="0d266-156">The image below shows the **?**</span></span> <span data-ttu-id="0d266-157">icono que se puede hacer clic para que aparezca la página de Ayuda de glimpse para esa característica.</span><span class="sxs-lookup"><span data-stu-id="0d266-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Eche un vistazo a la vista modelo de enlace](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="0d266-159">Rutas</span><span class="sxs-lookup"><span data-stu-id="0d266-159">Routes</span></span>

 <span data-ttu-id="0d266-160">La pestaña rutas vistazo puede le ayudará depurar y entender el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="0d266-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="0d266-161">En la imagen siguiente, se selecciona la ruta de producto (y lo muestra en verde, una convención de Glimpse).</span><span class="sxs-lookup"><span data-stu-id="0d266-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="0d266-162">![nombre del producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) también se muestran los tokens de datos, las áreas y restricciones de ruta.</span><span class="sxs-lookup"><span data-stu-id="0d266-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="0d266-163">Consulte [Glimpse rutas](http://getglimpse.com/Docs/Routes-Tab) y [enrutamiento mediante atributos en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="0d266-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="0d266-164">Uso de Glimpse en Azure</span><span class="sxs-lookup"><span data-stu-id="0d266-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="0d266-165">La directiva de seguridad predeterminada de Glimpse solo permite que los datos de observar lo que se muestran desde el host local.</span><span class="sxs-lookup"><span data-stu-id="0d266-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="0d266-166">Puede cambiar esta directiva de seguridad para que pueda ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure).</span><span class="sxs-lookup"><span data-stu-id="0d266-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="0d266-167">Para entornos de prueba en Azure, agregue la marca de resaltado hasta la parte inferior de la *web.confg* archivo para habilitar la perspectiva:</span><span class="sxs-lookup"><span data-stu-id="0d266-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="0d266-168">Con este cambio por sí solo, cualquier usuario puede ver los datos de Glimpse en un sitio remoto.</span><span class="sxs-lookup"><span data-stu-id="0d266-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="0d266-169">Considere la posibilidad de agregar el marcado anterior a un perfil de publicación, por lo que solo ha implementado un aplicada al usar ese perfil de publicación (por ejemplo, su proifle de prueba de Azure.) Para restringir los datos de Glimpse, agregaremos el `canViewGlimpseData` rol y permitir sólo a los usuarios con este rol para ver los datos de Glimpse.</span><span class="sxs-lookup"><span data-stu-id="0d266-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="0d266-170">Quite los comentarios de la *GlimpseSecurityPolicy.cs* y cambie el [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) llamar desde `Administrator` a la `canViewGlimpseData` rol:</span><span class="sxs-lookup"><span data-stu-id="0d266-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="0d266-171">Seguridad: los datos enriquecidos proporcionados por Glimpse podrían exponer la seguridad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d266-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="0d266-172">Microsoft no ha realizado una auditoría de seguridad de Glimpse para su uso en aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="0d266-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="0d266-173">Para obtener información sobre cómo agregar roles, consulte mi [implementar una aplicación web de ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span><span class="sxs-lookup"><span data-stu-id="0d266-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="0d266-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0d266-174">Additional Resources</span></span>

- [<span data-ttu-id="0d266-175">Implementar una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure</span><span class="sxs-lookup"><span data-stu-id="0d266-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="0d266-176">[Eche un vistazo configuración](http://getglimpse.com/Docs/Configuration) -página de documentación sobre la configuración de las fichas, directiva de tiempo de ejecución, registro y mucho más.</span><span class="sxs-lookup"><span data-stu-id="0d266-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
