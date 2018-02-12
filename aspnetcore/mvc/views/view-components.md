---
title: Componentes de vista
author: rick-anderson
description: "Los componentes de vista están diseñados para cualquier lugar que tenga lógica de representación reutilizable."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 27e77b8fa032c2b5be753a27db748b7499e27105
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="view-components"></a><span data-ttu-id="d1c3d-103">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-103">View components</span></span>

<span data-ttu-id="d1c3d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d1c3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d1c3d-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d1c3d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="d1c3d-106">Introducción a los componentes de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-106">Introducing view components</span></span>

<span data-ttu-id="d1c3d-107">Los componentes de vista son una novedad de ASP.NET Core MVC, similares a las vistas parciales pero mucho más eficaces.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="d1c3d-108">Los componentes de vista no usan el enlace de modelos y solo dependen de los datos proporcionados cuando se les llama.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="d1c3d-109">Un componente de vista:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-109">A view component:</span></span>

* <span data-ttu-id="d1c3d-110">Representa un fragmento en lugar de una respuesta completa.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-110">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="d1c3d-111">Incluye las mismas ventajas de separación de conceptos y capacidad de prueba que se encuentran entre un controlador y una vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-111">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="d1c3d-112">Puede tener parámetros y lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-112">Can have parameters and business logic.</span></span>
* <span data-ttu-id="d1c3d-113">Normalmente se invoca desde una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-113">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="d1c3d-114">Los componentes de vista están diseñados para cualquier lugar que tenga lógica de representación reutilizable demasiado compleja para una vista parcial, como:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="d1c3d-115">Menús de navegación dinámica</span><span class="sxs-lookup"><span data-stu-id="d1c3d-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="d1c3d-116">Nube de etiquetas (donde consulta la base de datos)</span><span class="sxs-lookup"><span data-stu-id="d1c3d-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="d1c3d-117">Panel de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="d1c3d-117">Login panel</span></span>
* <span data-ttu-id="d1c3d-118">Carro de la compra</span><span class="sxs-lookup"><span data-stu-id="d1c3d-118">Shopping cart</span></span>
* <span data-ttu-id="d1c3d-119">Artículos publicados recientemente</span><span class="sxs-lookup"><span data-stu-id="d1c3d-119">Recently published articles</span></span>
* <span data-ttu-id="d1c3d-120">Contenido de la barra lateral de un blog típico</span><span class="sxs-lookup"><span data-stu-id="d1c3d-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="d1c3d-121">Un panel de inicio de sesión que se representa en cada página y muestra los vínculos para iniciar o cerrar sesión, según el estado del usuario</span><span class="sxs-lookup"><span data-stu-id="d1c3d-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="d1c3d-122">Un componente de vista consta de dos partes: la clase (normalmente derivada de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) y el resultado que devuelve (por lo general, una vista).</span><span class="sxs-lookup"><span data-stu-id="d1c3d-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="d1c3d-123">Al igual que los controladores, un componente de vista puede ser un POCO, pero la mayoría de los desarrolladores prefieren aprovechar las ventajas que ofrecen los métodos y las propiedades disponibles al derivar de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="d1c3d-124">Crear un componente de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-124">Creating a view component</span></span>

<span data-ttu-id="d1c3d-125">Esta sección contiene los requisitos de alto nivel para crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="d1c3d-126">Más adelante en el artículo examinaremos cada paso en detalle y crearemos un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="d1c3d-127">La clase de componente de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-127">The view component class</span></span>

<span data-ttu-id="d1c3d-128">Es posible crear una clase de componente de vista mediante una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="d1c3d-129">Derivar de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="d1c3d-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="d1c3d-130">Decorar una clase con el atributo `[ViewComponent]` o derivar de una clase con el atributo `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="d1c3d-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="d1c3d-131">Crear una clase cuyo nombre termina con el sufijo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="d1c3d-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="d1c3d-132">Al igual que los controladores, los componentes de vista deben ser clases públicas, no anidadas y no abstractas.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="d1c3d-133">El nombre del componente de vista es el nombre de la clase sin el sufijo "ViewComponent".</span><span class="sxs-lookup"><span data-stu-id="d1c3d-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="d1c3d-134">También se puede especificar explícitamente mediante la propiedad `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="d1c3d-135">Una clase de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-135">A view component class:</span></span>

* <span data-ttu-id="d1c3d-136">Es totalmente compatible con la [inserción de dependencias](../../fundamentals/dependency-injection.md) de constructor.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="d1c3d-137">No participa en el ciclo de vida del controlador, lo que significa que no se pueden usar [filtros](../controllers/filters.md) en un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="d1c3d-138">Métodos de componente de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-138">View component methods</span></span>

<span data-ttu-id="d1c3d-139">Un componente de vista define su lógica en un método `InvokeAsync` que devuelve `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="d1c3d-140">Los parámetros proceden directamente de la invocación del componente de vista, no del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="d1c3d-141">Un componente de vista nunca controla directamente una solicitud.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-141">A view component never directly handles a request.</span></span> <span data-ttu-id="d1c3d-142">Por lo general, un componente de vista inicializa un modelo y lo pasa a una vista mediante una llamada al método `View`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="d1c3d-143">En resumen, los métodos de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-143">In summary, view component methods:</span></span>

* <span data-ttu-id="d1c3d-144">Definen un método `InvokeAsync` que devuelve `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="d1c3d-145">Por lo general, inicializan un modelo y lo pasan a una vista mediante una llamada al método `ViewComponent` `View`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="d1c3d-146">Los parámetros provienen del método que realiza la llamada, y no de HTTP, ya que no hay ningún enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="d1c3d-147">No son accesibles directamente como un punto de conexión HTTP, sino que se invocan desde el código (normalmente en una vista).</span><span class="sxs-lookup"><span data-stu-id="d1c3d-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="d1c3d-148">Un componente de vista nunca controla una solicitud.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-148">A view component never handles a request</span></span>
* <span data-ttu-id="d1c3d-149">Se sobrecargan en la firma, en lugar de los detalles de la solicitud HTTP actual.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="d1c3d-150">Ruta de búsqueda de la vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-150">View search path</span></span>

<span data-ttu-id="d1c3d-151">El tiempo de ejecución busca la vista en las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="d1c3d-152">Views/\<nombre_del_contrlador>/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista></span><span class="sxs-lookup"><span data-stu-id="d1c3d-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="d1c3d-153">Views/Shared/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista></span><span class="sxs-lookup"><span data-stu-id="d1c3d-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="d1c3d-154">El nombre de vista predeterminado para un componente de vista es *Default*, lo que significa que el archivo de vista normalmente se denominará *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="d1c3d-155">Puede especificar un nombre de vista diferente al crear el resultado del componente de vista o al llamar al método `View`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="d1c3d-156">Se recomienda que asigne al archivo de vista el nombre *Default.cshtml* y que use la ruta de acceso *Views/Shared/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista>*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="d1c3d-157">El componente de vista `PriorityList` usado en este ejemplo usa *Views/Shared/Components/PriorityList/Default.cshtml* para la vista del componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="d1c3d-158">Invocar un componente de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-158">Invoking a view component</span></span>

<span data-ttu-id="d1c3d-159">Para usar el componente de vista, llame a lo siguiente dentro de una vista:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="d1c3d-160">Los parámetros se pasarán al método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="d1c3d-161">El componente de vista `PriorityList` desarrollado en el artículo se invoca desde el archivo de vista *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="d1c3d-162">En la tabla siguiente, se llama al método `InvokeAsync` con dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="d1c3d-163">Invocación de un componente de vista como una aplicación auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="d1c3d-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="d1c3d-164">Para ASP.NET Core 1.1 y versiones posteriores, puede invocar un componente de vista como una [aplicación auxiliar de etiquetas](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="d1c3d-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="d1c3d-165">Los parámetros de clase y método con grafía Pascal para las aplicaciones auxiliares de etiquetas se convierten a su [grafía kebab en minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="d1c3d-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="d1c3d-166">La aplicación auxiliar de etiquetas que va a invocar un componente de vista usa el elemento `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="d1c3d-167">El componente de vista se especifica de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="d1c3d-168">Nota: Para poder usar un componente de vista como una aplicación auxiliar de etiquetas, debe registrar el ensamblado que contiene el componente de vista mediante la directiva `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="d1c3d-169">Por ejemplo, si el componente de vista está en un ensamblado denominado "MyWebApp", agregue la directiva siguiente al archivo `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="d1c3d-170">Puede registrar un componente de vista como una aplicación auxiliar de etiquetas en cualquier archivo que haga referencia al componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="d1c3d-171">Vea [Administración del ámbito de las aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para más información sobre cómo registrar aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="d1c3d-172">El método `InvokeAsync` usado en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="d1c3d-173">En el marcado de la aplicación auxiliar de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="d1c3d-174">En el ejemplo anterior, el componente de vista `PriorityList` se convierte en `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="d1c3d-175">Los parámetros para el componente de vista se pasan como atributos en grafía kebab en minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="d1c3d-176">Invocar un componente de vista directamente desde un controlador</span><span class="sxs-lookup"><span data-stu-id="d1c3d-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="d1c3d-177">Los componentes de vista suelen invocarse desde una vista, pero se pueden invocar directamente desde un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="d1c3d-178">Aunque los componentes de vista no definen puntos de conexión como controladores, se puede implementar fácilmente una acción del controlador que devuelva el contenido de `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="d1c3d-179">En este ejemplo, se llama al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="d1c3d-180">Tutorial: Creación de un componente de vista simple</span><span class="sxs-lookup"><span data-stu-id="d1c3d-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="d1c3d-181">[Descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compile y pruebe el código de inicio.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="d1c3d-182">Se trata de un proyecto simple con un controlador `Todo` que muestra una lista de *tareas pendientes*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de tareas pendientes](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="d1c3d-184">Agregar una clase ViewComponent</span><span class="sxs-lookup"><span data-stu-id="d1c3d-184">Add a ViewComponent class</span></span>

<span data-ttu-id="d1c3d-185">Cree una carpeta *ViewComponents* y agregue la siguiente clase `PriorityListViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="d1c3d-186">Notas sobre el código:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-186">Notes on the code:</span></span>

* <span data-ttu-id="d1c3d-187">Las clases de componentes de vista pueden estar contenidas en **cualquier** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="d1c3d-188">Dado que el nombre de clase PriorityList**ViewComponent** acaba con el sufijo **ViewComponent**, el tiempo de ejecución usará la cadena "PriorityList" cuando haga referencia al componente de clase desde una vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="d1c3d-189">Más adelante explicaremos esta cuestión con más detalle.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="d1c3d-190">El atributo `[ViewComponent]` puede cambiar el nombre usado para hacer referencia a un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="d1c3d-191">Por ejemplo, podríamos haber asignado a la clase el nombre `XYZ` y aplicado el atributo `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="d1c3d-192">El atributo `[ViewComponent]` anterior le indica al selector de componentes de vista que use el nombre `PriorityList` al buscar las vistas asociadas al componente y que use la cadena "PriorityList" al hacer referencia al componente de clase desde una vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="d1c3d-193">Más adelante explicaremos esta cuestión con más detalle.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="d1c3d-194">El componente usa la [inserción de dependencias](../../fundamentals/dependency-injection.md) para que el contexto de datos esté disponible.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="d1c3d-195">`InvokeAsync` expone un método al que se puede llamar desde una vista y puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="d1c3d-196">El método `InvokeAsync` devuelve el conjunto de elementos `ToDo` que cumplen los parámetros `isDone` y `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="d1c3d-197">Crear la vista de Razor del componente de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-197">Create the view component Razor view</span></span>

* <span data-ttu-id="d1c3d-198">Cree la carpeta *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="d1c3d-199">Esta carpeta **debe** denominarse *Components*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="d1c3d-200">Cree la carpeta *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="d1c3d-201">El nombre de esta carpeta debe coincidir con el nombre de la clase de componente de vista o con el nombre de la clase sin el sufijo (si se ha seguido la convención y se ha usado el sufijo *ViewComponent* en el nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="d1c3d-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="d1c3d-202">Si ha usado el atributo `ViewComponent`, el nombre de clase debe coincidir con la designación del atributo.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="d1c3d-203">Cree una vista de Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="d1c3d-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="d1c3d-204">La vista de Razor toma una lista de `TodoItem` y muestra estos elementos.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="d1c3d-205">Si el método `InvokeAsync` del componente de vista no pasa el nombre de la vista (como en nuestro ejemplo), se usa *Default* para el nombre de vista, según la convención.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="d1c3d-206">Más adelante en el tutorial veremos cómo pasar el nombre de la vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="d1c3d-207">Para reemplazar el estilo predeterminado de un controlador concreto, agregue una vista a la carpeta de vistas específicas del controlador (por ejemplo, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="d1c3d-208">Si el componente de vista es específico del controlador, puede agregarlo a la carpeta específica del controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d1c3d-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="d1c3d-209">Agregue un elemento `div` que contenga una llamada al componente de lista de prioridad en la parte inferior del archivo *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="d1c3d-210">El marcado `@await Component.InvokeAsync` muestra la sintaxis para llamar a los componentes de vista.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="d1c3d-211">El primer argumento es el nombre del componente que se quiere invocar o llamar.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="d1c3d-212">Los parámetros siguientes se pasan al componente.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="d1c3d-213">`InvokeAsync` puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="d1c3d-214">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-214">Test the app.</span></span> <span data-ttu-id="d1c3d-215">En la imagen siguiente se muestra la lista de tareas pendientes y los elementos de prioridad:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-215">The following image shows the ToDo list and the priority items:</span></span>

![lista de tareas pendientes y elementos de prioridad](view-components/_static/pi.png)

<span data-ttu-id="d1c3d-217">También puede llamar al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementos de prioridad de la acción IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="d1c3d-219">Especificar un nombre de vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-219">Specifying a view name</span></span>

<span data-ttu-id="d1c3d-220">En algunos casos, puede ser necesario que un componente de vista complejo especifique una vista no predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="d1c3d-221">En el código siguiente se muestra cómo especificar la vista "PVC" desde el método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="d1c3d-222">Actualice el método `InvokeAsync` en la clase `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="d1c3d-223">Copie el archivo *Views/Shared/Components/PriorityList/Default.cshtml* en una vista denominada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="d1c3d-224">Agregue un encabezado para indicar que se está usando la vista PVC.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="d1c3d-225">Actualice *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="d1c3d-226">Ejecute la aplicación y compruebe la vista PVC.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-226">Run the app and verify PVC view.</span></span>

![Componente de vista de prioridad](view-components/_static/pvc.png)

<span data-ttu-id="d1c3d-228">Si la vista PVC no se representa, compruebe que está llamando al componente de vista con prioridad cuatro o superior.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="d1c3d-229">Examinar la ruta de acceso de la vista</span><span class="sxs-lookup"><span data-stu-id="d1c3d-229">Examine the view path</span></span>

* <span data-ttu-id="d1c3d-230">Cambie el parámetro de prioridad a tres o menos para que no se devuelva la vista de prioridad.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="d1c3d-231">Cambie temporalmente el nombre de *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="d1c3d-232">Pruebe la aplicación. Obtendrá el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="d1c3d-233">Copie *Views/Todo/Components/PriorityList/1Default.cshtml* en *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="d1c3d-234">Agregue algún marcado a la vista de componentes de vista de la lista de tareas pendientes *Shared* para indicar que la vista está en la carpeta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="d1c3d-235">Pruebe la vista de componentes **Shared**.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-235">Test the **Shared** component view.</span></span>

![Salida de la lista de tareas pendientes con la vista de componentes Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="d1c3d-237">Evitar cadenas mágicas</span><span class="sxs-lookup"><span data-stu-id="d1c3d-237">Avoiding magic strings</span></span>

<span data-ttu-id="d1c3d-238">Si busca seguridad en tiempo de compilación, puede reemplazar el nombre del componente de vista codificado de forma rígida por el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="d1c3d-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="d1c3d-239">Cree el componente de vista sin el sufijo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="d1c3d-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="d1c3d-240">Agregue una instrucción `using` para su archivo de vista de Razor y use el operador `nameof`:</span><span class="sxs-lookup"><span data-stu-id="d1c3d-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="d1c3d-241">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d1c3d-241">Additional resources</span></span>

* [<span data-ttu-id="d1c3d-242">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="d1c3d-242">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
