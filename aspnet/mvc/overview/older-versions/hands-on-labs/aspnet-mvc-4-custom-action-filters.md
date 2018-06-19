---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Los filtros de acción personalizada de ASP.NET MVC 4 | Documentos de Microsoft
author: rick-anderson
description: ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llama a un método de acción. Los filtros de acción son atributos personalizados tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877715"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="3a64f-104">Filtros de acción personalizada de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="3a64f-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="3a64f-105">Por [Web colonias equipo](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3a64f-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="3a64f-106">Descargar el Kit de aprendizaje de colonias de Web</span><span class="sxs-lookup"><span data-stu-id="3a64f-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="3a64f-107">ASP.NET MVC proporciona filtros de acción para ejecutar lógica de filtrado antes o después de llama a un método de acción.</span><span class="sxs-lookup"><span data-stu-id="3a64f-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="3a64f-108">Filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar comportamiento previo y posterior a la acción a los métodos de acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="3a64f-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="3a64f-109">En este laboratorio práctico creará un atributo de filtro de acción personalizada a la solución de MvcMusicStore para detectar las solicitudes del controlador y registra la actividad de un sitio en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="3a64f-110">Podrá agregar el filtro de registro por inyección de código a cualquier acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="3a64f-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="3a64f-111">Por último, verá la vista del registro que muestra la lista de los visitantes.</span><span class="sxs-lookup"><span data-stu-id="3a64f-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="3a64f-112">Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="3a64f-113">Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **Fundamentos de ASP.NET MVC 4** laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="3a64f-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="3a64f-114">Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="3a64f-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="3a64f-115">Está disponible en el proyecto específico para este laboratorio [filtros de acción personalizados de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="3a64f-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3a64f-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="3a64f-116">Objectives</span></span>

<span data-ttu-id="3a64f-117">En este laboratorio práctico, aprenderá cómo:</span><span class="sxs-lookup"><span data-stu-id="3a64f-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="3a64f-118">Cree un atributo de filtro de acción personalizada para ampliar las capacidades de filtrado</span><span class="sxs-lookup"><span data-stu-id="3a64f-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="3a64f-119">Aplicar un atributo de filtro personalizado mediante la inserción de un nivel específico</span><span class="sxs-lookup"><span data-stu-id="3a64f-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="3a64f-120">Registrar los filtros de acción personalizada global</span><span class="sxs-lookup"><span data-stu-id="3a64f-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3a64f-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3a64f-121">Prerequisites</span></span>

<span data-ttu-id="3a64f-122">Debe tener los elementos siguientes para completar esta práctica:</span><span class="sxs-lookup"><span data-stu-id="3a64f-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="3a64f-123">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="3a64f-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3a64f-124">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="3a64f-124">Setup</span></span>

<span data-ttu-id="3a64f-125">**Instalación de fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="3a64f-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="3a64f-126">Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3a64f-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="3a64f-127">Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.</span><span class="sxs-lookup"><span data-stu-id="3a64f-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="3a64f-128">Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="3a64f-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3a64f-129">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="3a64f-129">Exercises</span></span>

<span data-ttu-id="3a64f-130">Este laboratorio práctico se compone de los ejercicios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3a64f-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="3a64f-131">Ejercicio 1: Registrar acciones</span><span class="sxs-lookup"><span data-stu-id="3a64f-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="3a64f-132">Ejercicio 2: Administración de varios filtros de acción</span><span class="sxs-lookup"><span data-stu-id="3a64f-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="3a64f-133">Tiempo estimado para completar esta práctica: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="3a64f-134">Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="3a64f-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="3a64f-135">Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="3a64f-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="3a64f-136">Ejercicio 1: Registrar acciones</span><span class="sxs-lookup"><span data-stu-id="3a64f-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="3a64f-137">En este ejercicio, obtendrá información sobre cómo crear un filtro de registro de acción personalizada mediante el uso de proveedores de filtros de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3a64f-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="3a64f-138">Para ello aplicará un filtro de registro para el sitio de MusicStore que registra todas las actividades en los controladores seleccionados.</span><span class="sxs-lookup"><span data-stu-id="3a64f-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="3a64f-139">El filtro se ampliará **ActionFilterAttributeClass** e invalide **OnActionExecuting** método para detectar cada solicitud y, a continuación, realice las acciones de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="3a64f-140">Ejecutar métodos, resultados y parámetros se proporcionará información de contexto sobre solicitudes HTTP, ASP.NET MVC **ResultExecutingContext** clase **.**</span><span class="sxs-lookup"><span data-stu-id="3a64f-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="3a64f-141">ASP.NET MVC 4 también tiene proveedores de filtros predeterminados que puede utilizar sin crear un filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="3a64f-142">ASP.NET MVC 4 proporciona los siguientes tipos de filtros:</span><span class="sxs-lookup"><span data-stu-id="3a64f-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="3a64f-143">**Autorización** filtrar, que toma decisiones de seguridad sobre si se debe ejecutar un método de acción, como realizar la autenticación o validar las propiedades de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3a64f-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="3a64f-144">**Acción** filtro, que encapsula la ejecución del método de acción.</span><span class="sxs-lookup"><span data-stu-id="3a64f-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="3a64f-145">Este filtro puede llevar a cabo un procesamiento adicional, como proporcionar datos extra al método de acción, inspeccionar el valor devuelto o cancelar la ejecución del método de acción</span><span class="sxs-lookup"><span data-stu-id="3a64f-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="3a64f-146">**Resultado** filtro, que encapsula la ejecución del objeto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="3a64f-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="3a64f-147">Este filtro puede llevar a cabo un procesamiento adicional del resultado, como modificar la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3a64f-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="3a64f-148">**Excepción** filtro, que se ejecuta si hay una excepción no controlada producida en algún lugar en el método de acción, empezando por los filtros de autorización y finalizando con la ejecución del resultado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="3a64f-149">Filtros de excepciones se pueden utilizar para tareas como registrar o mostrar una página de error.</span><span class="sxs-lookup"><span data-stu-id="3a64f-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="3a64f-150">Para obtener más información acerca de los proveedores de filtros, visite este vínculo MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="3a64f-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="3a64f-151">Acerca de la característica de registro de aplicación de tienda de música de MVC</span><span class="sxs-lookup"><span data-stu-id="3a64f-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="3a64f-152">Esta solución de la tienda de música tiene una nueva tabla de modelo de datos para el registro de sitio, **ActionLog**, con los siguientes campos: nombre del controlador que recibió una solicitud, acción de llamadas, dirección IP del cliente y marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="3a64f-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="3a64f-153">![Modelo de datos. Tabla ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de datos. Tabla ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="3a64f-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="3a64f-154">*Modelos de datos, tabla de ActionLog*</span><span class="sxs-lookup"><span data-stu-id="3a64f-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="3a64f-155">La solución proporciona una vista de MVC de ASP.NET para el registro de acciones que se pueden encontrar en **MvcMusicStores/vistas/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="3a64f-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="3a64f-156">![Vista de registro de acción](aspnet-mvc-4-custom-action-filters/_static/image2.png "vista de registro de acciones")</span><span class="sxs-lookup"><span data-stu-id="3a64f-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="3a64f-157">*Vista de registro de acción*</span><span class="sxs-lookup"><span data-stu-id="3a64f-157">*Action Log view*</span></span>

<span data-ttu-id="3a64f-158">Con esto tiene la estructura, todo el trabajo se centrará en interrumpir la solicitud del controlador y realizar el registro mediante el uso de filtrado personalizado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="3a64f-159">Tarea 1: crear un filtro personalizado para detectar la solicitud del controlador</span><span class="sxs-lookup"><span data-stu-id="3a64f-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="3a64f-160">En esta tarea creará una clase de atributo de filtro personalizado que contendrá la lógica de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="3a64f-161">Para ello se ampliará ASP.NET MVC **ActionFilterAttribute** clase e implemente la interfaz **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="3a64f-162">El **ActionFilterAttribute** es la clase base para todos los filtros de atributo.</span><span class="sxs-lookup"><span data-stu-id="3a64f-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="3a64f-163">Proporciona los métodos siguientes para ejecutar una lógica específica después y antes de la ejecución de la acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="3a64f-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="3a64f-164">**OnActionExecuting**(ResultExecutingContext filterContext): justo antes de la acción se denomina método.</span><span class="sxs-lookup"><span data-stu-id="3a64f-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="3a64f-165">**OnActionExecuted**(ActionExecutedContext filterContext): una vez que se llama al método de acción y antes de que se ejecuta el resultado (antes de la representación de vista).</span><span class="sxs-lookup"><span data-stu-id="3a64f-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="3a64f-166">**OnResultExecuting**(ActionExecutingContext filterContext): justo antes de que se ejecuta el resultado (antes de la representación de vista).</span><span class="sxs-lookup"><span data-stu-id="3a64f-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="3a64f-167">**OnResultExecuted**(ResultExecutedContext filterContext): después de ejecuta el resultado (después de que se represente la vista).</span><span class="sxs-lookup"><span data-stu-id="3a64f-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="3a64f-168">Al reemplazar cualquiera de estos métodos en una clase derivada, es posible ejecutar su propio código de filtrado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="3a64f-169">Abra la **comenzar** solución ubicado en **\Source\Ex01-LoggingActions\Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="3a64f-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="3a64f-170">Debe descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="3a64f-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="3a64f-171">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="3a64f-172">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="3a64f-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="3a64f-173">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="3a64f-174">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a64f-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3a64f-175">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="3a64f-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3a64f-176">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="3a64f-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="3a64f-177">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="3a64f-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="3a64f-178">Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="3a64f-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="3a64f-179">Esta carpeta almacenará todos los filtros personalizados.</span><span class="sxs-lookup"><span data-stu-id="3a64f-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="3a64f-180">Abra **CustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacios de nombres:</span><span class="sxs-lookup"><span data-stu-id="3a64f-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="3a64f-181">(Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="3a64f-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="3a64f-182">Heredar la **CustomActionFilter** clase **ActionFilterAttribute** y, a continuación, realice **CustomActionFilter** clase implemente **IActionFilter** interfaz.</span><span class="sxs-lookup"><span data-stu-id="3a64f-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="3a64f-183">Asegúrese de **CustomActionFilter** clase invalide el método **OnActionExecuting** y agregue la lógica necesaria para registrar la ejecución del filtro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="3a64f-184">Para ello, agregue el código que aparece resaltado en **CustomActionFilter** clase.</span><span class="sxs-lookup"><span data-stu-id="3a64f-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="3a64f-185">(Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="3a64f-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="3a64f-186">**OnActionExecuting** método consiste en utilizar **Entity Framework** para agregar un nuevo registro de ActionLog.</span><span class="sxs-lookup"><span data-stu-id="3a64f-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="3a64f-187">Crea y rellena una nueva instancia de entidad con la información de contexto de **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="3a64f-188">Más información sobre **elemento ControllerContext** clase en [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a64f-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="3a64f-189">Tarea 2: insertar un Interceptor de código en la clase de controlador de almacén</span><span class="sxs-lookup"><span data-stu-id="3a64f-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="3a64f-190">En esta tarea, agregará el filtro personalizado insertando a todas las clases de controlador y las acciones de controlador que se registrarán.</span><span class="sxs-lookup"><span data-stu-id="3a64f-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="3a64f-191">En este ejercicio, la clase de controlador de almacén tendrá un registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="3a64f-192">El método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado se ejecuta cuando se llama a un elemento insertado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="3a64f-193">También es posible interceptar un método de un controlador específico.</span><span class="sxs-lookup"><span data-stu-id="3a64f-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="3a64f-194">Abra la **StoreController** en **MvcMusicStore\Controllers** y agregue una referencia a la **filtros** espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="3a64f-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="3a64f-195">Insertar el filtro personalizado **CustomActionFilter** en **StoreController** clase agregando **[CustomActionFilter]** atributo antes de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="3a64f-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="3a64f-196">Cuando se aplica un filtro en una clase de controlador, también se insertan todas sus acciones.</span><span class="sxs-lookup"><span data-stu-id="3a64f-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="3a64f-197">Si desea que se aplica el filtro sólo para un conjunto de acciones, tendría que insertar **[CustomActionFilter]** a cada uno de ellos:</span><span class="sxs-lookup"><span data-stu-id="3a64f-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="3a64f-198">Tarea 3: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3a64f-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="3a64f-199">En esta tarea, probará que funciona el filtro de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="3a64f-200">Se iniciará la aplicación y visitar la tienda y, a continuación, comprobará las actividades ha iniciado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="3a64f-201">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="3a64f-202">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro:</span><span class="sxs-lookup"><span data-stu-id="3a64f-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="3a64f-203">![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image3.png "estado de seguimiento antes de la actividad de las páginas del registro")</span><span class="sxs-lookup"><span data-stu-id="3a64f-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="3a64f-204">*Estado de seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="3a64f-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="3a64f-205">De forma predeterminada, siempre mostrará un elemento que se genera cuando se recuperan los géneros existentes para el menú.</span><span class="sxs-lookup"><span data-stu-id="3a64f-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="3a64f-206">Para fines de simplificar nos estamos limpiar el **ActionLog** tabla cada vez que se ejecuta la aplicación, por lo que sólo mostrará los registros de comprobación de cada tarea determinada.</span><span class="sxs-lookup"><span data-stu-id="3a64f-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="3a64f-207">Tendrá que quitar el código siguiente desde el **sesión\_iniciar** (método) (en el **Global.asax** clase), con el fin de guardar un registro histórico de todas las acciones que se ejecuta en el almacén Controlador.</span><span class="sxs-lookup"><span data-stu-id="3a64f-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="3a64f-208">Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="3a64f-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="3a64f-209">Vaya a **/ActionLog** y si el registro está vacío presione **F5** para actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="3a64f-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="3a64f-210">Compruebe que se realizó el seguimiento de las visitas:</span><span class="sxs-lookup"><span data-stu-id="3a64f-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="3a64f-211">![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image4.png "registro de acciones con el registro de actividad")</span><span class="sxs-lookup"><span data-stu-id="3a64f-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="3a64f-212">*Registro de acciones con el registro de actividad*</span><span class="sxs-lookup"><span data-stu-id="3a64f-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="3a64f-213">Ejercicio 2: Administración de varios filtros de acción</span><span class="sxs-lookup"><span data-stu-id="3a64f-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="3a64f-214">En este ejercicio agregará un segundo filtro de acción personalizado a la clase StoreController y definir el orden específico en el que se ejecutarán ambos filtros.</span><span class="sxs-lookup"><span data-stu-id="3a64f-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="3a64f-215">A continuación, actualizará el código para registrar el filtro global.</span><span class="sxs-lookup"><span data-stu-id="3a64f-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="3a64f-216">Hay diferentes opciones para tener en cuenta al definir el orden de ejecución de los filtros.</span><span class="sxs-lookup"><span data-stu-id="3a64f-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="3a64f-217">Por ejemplo, la propiedad orden y ámbito de los filtros:</span><span class="sxs-lookup"><span data-stu-id="3a64f-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="3a64f-218">Puede definir un **ámbito** para cada uno de los filtros, por ejemplo, puede definir el ámbito todos los filtros de acción para que se ejecute dentro de la **controlador ámbito**y todos los filtros de autorización para que se ejecute **ámbito Global** .</span><span class="sxs-lookup"><span data-stu-id="3a64f-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="3a64f-219">Los ámbitos tienen un orden de ejecución definido.</span><span class="sxs-lookup"><span data-stu-id="3a64f-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="3a64f-220">Además, cada filtro de acción tiene una propiedad de orden que se usa para determinar el orden de ejecución en el ámbito del filtro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="3a64f-221">Para obtener más información sobre el orden de ejecución de filtros de acción personalizados, visite este artículo MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="3a64f-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="3a64f-222">Tarea 1: Crear un nuevo filtro de acción personalizado</span><span class="sxs-lookup"><span data-stu-id="3a64f-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="3a64f-223">En esta tarea, creará un nuevo filtro de acción personalizado para insertar en la clase StoreController, obtener información sobre cómo administrar el orden de ejecución de los filtros.</span><span class="sxs-lookup"><span data-stu-id="3a64f-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="3a64f-224">Abra la **comenzar** solución ubicado en **\Source\Ex02-ManagingMultipleActionFilters\Begin** carpeta.</span><span class="sxs-lookup"><span data-stu-id="3a64f-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="3a64f-225">En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="3a64f-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="3a64f-226">Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="3a64f-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3a64f-227">Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3a64f-228">En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="3a64f-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3a64f-229">Por último, compile la solución haciendo clic en **generar** | **generar solución**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="3a64f-230">Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a64f-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3a64f-231">Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="3a64f-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3a64f-232">Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="3a64f-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="3a64f-233">Para obtener más información, consulte este artículo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="3a64f-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="3a64f-234">Agregue una nueva clase de C# en el **filtros** carpeta y asígnele el nombre *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="3a64f-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="3a64f-235">Abra **MyNewCustomActionFilter.cs** y agregue una referencia a **System.Web.Mvc** y **MvcMusicStore.Models** espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="3a64f-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="3a64f-236">(Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="3a64f-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="3a64f-237">Reemplace la declaración de clase predeterminada por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3a64f-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="3a64f-238">(Código de fragmento de código: *filtros de acción personalizada de ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="3a64f-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="3a64f-239">Este filtro de acción personalizado es casi idéntico a la que creó en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="3a64f-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="3a64f-240">La principal diferencia es que tiene el *&quot;iniciado por&quot;* atributo actualizado con el nombre de esta nueva clase para identificar el filtro wich registra el registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="3a64f-241">Tarea 2: Insertar un Interceptor de código nuevo en la clase StoreController</span><span class="sxs-lookup"><span data-stu-id="3a64f-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="3a64f-242">En esta tarea, agregará un nuevo filtro personalizado en la clase StoreController y ejecutar la solución para comprobar cómo ambos filtros funcionan juntos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="3a64f-243">Abra la **StoreController** clase ubicado en **MvcMusicStore\Controllers** e inserta el nuevo filtro personalizado **MyNewCustomActionFilter** en  **StoreController** clase como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3a64f-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="3a64f-244">Ahora, ejecute la aplicación para ver cómo funcionan estos dos filtros de acción personalizada.</span><span class="sxs-lookup"><span data-stu-id="3a64f-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="3a64f-245">Para ello, presione **F5** y espere hasta que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="3a64f-246">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="3a64f-247">![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image5.png "estado de seguimiento antes de la actividad de las páginas del registro")</span><span class="sxs-lookup"><span data-stu-id="3a64f-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="3a64f-248">*Estado de seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="3a64f-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="3a64f-249">Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="3a64f-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="3a64f-250">Compruebe que en este momento; las visitas se realizó el seguimiento dos veces: una vez para cada uno de los filtros de acción personalizado que agregó en el **controladora de almacenamiento** clase.</span><span class="sxs-lookup"><span data-stu-id="3a64f-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="3a64f-251">![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image6.png "registro de acciones con el registro de actividad")</span><span class="sxs-lookup"><span data-stu-id="3a64f-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="3a64f-252">*Registro de acciones con el registro de actividad*</span><span class="sxs-lookup"><span data-stu-id="3a64f-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="3a64f-253">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="3a64f-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="3a64f-254">Tarea 3: Administrar el orden de los filtros</span><span class="sxs-lookup"><span data-stu-id="3a64f-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="3a64f-255">En esta tarea, aprenderá a administrar el orden de ejecución de los filtros mediante la propiedad de orden.</span><span class="sxs-lookup"><span data-stu-id="3a64f-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="3a64f-256">Abra la **StoreController** clase ubicado en **MvcMusicStore\Controllers** y especifique la **orden** propiedad en ambos filtros como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="3a64f-257">Ahora, compruebe cómo se ejecutan los filtros según el valor de la propiedad de su orden.</span><span class="sxs-lookup"><span data-stu-id="3a64f-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="3a64f-258">Encontrará que el filtro con el valor de orden más pequeño (**CustomActionFilter**) es el primero que se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="3a64f-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="3a64f-259">Presione **F5** y espere hasta que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="3a64f-260">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="3a64f-261">![Estado de seguimiento antes de la actividad de las páginas del registro](aspnet-mvc-4-custom-action-filters/_static/image7.png "estado de seguimiento antes de la actividad de las páginas del registro")</span><span class="sxs-lookup"><span data-stu-id="3a64f-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="3a64f-262">*Estado de seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="3a64f-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="3a64f-263">Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="3a64f-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="3a64f-264">Compruebe que este momento, las visitas se realizó el seguimiento se ordenan por valor de orden de los filtros: **CustomActionFilter** registros primero.</span><span class="sxs-lookup"><span data-stu-id="3a64f-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="3a64f-265">![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image8.png "registro de acciones con el registro de actividad")</span><span class="sxs-lookup"><span data-stu-id="3a64f-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="3a64f-266">*Registro de acciones con el registro de actividad*</span><span class="sxs-lookup"><span data-stu-id="3a64f-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="3a64f-267">Ahora, debe actualizar el valor de orden de los filtros y comprobar cómo se cambia el orden de registro.</span><span class="sxs-lookup"><span data-stu-id="3a64f-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="3a64f-268">En el **StoreController** clase, actualice el valor de orden de los filtros como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="3a64f-269">Vuelva a ejecutar la aplicación presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="3a64f-270">Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="3a64f-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="3a64f-271">Compruebe que este momento, los registros crean por **MyNewCustomActionFilter** filtro aparece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="3a64f-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="3a64f-272">![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image9.png "registro de acciones con el registro de actividad")</span><span class="sxs-lookup"><span data-stu-id="3a64f-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="3a64f-273">*Registro de acciones con el registro de actividad*</span><span class="sxs-lookup"><span data-stu-id="3a64f-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="3a64f-274">Tarea 4: Registrar filtros global</span><span class="sxs-lookup"><span data-stu-id="3a64f-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="3a64f-275">En esta tarea, actualizará la solución para registrar el nuevo filtro (**MyNewCustomActionFilter**) como un filtro global.</span><span class="sxs-lookup"><span data-stu-id="3a64f-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="3a64f-276">Al hacerlo, se activará por la realiza acciones en la aplicación y no solo en el que StoreController como se muestra en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="3a64f-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="3a64f-277">En **StoreController** clase, quite **[MyNewCustomActionFilter]** atributo y la propiedad order de **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="3a64f-278">Debería ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a64f-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="3a64f-279">Abra **Global.asax** de archivos y busque la **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="3a64f-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="3a64f-280">Observe que cada vez que inicia la aplicación está registrando los filtros globales mediante una llamada a **RegisterGlobalFilters** método dentro de **FilterConfig** clase.</span><span class="sxs-lookup"><span data-stu-id="3a64f-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="3a64f-281">![Registro de filtros globales en Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrar filtros globales en Global.asax")</span><span class="sxs-lookup"><span data-stu-id="3a64f-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="3a64f-282">*Registro de filtros globales en Global.asax*</span><span class="sxs-lookup"><span data-stu-id="3a64f-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="3a64f-283">Abra **FilterConfig.cs** archivo dentro de **aplicación\_iniciar** carpeta.</span><span class="sxs-lookup"><span data-stu-id="3a64f-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="3a64f-284">Agregue una referencia al uso de System.Web.Mvc; uso de MvcMusicStore.Filters; espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="3a64f-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="3a64f-285">Actualización **RegisterGlobalFilters** método Agregar filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="3a64f-286">Para ello, agregue el código resaltado:</span><span class="sxs-lookup"><span data-stu-id="3a64f-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="3a64f-287">Ejecute la aplicación presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="3a64f-288">Haga clic en uno de los **géneros** en el menú y realizar algunas acciones, como navegar por un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="3a64f-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="3a64f-289">Compruebe que ahora **[MyNewCustomActionFilter]** es que se va a insertar en HomeController y ActionLogController demasiado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="3a64f-290">![Registro de acciones con el registro de actividad](aspnet-mvc-4-custom-action-filters/_static/image11.png "registro de acciones con el registro de actividad")</span><span class="sxs-lookup"><span data-stu-id="3a64f-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="3a64f-291">*Registro de acciones con registro de actividad global*</span><span class="sxs-lookup"><span data-stu-id="3a64f-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="3a64f-292">Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="3a64f-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3a64f-293">Resumen</span><span class="sxs-lookup"><span data-stu-id="3a64f-293">Summary</span></span>

<span data-ttu-id="3a64f-294">Al completar este laboratorio práctico ha aprendido cómo ampliar un filtro de acción para ejecutar acciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="3a64f-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="3a64f-295">También ha aprendido cómo insertar cualquier filtro para los controladores de página.</span><span class="sxs-lookup"><span data-stu-id="3a64f-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="3a64f-296">Se usaron los siguientes conceptos:</span><span class="sxs-lookup"><span data-stu-id="3a64f-296">The following concepts were used:</span></span>

- <span data-ttu-id="3a64f-297">Cómo crear filtros de acción personalizado con la clase ActionFilterAttribute de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3a64f-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="3a64f-298">Cómo insertar filtros en los controladores de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3a64f-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="3a64f-299">Cómo administrar el orden de los filtros utilizando la propiedad Order</span><span class="sxs-lookup"><span data-stu-id="3a64f-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="3a64f-300">Cómo registrar filtros global</span><span class="sxs-lookup"><span data-stu-id="3a64f-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="3a64f-301">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="3a64f-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="3a64f-302">Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="3a64f-303">Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="3a64f-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="3a64f-304">Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="3a64f-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3a64f-305">O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="3a64f-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="3a64f-306">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-306">Click on **Install Now**.</span></span> <span data-ttu-id="3a64f-307">Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.</span><span class="sxs-lookup"><span data-stu-id="3a64f-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3a64f-308">Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3a64f-309">![Instale Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instale Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="3a64f-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="3a64f-310">*Instale Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="3a64f-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="3a64f-311">Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.</span><span class="sxs-lookup"><span data-stu-id="3a64f-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Acepte los términos de licencia](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="3a64f-313">*Acepte los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="3a64f-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3a64f-314">Espere hasta que finalice el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-314">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="3a64f-316">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="3a64f-316">*Installation progress*</span></span>
6. <span data-ttu-id="3a64f-317">Cuando se complete la instalación, haga clic en **finalizar**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-317">When the installation completes, click **Finish**.</span></span>

    ![Se completó la instalación](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="3a64f-319">*Se completó la instalación*</span><span class="sxs-lookup"><span data-stu-id="3a64f-319">*Installation completed*</span></span>
7. <span data-ttu-id="3a64f-320">Haga clic en **Exit** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="3a64f-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="3a64f-321">Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.</span><span class="sxs-lookup"><span data-stu-id="3a64f-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Express de VS para icono Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="3a64f-323">*Express de VS para icono Web*</span><span class="sxs-lookup"><span data-stu-id="3a64f-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3a64f-324">Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3a64f-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="3a64f-325">Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3a64f-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="3a64f-326">Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure</span><span class="sxs-lookup"><span data-stu-id="3a64f-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="3a64f-327">Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.</span><span class="sxs-lookup"><span data-stu-id="3a64f-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3a64f-328">Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico.</span><span class="sxs-lookup"><span data-stu-id="3a64f-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="3a64f-329">Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="3a64f-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="3a64f-330">![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "inicie sesión en el portal de Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3a64f-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="3a64f-331">*Inicie sesión en el Portal de administración de Azure*</span><span class="sxs-lookup"><span data-stu-id="3a64f-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="3a64f-332">Haga clic en **New** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="3a64f-333">![Crear un nuevo sitio Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "crear un nuevo sitio Web")</span><span class="sxs-lookup"><span data-stu-id="3a64f-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="3a64f-334">*Crear un nuevo sitio Web*</span><span class="sxs-lookup"><span data-stu-id="3a64f-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="3a64f-335">Haga clic en **proceso** | **sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="3a64f-336">A continuación, seleccione **creación rápida** opción.</span><span class="sxs-lookup"><span data-stu-id="3a64f-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="3a64f-337">Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3a64f-338">Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="3a64f-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="3a64f-339">La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="3a64f-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="3a64f-340">No se incluyen pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="3a64f-341">![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "crear un nuevo sitio Web mediante Creación rápida")</span><span class="sxs-lookup"><span data-stu-id="3a64f-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="3a64f-342">*Crear un nuevo sitio Web mediante Creación rápida*</span><span class="sxs-lookup"><span data-stu-id="3a64f-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="3a64f-343">Espere hasta que el nuevo **sitio Web** se crea.</span><span class="sxs-lookup"><span data-stu-id="3a64f-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="3a64f-344">Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna.</span><span class="sxs-lookup"><span data-stu-id="3a64f-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="3a64f-345">Compruebe que funciona el nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="3a64f-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="3a64f-346">![En el nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image20.png "exploración al sitio web nuevo")</span><span class="sxs-lookup"><span data-stu-id="3a64f-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="3a64f-347">*Examinar el sitio web nuevo*</span><span class="sxs-lookup"><span data-stu-id="3a64f-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="3a64f-348">![Sitio Web que ejecute](aspnet-mvc-4-custom-action-filters/_static/image21.png "sitio Web que se ejecuta")</span><span class="sxs-lookup"><span data-stu-id="3a64f-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="3a64f-349">*Sitio Web que se ejecuta*</span><span class="sxs-lookup"><span data-stu-id="3a64f-349">*Web site running*</span></span>
6. <span data-ttu-id="3a64f-350">Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="3a64f-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="3a64f-351">![Abrir las páginas de administración del sitio web](aspnet-mvc-4-custom-action-filters/_static/image22.png "abrir las páginas de administración del sitio web")</span><span class="sxs-lookup"><span data-stu-id="3a64f-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="3a64f-352">*Abrir las páginas de administración del sitio Web*</span><span class="sxs-lookup"><span data-stu-id="3a64f-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="3a64f-353">En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.</span><span class="sxs-lookup"><span data-stu-id="3a64f-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3a64f-354">El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="3a64f-355">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="3a64f-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3a64f-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="3a64f-357">![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image23.png "descargar el sitio web de perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="3a64f-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="3a64f-358">*Descargar el sitio Web de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="3a64f-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="3a64f-359">Descargue el archivo de perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="3a64f-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="3a64f-360">Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3a64f-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="3a64f-361">![Guardar el archivo de perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image24.png "guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="3a64f-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="3a64f-362">*Guardar el archivo de perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="3a64f-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="3a64f-363">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="3a64f-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="3a64f-364">Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="3a64f-365">Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="3a64f-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="3a64f-366">Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a64f-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="3a64f-367">Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="3a64f-368">Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="3a64f-369">Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="3a64f-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="3a64f-370">No cree la base de datos, tal y como se creará en una fase posterior.</span><span class="sxs-lookup"><span data-stu-id="3a64f-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="3a64f-371">![Panel de servidor de base de datos SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "panel de base de datos SQL Server")</span><span class="sxs-lookup"><span data-stu-id="3a64f-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="3a64f-372">*Panel de base de datos SQL Server*</span><span class="sxs-lookup"><span data-stu-id="3a64f-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="3a64f-373">En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="3a64f-374">Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) botón.</span><span class="sxs-lookup"><span data-stu-id="3a64f-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Agregar dirección IP del cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="3a64f-376">*Agregar dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="3a64f-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="3a64f-377">Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="3a64f-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="3a64f-379">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="3a64f-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3a64f-380">Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3a64f-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="3a64f-381">Vuelva a la solución de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3a64f-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="3a64f-382">En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="3a64f-383">![Publicar la aplicación](aspnet-mvc-4-custom-action-filters/_static/image29.png "publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="3a64f-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="3a64f-384">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="3a64f-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="3a64f-385">Importar el perfil de publicación que se ha guardado en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="3a64f-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="3a64f-386">![Importar el perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image30.png "importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="3a64f-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="3a64f-387">*Importar perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="3a64f-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="3a64f-388">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-388">Click **Validate Connection**.</span></span> <span data-ttu-id="3a64f-389">Una vez completada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3a64f-390">Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.</span><span class="sxs-lookup"><span data-stu-id="3a64f-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="3a64f-391">![Validación de la conexión](aspnet-mvc-4-custom-action-filters/_static/image31.png "validación de la conexión")</span><span class="sxs-lookup"><span data-stu-id="3a64f-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="3a64f-392">*Validación de la conexión*</span><span class="sxs-lookup"><span data-stu-id="3a64f-392">*Validating connection*</span></span>
4. <span data-ttu-id="3a64f-393">En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="3a64f-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="3a64f-394">![Configuración de Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "configuración de Web deploy")</span><span class="sxs-lookup"><span data-stu-id="3a64f-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="3a64f-395">*Configuración de Web deploy*</span><span class="sxs-lookup"><span data-stu-id="3a64f-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="3a64f-396">Configure la conexión de base de datos de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a64f-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="3a64f-397">En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.</span><span class="sxs-lookup"><span data-stu-id="3a64f-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="3a64f-398">En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="3a64f-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="3a64f-399">En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.</span><span class="sxs-lookup"><span data-stu-id="3a64f-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="3a64f-400">Escriba un nuevo nombre de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a64f-400">Type a new database name.</span></span>

     <span data-ttu-id="3a64f-401">![Configurar la cadena de conexión de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "configurar la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="3a64f-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="3a64f-402">*Configurar la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="3a64f-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="3a64f-403">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-403">Then click **OK**.</span></span> <span data-ttu-id="3a64f-404">Cuando se le solicite para crear la base de datos, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="3a64f-405">![Crear la base de datos](aspnet-mvc-4-custom-action-filters/_static/image34.png "crear la cadena de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="3a64f-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="3a64f-406">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="3a64f-406">*Creating the database*</span></span>
7. <span data-ttu-id="3a64f-407">La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="3a64f-408">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-408">Then click **Next**.</span></span>

    <span data-ttu-id="3a64f-409">![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "cadena de conexión que apunte a la base de datos SQL")</span><span class="sxs-lookup"><span data-stu-id="3a64f-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="3a64f-410">*Cadena de conexión que apunte a la base de datos SQL*</span><span class="sxs-lookup"><span data-stu-id="3a64f-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="3a64f-411">En el **vista previa** página, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="3a64f-412">![Publicar la aplicación web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publicar la aplicación web")</span><span class="sxs-lookup"><span data-stu-id="3a64f-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="3a64f-413">*Publicar la aplicación web*</span><span class="sxs-lookup"><span data-stu-id="3a64f-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="3a64f-414">Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.</span><span class="sxs-lookup"><span data-stu-id="3a64f-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="3a64f-415">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="3a64f-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="3a64f-416">Con fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="3a64f-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="3a64f-417">El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="3a64f-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="3a64f-418">![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmentos de código en Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="3a64f-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="3a64f-419">*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="3a64f-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="3a64f-420">***Para agregar un fragmento de código mediante el teclado (solo C#)***</span><span class="sxs-lookup"><span data-stu-id="3a64f-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="3a64f-421">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="3a64f-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="3a64f-422">Comience a escribir el nombre del fragmento (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="3a64f-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="3a64f-423">Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="3a64f-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="3a64f-424">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="3a64f-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="3a64f-425">Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="3a64f-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="3a64f-426">![Comience a escribir el nombre del fragmento](aspnet-mvc-4-custom-action-filters/_static/image38.png "comience a escribir el nombre del fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="3a64f-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="3a64f-427">*Comience a escribir el nombre del fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="3a64f-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="3a64f-428">![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-custom-action-filters/_static/image39.png "presione Tab para seleccionar el fragmento de código resaltada")</span><span class="sxs-lookup"><span data-stu-id="3a64f-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="3a64f-429">*Presione la tecla Tab para seleccionar el fragmento de código resaltada*</span><span class="sxs-lookup"><span data-stu-id="3a64f-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="3a64f-430">![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-custom-action-filters/_static/image40.png "vuelva a presionar Tab y el fragmento de código se expandirán")</span><span class="sxs-lookup"><span data-stu-id="3a64f-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="3a64f-431">*Vuelva a presionar Tab y el fragmento de código se expandirán*</span><span class="sxs-lookup"><span data-stu-id="3a64f-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="3a64f-432">***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="3a64f-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="3a64f-433">Haga clic en donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="3a64f-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="3a64f-434">Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="3a64f-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="3a64f-435">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="3a64f-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="3a64f-436">![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-custom-action-filters/_static/image41.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")</span><span class="sxs-lookup"><span data-stu-id="3a64f-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="3a64f-437">*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*</span><span class="sxs-lookup"><span data-stu-id="3a64f-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="3a64f-438">![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-custom-action-filters/_static/image42.png "elegir el fragmento de código relevante de la lista haciendo clic en él")</span><span class="sxs-lookup"><span data-stu-id="3a64f-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="3a64f-439">*Seleccione el fragmento de código relevante de la lista haciendo clic en él*</span><span class="sxs-lookup"><span data-stu-id="3a64f-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
