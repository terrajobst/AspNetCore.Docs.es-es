---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Cree un perfil y depurar la aplicación de ASP.NET MVC con perspectiva | Documentos de Microsoft
author: Rick-Anderson
description: Perspectiva es un éxito y aumenta la familia de paquetes de NuGet de código abierto que proporciona rendimiento detallados, depuración e información de diagnóstico para ASP.NET un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872980"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="b9fbf-103">Cree un perfil y depurar la aplicación de ASP.NET MVC con perspectiva</span><span class="sxs-lookup"><span data-stu-id="b9fbf-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="b9fbf-104">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b9fbf-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="b9fbf-105">Perspectiva es un éxito y aumenta la familia de paquetes de NuGet de código abierto que proporciona rendimiento detallados, depuración e información de diagnóstico para las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="b9fbf-106">Es muy fácil de instalar, ultrarrápido ligero y muestra las métricas de rendimiento clave en la parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="b9fbf-107">Permite profundizar en la aplicación cuando necesite averiguar qué está sucediendo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="b9fbf-108">Perspectiva proporciona sea tan valiosa información que le recomendamos que use a lo largo del ciclo de desarrollo, incluidos el entorno de prueba de Azure.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="b9fbf-109">Mientras [Fiddler](http://www.telerik.com/fiddler) y [herramientas de desarrollo de F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan un cliente, vista de perspectiva proporciona una vista detallada del servidor.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="b9fbf-110">Este tutorial se centrará en utilizar los paquetes EF y vistazo ASP.NET MVC, pero hay muchos otros paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="b9fbf-111">Siempre que sea posible vinculará a la correspondiente [echarle documentos](http://getglimpse.com/Docs/) que ayudar a mantener.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="b9fbf-112">Perspectiva es un proyecto de código abierto, demasiado pueden contribuir a los documentos, el código fuente.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="b9fbf-113">Instalación de perspectiva</span><span class="sxs-lookup"><span data-stu-id="b9fbf-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="b9fbf-114">Habilitar la perspectiva para el host local</span><span class="sxs-lookup"><span data-stu-id="b9fbf-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="b9fbf-115">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="b9fbf-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="b9fbf-116">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b9fbf-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="b9fbf-117">Rutas</span><span class="sxs-lookup"><span data-stu-id="b9fbf-117">Routes</span></span>](#route)
- [<span data-ttu-id="b9fbf-118">Con la perspectiva en Azure</span><span class="sxs-lookup"><span data-stu-id="b9fbf-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="b9fbf-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b9fbf-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="b9fbf-120">Instalación de perspectiva</span><span class="sxs-lookup"><span data-stu-id="b9fbf-120">Installing Glimpse</span></span>

<span data-ttu-id="b9fbf-121">Puede instalar perspectiva desde la consola del Administrador de paquetes de NuGet o desde el **administrar paquetes de NuGet** consola.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="b9fbf-122">En esta demostración, instalarán los paquetes Mvc5 y EF6:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalar la perspectiva de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="b9fbf-124">Busque *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="b9fbf-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF desde el cuadro de diálogo de instalación de NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="b9fbf-126">Si selecciona **paquetes instalados**, puede ver los módulos dependientes de perspectiva instalados:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Instalar paquetes de perspectiva de cuadro de diálogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="b9fbf-128">El siguiente comando instala módulos Glimpse MVC5 y EF6 desde la consola de administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="b9fbf-129">Habilitar la perspectiva para el host local</span><span class="sxs-lookup"><span data-stu-id="b9fbf-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="b9fbf-130">Vaya a http://localhost: &lt;º de puerto&gt;/glimpse.axd y haga clic en el <strong>activar Glimpse</strong> botón.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Página de perspectiva axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="b9fbf-132">Si tienes que muestra la barra de favoritos, puede arrastrar y colocar los botones de la perspectiva y agregarlas como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer con boookmarklets de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="b9fbf-134">Ahora puede navegar por la aplicación y el **cabezales de una presentación** (HUD pantalla) se muestra en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Póngase en contacto con el administrador página con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="b9fbf-136">El [página perspectiva HUD](http://getglimpse.com/Docs/Heads-up-Display) se detalla la información de control de tiempo mostrada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="b9fbf-137">La muestra de datos el HUD de rendimiento discreto puede notificar a un problema inmediatamente - antes de llegar al ciclo de prueba.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="b9fbf-138">Al hacer clic en el &quot;g&quot; abre el panel de la perspectiva en la esquina inferior derecha:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="b9fbf-140">En la imagen anterior, el [ficha ejecución](http://getglimpse.com/Docs/Execution-Tab) está seleccionada, que muestra detalles de tiempo de las acciones y los filtros en la canalización.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="b9fbf-141">Puede ver mi [temporizador de filtro de inspección detener](http://www.nuget.org/packages/StopWatch/) empiezan en la fase 6 de la canalización.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="b9fbf-142">Aunque puede proporcionar el temporizador ligero útil perfil/datos de control de tiempo, pierde todo el tiempo empleado en la autorización y la representación de la vista.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="b9fbf-143">Puede leer sobre mi temporizador en [de perfil y la hora de la aplicación de ASP.NET MVC a Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9fbf-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="b9fbf-144">El [pestañas](http://getglimpse.com/Docs/Tabs) página proporciona vínculos a información detallada sobre cada ficha.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="b9fbf-145">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="b9fbf-145">The Timeline tab</span></span>

<span data-ttu-id="b9fbf-146">Modifiqué de Tom Dykstra pendientes [tutorial de 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con el código siguiente, cambiar el controlador de instructores:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="b9fbf-147">El código anterior me permite pasar en la cadena de consulta (`eager`) al control diligente o explícita la carga de datos.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="b9fbf-148">En la imagen siguiente, se utiliza la carga explícita y la página de control de tiempo muestra cada inscripción cargado en el `Index` método de acción:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="b9fbf-150">En el código siguiente, se especifica diligente y se captura cada inscripción después de la `Index` vista se denomina:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![de forma diligente](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="b9fbf-152">Puede desplazar el puntero sobre un segmento de tiempo para obtener información de tiempo detallada:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-152">You can hover over a time segment to get detailed timing information:</span></span>

![al mantener el mouse para ver el control de tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="b9fbf-154">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="b9fbf-154">Model Binding</span></span>

<span data-ttu-id="b9fbf-155">El [pestaña modelo de enlace](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información para ayudarle a entender cómo se enlazan las variables de formulario y por qué algunos no se enlazan tal y como se esperaría.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="b9fbf-156">¿La imagen siguiente muestra el **?**</span><span class="sxs-lookup"><span data-stu-id="b9fbf-156">The image below shows the **?**</span></span> <span data-ttu-id="b9fbf-157">icono, lo que puede hacer clic en para abrir la página de Ayuda de perspectiva para esa característica.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![enlace de vista de modelos de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="b9fbf-159">Rutas</span><span class="sxs-lookup"><span data-stu-id="b9fbf-159">Routes</span></span>

 <span data-ttu-id="b9fbf-160">La ficha rutas de perspectiva le puede permitirá depurar y entender el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="b9fbf-161">En la imagen siguiente, se selecciona la ruta de producto (y se muestra en verde, una convención de perspectiva).</span><span class="sxs-lookup"><span data-stu-id="b9fbf-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="b9fbf-162">![nombre de producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) también se muestran los tokens de datos, las áreas y las restricciones de ruta.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="b9fbf-163">Vea [Glimpse rutas](http://getglimpse.com/Docs/Routes-Tab) y [atributo enrutamiento en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="b9fbf-164">Con la perspectiva en Azure</span><span class="sxs-lookup"><span data-stu-id="b9fbf-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="b9fbf-165">La directiva de seguridad predeterminada de perspectiva permite solo vistazo datos mostrarán desde el host local.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="b9fbf-166">Puede cambiar esta directiva de seguridad para que pueda ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure).</span><span class="sxs-lookup"><span data-stu-id="b9fbf-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="b9fbf-167">Para entornos de prueba en Azure, agregue la marca de resaltado hasta la parte inferior de la *Web.config* archivo para habilitar la perspectiva:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="b9fbf-168">Con este cambio por sí sola, cualquier usuario puede ver los datos de la perspectiva en un sitio remoto.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="b9fbf-169">Considere la posibilidad de agregar el marcado anteriormente a un perfil de publicación para que solo ha implementado un aplicado cuando se usa ese perfil de publicación (por ejemplo, su proifle de prueba de Azure.) Para restringir los datos de la perspectiva, agregaremos el `canViewGlimpseData` rol y solo se permiten a los usuarios de este rol para ver los datos de la perspectiva.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="b9fbf-170">Quite los comentarios de la *GlimpseSecurityPolicy.cs* y cambie el [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) llamar desde `Administrator` a la `canViewGlimpseData` rol:</span><span class="sxs-lookup"><span data-stu-id="b9fbf-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="b9fbf-171">Seguridad: los datos enriquecidos proporcionados por perspectiva pudieron exponer la seguridad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="b9fbf-172">Microsoft no ha realizado una auditoría de seguridad de Glimpse para su uso en aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="b9fbf-173">Para obtener información sobre cómo agregar roles, consulte mi [implementar una aplicación web de Secure ASP.NET MVC 5 con pertenencia, OAuth y base de datos SQL en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="b9fbf-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b9fbf-174">Additional Resources</span></span>

- [<span data-ttu-id="b9fbf-175">Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL en Azure</span><span class="sxs-lookup"><span data-stu-id="b9fbf-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="b9fbf-176">[Configuración de echarle](http://getglimpse.com/Docs/Configuration) -página de documentación sobre la configuración de las fichas, directiva de tiempo de ejecución, el registro y mucho más.</span><span class="sxs-lookup"><span data-stu-id="b9fbf-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
