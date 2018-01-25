---
title: "Vistas de núcleo de ASP.NET MVC"
author: ardalis
description: "Obtenga información acerca de cómo vistas controlan la presentación de datos de la aplicación y la interacción del usuario en MVC de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: dc36c76dbd7d82a926e39d8a8ab3a2a53b65d954
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="10506-103">Vistas de núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="10506-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="10506-104">Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="10506-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="10506-105">Este documento explica vistas utilizadas en las aplicaciones de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10506-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="10506-106">Para obtener información sobre las páginas de Razor, consulte [Introducción a las páginas Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="10506-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="10506-107">En el **M**odelo -**V**er -**C**patrón ontroller (MVC), el *vista* administra la interacción de usuario y la presentación de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="10506-108">Una vista es una plantilla HTML con incrustado [marcado Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="10506-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="10506-109">Marcado de Razor es código que interactúa con el marcado HTML para generar una página Web que se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="10506-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="10506-110">En el núcleo de ASP.NET MVC, las vistas son *.cshtml* archivos que usan el [lenguaje de programación de C#](/dotnet/csharp/) en el marcado de Razor.</span><span class="sxs-lookup"><span data-stu-id="10506-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="10506-111">Por lo general, ver los archivos se agrupan en carpetas con el nombre de cada una de la aplicación [controladores](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="10506-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="10506-112">Las carpetas se almacenan en un *vistas* carpeta en la raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="10506-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Carpeta de vistas en el Explorador de soluciones de Visual Studio está abierta con la carpeta de inicio abierta para mostrar los archivos About.cshtml, Contact.cshtml y Index.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="10506-114">El *inicio* controlador se representa mediante un *inicio* carpeta dentro de la *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="10506-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="10506-115">El *inicio* carpeta contiene las vistas para la *sobre*, *póngase en contacto con*, y *índice* páginas Web (página principal).</span><span class="sxs-lookup"><span data-stu-id="10506-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="10506-116">Cuando un usuario solicita uno de estos tres páginas Web, las acciones de controlador en el *inicio* controlador determinar cuál de las tres vistas se utiliza para crear y devolver una página Web para el usuario.</span><span class="sxs-lookup"><span data-stu-id="10506-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="10506-117">Use [diseños](xref:mvc/views/layout) para proporcionar las secciones de la página Web coherente y reducir la repetición del código.</span><span class="sxs-lookup"><span data-stu-id="10506-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="10506-118">Diseños suelen contengan el encabezado, los elementos de menú y de navegación y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="10506-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="10506-119">El encabezado y pie de página suelen contengan marcado reutilizable para muchos elementos de metadatos y vínculos a recursos de script y el estilo.</span><span class="sxs-lookup"><span data-stu-id="10506-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="10506-120">Diseños de ayudarle a evitar este marcado repetitivo en las vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="10506-121">[Las vistas parciales](xref:mvc/views/partial) reducir la duplicación de código mediante la administración de elementos reutilizables de vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="10506-122">Por ejemplo, una vista parcial es útil para una biografía del autor en un sitio Web de blog que aparece en varias vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="10506-123">Una biografía del autor es contenido de la vista normal y no requiere código para ejecutar con el fin de generar el contenido de la página Web.</span><span class="sxs-lookup"><span data-stu-id="10506-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="10506-124">Contenido de biografía del autor está disponible para la vista de forma independiente, el enlace de modelos para que el uso de una vista parcial para este tipo de contenido es ideal.</span><span class="sxs-lookup"><span data-stu-id="10506-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="10506-125">[Ver componentes](xref:mvc/views/view-components) son vistas similares en parcial que le permiten reducir código repetitivo, pero son adecuados para ver el contenido que requiere el código que se ejecuta en el servidor con el fin de representar la página Web.</span><span class="sxs-lookup"><span data-stu-id="10506-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="10506-126">Permite ver los componentes son útiles cuando el contenido representado requiere la interacción de la base de datos, como para un sitio Web de carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="10506-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="10506-127">Componentes de la vista no están limitados para enlace de modelo para generar la salida de la página Web.</span><span class="sxs-lookup"><span data-stu-id="10506-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="10506-128">Ventajas del uso de vistas</span><span class="sxs-lookup"><span data-stu-id="10506-128">Benefits of using views</span></span>

<span data-ttu-id="10506-129">Vistas ayudan a establecer una [ **S**eparation **o**f **C**oncerns (SoC) diseño](http://deviq.com/separation-of-concerns/) dentro de una aplicación MVC separando el marcado de la interfaz de usuario de otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="10506-130">Después de SoC diseño hace que la aplicación modular, que ofrece varias ventajas:</span><span class="sxs-lookup"><span data-stu-id="10506-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="10506-131">La aplicación es más fácil de mantener, ya que mejor se organizan.</span><span class="sxs-lookup"><span data-stu-id="10506-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="10506-132">Vistas generalmente se agrupan por la característica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="10506-133">Resulta más fácil encontrar vistas relacionadas cuando se trabaja en una característica.</span><span class="sxs-lookup"><span data-stu-id="10506-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="10506-134">Las partes de la aplicación están acopladas.</span><span class="sxs-lookup"><span data-stu-id="10506-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="10506-135">Puede crear y actualizar las vistas de la aplicación por separado de los componentes de acceso lógica y los datos de negocio.</span><span class="sxs-lookup"><span data-stu-id="10506-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="10506-136">Puede modificar las vistas de la aplicación sin necesidad de tener que actualizar otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="10506-137">Es más fácil de probar los elementos de interfaz de usuario de la aplicación, ya que las vistas son unidades independientes.</span><span class="sxs-lookup"><span data-stu-id="10506-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="10506-138">Debido a una mejor organización, es menos probable que deberá accidentalmente secciones de repetición de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="10506-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="10506-139">Crear una vista</span><span class="sxs-lookup"><span data-stu-id="10506-139">Creating a view</span></span>

<span data-ttu-id="10506-140">Se crean vistas que son específicas para un controlador en el *vistas / [ControllerName]* carpeta.</span><span class="sxs-lookup"><span data-stu-id="10506-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="10506-141">Vistas que se comparten entre los controladores se colocan en la *vistas/compartidas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="10506-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="10506-142">Para crear una vista, agregue un nuevo archivo y asígnele el mismo nombre que su acción de controlador asociado con la *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="10506-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="10506-143">Para crear una vista que se corresponde con el *sobre* acción en el *inicio* controlador, cree una *About.cshtml* un archivo en el *vistas/inicio*carpeta:</span><span class="sxs-lookup"><span data-stu-id="10506-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="10506-144">*Razor* marcado comienza con la `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="10506-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="10506-145">Código de ejecución instrucciones de C# mediante la colocación de C# de [bloques de código Razor](xref:mvc/views/razor#razor-code-blocks) desactivar por llaves (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="10506-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="10506-146">Por ejemplo, vea la asignación de "About" a `ViewData["Title"]` mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="10506-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="10506-147">Puede mostrar valores en HTML haciendo referencia simplemente el valor con el `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="10506-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="10506-148">Ver el contenido de la `<h2>` y `<h3>` elementos anteriores.</span><span class="sxs-lookup"><span data-stu-id="10506-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="10506-149">El contenido de la vista mostrado anteriormente es solo una parte de toda la página Web que se presenta al usuario.</span><span class="sxs-lookup"><span data-stu-id="10506-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="10506-150">El resto del diseño de la página y otros aspectos comunes de la vista se especifican en otros archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="10506-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="10506-151">Para obtener más información, consulte el [tema diseño](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="10506-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="10506-152">Cómo controladores especifican vistas</span><span class="sxs-lookup"><span data-stu-id="10506-152">How controllers specify views</span></span>

<span data-ttu-id="10506-153">Vistas normalmente se devuelven de acciones como un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), que es un tipo de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="10506-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="10506-154">El método de acción puede crear y devolver un `ViewResult` directamente, pero que normalmente no está listo.</span><span class="sxs-lookup"><span data-stu-id="10506-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="10506-155">Puesto que la mayoría de los controladores que se hereda de [controlador](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), simplemente se usa el `View` método auxiliar para devolver el `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="10506-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="10506-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="10506-156">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="10506-157">Esta acción devuelve el *About.cshtml* se muestra en la última sección de vista se representa como la página Web siguiente:</span><span class="sxs-lookup"><span data-stu-id="10506-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Acerca de la página representada en el explorador Edge](overview/_static/about-page.png)

<span data-ttu-id="10506-159">El `View` método auxiliar tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="10506-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="10506-160">También puede especificar:</span><span class="sxs-lookup"><span data-stu-id="10506-160">You can optionally specify:</span></span>

* <span data-ttu-id="10506-161">Una vista explícita para devolver:</span><span class="sxs-lookup"><span data-stu-id="10506-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="10506-162">A [modelo](xref:mvc/models/model-binding) para pasar a la la vista:</span><span class="sxs-lookup"><span data-stu-id="10506-162">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="10506-163">Una vista y un modelo:</span><span class="sxs-lookup"><span data-stu-id="10506-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="10506-164">Detección de vista</span><span class="sxs-lookup"><span data-stu-id="10506-164">View discovery</span></span>

<span data-ttu-id="10506-165">Cuando una acción devuelve una vista, un proceso llamado *detección de vista* tiene lugar.</span><span class="sxs-lookup"><span data-stu-id="10506-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="10506-166">Este proceso determina qué archivo de vista se utiliza en función del nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="10506-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="10506-167">El comportamiento predeterminado de la `View` (método) (`return View();`) debe devolver una vista con el mismo nombre que el método de acción desde la que se llama.</span><span class="sxs-lookup"><span data-stu-id="10506-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="10506-168">Por ejemplo, el *sobre* `ActionResult` nombre de método del controlador se usa para buscar un archivo de vista denominado *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="10506-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="10506-169">En primer lugar, el tiempo de ejecución busca en el *vistas / [ControllerName]* carpeta para la vista.</span><span class="sxs-lookup"><span data-stu-id="10506-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="10506-170">Si no encuentra una vista de búsqueda de coincidencias no existe, busca la *Shared* carpeta para la vista.</span><span class="sxs-lookup"><span data-stu-id="10506-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="10506-171">No importa si implícitamente devuelve el `ViewResult` con `return View();` o pasar explícitamente el nombre de la vista a la `View` método con `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="10506-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="10506-172">En ambos casos, la detección de vista busca un archivo de vista coincidente en este orden:</span><span class="sxs-lookup"><span data-stu-id="10506-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="10506-173">*Views/\[ControllerName]\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="10506-173">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="10506-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="10506-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="10506-175">Se puede proporcionar una ruta de acceso del archivo de vista en lugar de un nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="10506-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="10506-176">Si utiliza una ruta de acceso absoluta a partir de la raíz de la aplicación (si lo desea, a partir de "/" o "~ /"), el *.cshtml* extensión se debe especificar:</span><span class="sxs-lookup"><span data-stu-id="10506-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="10506-177">También puede usar una ruta de acceso relativa para especificar vistas en directorios distintos sin la *.cshtml* extensión.</span><span class="sxs-lookup"><span data-stu-id="10506-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="10506-178">Dentro de la `HomeController`, puede devolver el *índice* ver de su *administrar* vistas con una ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="10506-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="10506-179">De forma similar, puede indicar el directorio específico del controlador actual con el ". /" prefijo:</span><span class="sxs-lookup"><span data-stu-id="10506-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="10506-180">[Las vistas parciales](xref:mvc/views/partial) y [ver componentes](xref:mvc/views/view-components) utilizar mecanismos de detección similares (pero no idénticos).</span><span class="sxs-lookup"><span data-stu-id="10506-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="10506-181">Puede personalizar la convención predeterminada para cómo las vistas se encuentran dentro de la aplicación mediante el comparador [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="10506-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="10506-182">Detección de la vista se basa en Buscar archivos de la vista por nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="10506-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="10506-183">Si el sistema de archivos subyacente distingue mayúsculas de minúsculas, nombres de las vistas son probablemente con diferenciación entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="10506-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="10506-184">Para la compatibilidad entre los sistemas operativos, Coincidir mayúsculas y minúsculas entre el controlador y los nombres de acción y las carpetas de la vista asociada y nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="10506-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="10506-185">Si se produce un error que no se encuentra un archivo de vista mientras se trabaja con un sistema de archivos distingue mayúsculas de minúsculas, confirme que coincide con las mayúsculas y minúsculas entre el archivo de vista solicitada y el nombre de archivo real de la vista.</span><span class="sxs-lookup"><span data-stu-id="10506-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="10506-186">Siga el procedimiento recomendado de organizar la estructura de archivos para las vistas reflejar las relaciones existentes entre controladores, acciones y vistas para mayor claridad y mantenimiento.</span><span class="sxs-lookup"><span data-stu-id="10506-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="10506-187">Pasar datos a las vistas</span><span class="sxs-lookup"><span data-stu-id="10506-187">Passing data to views</span></span>

<span data-ttu-id="10506-188">Puede pasar datos a vistas que utilizan varios enfoques.</span><span class="sxs-lookup"><span data-stu-id="10506-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="10506-189">El enfoque más eficaz consiste en especificar un [modelo](xref:mvc/models/model-binding) tipo en la vista.</span><span class="sxs-lookup"><span data-stu-id="10506-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="10506-190">Este modelo se conoce normalmente como un *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="10506-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="10506-191">Pasar una instancia del tipo de modelo de vista a la vista de la acción.</span><span class="sxs-lookup"><span data-stu-id="10506-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="10506-192">Uso de un modelo de vista para pasar datos a una vista permite que la vista aprovechar las ventajas de *seguro* comprobación de tipos.</span><span class="sxs-lookup"><span data-stu-id="10506-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="10506-193">*Establecimiento inflexible de tipos* (o *fuertemente tipado*) significa que cada variable y la constante tienen un tipo definido de forma explícita (por ejemplo, `string`, `int`, o `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="10506-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="10506-194">Se comprueba la validez de los tipos utilizados en una vista en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="10506-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="10506-195">[Visual Studio](https://www.visualstudio.com/vs/) y [código de Visual Studio](https://code.visualstudio.com/) se enumeran los miembros de clase fuertemente tipada utilizando una característica denominada [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="10506-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="10506-196">Cuando desea ver las propiedades de un modelo de vista, escriba el nombre de variable para el modelo de vista seguido por un punto (`.`).</span><span class="sxs-lookup"><span data-stu-id="10506-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="10506-197">Esto ayuda a escribir código más rápidamente con menos errores.</span><span class="sxs-lookup"><span data-stu-id="10506-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="10506-198">Especifique un modelo con el `@model` directiva.</span><span class="sxs-lookup"><span data-stu-id="10506-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="10506-199">Utilizar el modelo con `@Model`:</span><span class="sxs-lookup"><span data-stu-id="10506-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="10506-200">Para proporcionar el modelo a la vista, el controlador lo pasa como un parámetro:</span><span class="sxs-lookup"><span data-stu-id="10506-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="10506-201">No hay ninguna restricción sobre los tipos de modelo que puede proporcionar a una vista.</span><span class="sxs-lookup"><span data-stu-id="10506-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="10506-202">Se recomienda usar **P** **O**ld **C**LR **O**viewmodels bjeto (POCO) con poca o ninguna comportamiento (métodos) definido.</span><span class="sxs-lookup"><span data-stu-id="10506-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="10506-203">Por lo general, las clases del modelo de vista se almacenan en la *modelos* carpeta o en otro *ViewModels* carpeta en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="10506-204">El *dirección* viewmodel usado en el ejemplo anterior es un viewmodel POCO almacenado en un archivo denominado *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="10506-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="10506-205">Nada le impide usar las mismas clases para los tipos de modelo de vista y los tipos de modelo de negocio.</span><span class="sxs-lookup"><span data-stu-id="10506-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="10506-206">Sin embargo, el uso de modelos independientes permite sus vistas variar de forma independiente de la lógica de negocios y datos de partes de acceso de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="10506-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="10506-207">Separación de los modelos y viewmodels también ofrece ventajas de seguridad cuando los modelos utilizan [enlace de modelo](xref:mvc/models/model-binding) y [validación](xref:mvc/models/validation) para los datos enviados a la aplicación por el usuario.</span><span class="sxs-lookup"><span data-stu-id="10506-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="10506-208">Datos débilmente tipada (ViewData y ViewBag)</span><span class="sxs-lookup"><span data-stu-id="10506-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="10506-209">Nota: `ViewBag` no está disponible en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="10506-209">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="10506-210">Además de vistas fuertemente tipadas, vistas tienen acceso a un *débilmente tipada* (también denominada *imprecisa*) recopilación de datos.</span><span class="sxs-lookup"><span data-stu-id="10506-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="10506-211">A diferencia de los tipos seguros, *tipos débiles* (o *perder tipos*) significa que no declara explícitamente el tipo de datos que esté utilizando.</span><span class="sxs-lookup"><span data-stu-id="10506-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="10506-212">Puede usar la recopilación de datos débilmente tipada para pasar pequeñas cantidades de datos dentro y fuera de los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="10506-213">Pasar datos entre un...</span><span class="sxs-lookup"><span data-stu-id="10506-213">Passing data between a ...</span></span>                        | <span data-ttu-id="10506-214">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10506-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="10506-215">Controlador y una vista</span><span class="sxs-lookup"><span data-stu-id="10506-215">Controller and a view</span></span>                             | <span data-ttu-id="10506-216">Rellenar una lista desplegable con los datos.</span><span class="sxs-lookup"><span data-stu-id="10506-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="10506-217">Vista y un [vista de diseño](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="10506-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="10506-218">Establecer el  **\<título >** contenido del elemento en la vista de diseño de un archivo de vista.</span><span class="sxs-lookup"><span data-stu-id="10506-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="10506-219">[Vista parcial](xref:mvc/views/partial) y una vista</span><span class="sxs-lookup"><span data-stu-id="10506-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="10506-220">Un widget que muestra datos basados en la página Web que el usuario solicitó.</span><span class="sxs-lookup"><span data-stu-id="10506-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="10506-221">Puede hacer referencia a esta colección a través del `ViewData` o `ViewBag` propiedades en los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="10506-222">El `ViewData` propiedad es un diccionario de objetos débilmente tipada.</span><span class="sxs-lookup"><span data-stu-id="10506-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="10506-223">El `ViewBag` propiedad es un contenedor alrededor de `ViewData` que proporciona las propiedades dinámicas para subyacente `ViewData` colección.</span><span class="sxs-lookup"><span data-stu-id="10506-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="10506-224">`ViewData`y `ViewBag` se resuelven de forma dinámica en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="10506-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="10506-225">Debido a que no ofrecen la comprobación de tipos de tiempo de compilación, ambos son generalmente más propensas a errores que el uso de un modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="10506-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="10506-226">Por esta razón, algunos desarrolladores prefieren mínimamente o nunca `ViewData` y `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="10506-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="10506-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="10506-227">**ViewData**</span></span>

<span data-ttu-id="10506-228">`ViewData`es un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) tiene acceso a través del objeto `string` claves.</span><span class="sxs-lookup"><span data-stu-id="10506-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="10506-229">Datos de cadena se pueden almacenar y utilizar directamente sin necesidad de una conversión, pero primero debe convertir otros `ViewData` valores a tipos específicos de objeto cuando se extrae de ellos.</span><span class="sxs-lookup"><span data-stu-id="10506-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="10506-230">Puede usar `ViewData` para pasar datos de controladores a vistas y en las vistas, incluidos los [vistas parciales](xref:mvc/views/partial) y [diseños](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="10506-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="10506-231">El siguiente es un ejemplo que establece los valores de un saludo y una dirección con `ViewData` en acción:</span><span class="sxs-lookup"><span data-stu-id="10506-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="10506-232">Trabajar con los datos en una vista:</span><span class="sxs-lookup"><span data-stu-id="10506-232">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="10506-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="10506-233">**ViewBag**</span></span>

<span data-ttu-id="10506-234">Nota: `ViewBag` no está disponible en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="10506-234">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="10506-235">`ViewBag`es un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objeto que proporciona acceso dinámico a los objetos almacenados en `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="10506-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="10506-236">`ViewBag`puede ser más cómodo trabajar con, ya que no requiere conversión.</span><span class="sxs-lookup"><span data-stu-id="10506-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="10506-237">En el ejemplo siguiente se muestra cómo usar `ViewBag` con el mismo resultado que con `ViewData` anteriormente:</span><span class="sxs-lookup"><span data-stu-id="10506-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="10506-238">**Usando ViewData y ViewBag simultáneamente**</span><span class="sxs-lookup"><span data-stu-id="10506-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="10506-239">Nota: `ViewBag` no está disponible en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="10506-239">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="10506-240">Puesto que `ViewData` y `ViewBag` hacen referencia al mismo subyacente `ViewData` colección, puede usar ambos `ViewData` y `ViewBag` y mezclar y combinar entre ellos al leer y escribir valores.</span><span class="sxs-lookup"><span data-stu-id="10506-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="10506-241">Establecer el título con `ViewBag` y la descripción mediante `ViewData` en la parte superior de un *About.cshtml* vista:</span><span class="sxs-lookup"><span data-stu-id="10506-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="10506-242">Leer las propiedades pero invertir el uso de `ViewData` y `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="10506-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="10506-243">En el *_Layout.cshtml* de archivos, obtenga el título usando `ViewData` y obtener la descripción utilizando `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="10506-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="10506-244">Recuerde que las cadenas no requieren una conversión para `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="10506-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="10506-245">Puede usar `@ViewData["Title"]` sin conversión alguna.</span><span class="sxs-lookup"><span data-stu-id="10506-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="10506-246">Utilizar `ViewData` y `ViewBag` en el mismo funciona de tiempo, como hace mezclar y hacer coincidir la lectura y escritura de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="10506-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="10506-247">Se representa el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="10506-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="10506-248">**Resumen de las diferencias entre ViewData y ViewBag**</span><span class="sxs-lookup"><span data-stu-id="10506-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="10506-249">`ViewBag`no está disponible en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="10506-249">`ViewBag` isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="10506-250">Se deriva de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), por lo que tiene propiedades de diccionario que pueden ser útiles, por ejemplo, `ContainsKey`, `Add`, `Remove`, y `Clear`.</span><span class="sxs-lookup"><span data-stu-id="10506-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="10506-251">Las claves del diccionario son cadenas, por lo que se permiten espacios en blanco.</span><span class="sxs-lookup"><span data-stu-id="10506-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="10506-252">Ejemplo: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="10506-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="10506-253">Cualquier tipo excepto un `string` deben convertirse en la vista que usa `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="10506-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="10506-254">Se deriva de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), por lo que permite la creación de propiedades dinámicas mediante la notación de puntos (`@ViewBag.SomeKey = <value or object>`), y no se requiere ninguna conversión.</span><span class="sxs-lookup"><span data-stu-id="10506-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="10506-255">La sintaxis de `ViewBag` resulta más rápida agregar controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="10506-256">Más sencillo comprobar si hay valores null.</span><span class="sxs-lookup"><span data-stu-id="10506-256">Simpler to check for null values.</span></span> <span data-ttu-id="10506-257">Ejemplo: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="10506-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="10506-258">**Cuándo utilizar ViewData o ViewBag**</span><span class="sxs-lookup"><span data-stu-id="10506-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="10506-259">Ambos `ViewData` y `ViewBag` son igualmente válidos enfoques para pasar pequeñas cantidades de datos entre controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="10506-260">La elección de cuál utilizar se basa en la preferencia.</span><span class="sxs-lookup"><span data-stu-id="10506-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="10506-261">Puede mezclar y combinar `ViewData` y `ViewBag` objetos, sin embargo, el código sea más fácil de leer y mantener con un enfoque que se utiliza de forma coherente.</span><span class="sxs-lookup"><span data-stu-id="10506-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="10506-262">Ambos enfoques son resuelto dinámicamente en tiempo de ejecución y, por tanto, son propensos a lo que produce errores en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="10506-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="10506-263">Algunos equipos de desarrollo evitarlos.</span><span class="sxs-lookup"><span data-stu-id="10506-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="10506-264">Vistas dinámicas</span><span class="sxs-lookup"><span data-stu-id="10506-264">Dynamic views</span></span>

<span data-ttu-id="10506-265">El tipo de vistas que no declaran un modelo con `@model` , pero que tiene una instancia de modelo que se les pasa (por ejemplo, `return View(Address);`) puede hacer referencia a propiedades la instancia dinámicamente:</span><span class="sxs-lookup"><span data-stu-id="10506-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="10506-266">Esta característica proporciona la flexibilidad, pero no ofrece protección de compilación ni en IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="10506-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="10506-267">Si la propiedad no existe, se produce un error en la generación de la página Web en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="10506-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="10506-268">Más características de vista</span><span class="sxs-lookup"><span data-stu-id="10506-268">More view features</span></span>

<span data-ttu-id="10506-269">[Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) resulta fácil agregar el comportamiento de servidor a las etiquetas HTML existentes.</span><span class="sxs-lookup"><span data-stu-id="10506-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="10506-270">El uso de aplicaciones auxiliares de etiquetas evita la necesidad de escribir código personalizado o aplicaciones auxiliares en las vistas.</span><span class="sxs-lookup"><span data-stu-id="10506-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="10506-271">Aplicaciones auxiliares de etiquetas se aplican como atributos a elementos HTML y hace caso omiso de editores que no pueden procesarlos.</span><span class="sxs-lookup"><span data-stu-id="10506-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="10506-272">Esto le permite editar y representar el marcado de la vista en una variedad de herramientas.</span><span class="sxs-lookup"><span data-stu-id="10506-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="10506-273">Generar marcado HTML personalizado se puede lograr con muchas aplicaciones auxiliares de HTML integrados.</span><span class="sxs-lookup"><span data-stu-id="10506-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="10506-274">Lógica de la interfaz de usuario más compleja puede administrarse mediante [ver componentes](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="10506-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="10506-275">Ver componentes proporcionan la misma SoC que controladores y vistas ofrecen.</span><span class="sxs-lookup"><span data-stu-id="10506-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="10506-276">Puede eliminar la necesidad de acciones y vistas que se encargan de datos que usa elementos comunes de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="10506-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="10506-277">Al igual que muchos otros aspectos de ASP.NET Core, vistas admiten [inyección de dependencia](xref:fundamentals/dependency-injection), lo que permite a los servicios estén [insertado en vistas](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="10506-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
