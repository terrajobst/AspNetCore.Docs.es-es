---
title: Componentes de vista en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo se usan los componentes de vista en ASP.NET Core y cómo agregarlos a las aplicaciones.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 2b196d8d46942604d1c85eb5f2f073661e5acb30
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278367"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="0cdf1-103">Componentes de vista en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cdf1-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="0cdf1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cdf1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0cdf1-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cdf1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="0cdf1-106">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-106">View components</span></span>

<span data-ttu-id="0cdf1-107">Los componentes de vista son similares a las vistas parciales, pero mucho más eficaces.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="0cdf1-108">Los componentes de vista no usan el enlace de modelos y solo dependen de los datos proporcionados cuando se les llama.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="0cdf1-109">Este artículo se escribió en torno a ASP.NET Core MVC, pero los componentes de vista también funcionan con páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="0cdf1-110">Un componente de vista:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-110">A view component:</span></span>

* <span data-ttu-id="0cdf1-111">Representa un fragmento en lugar de una respuesta completa.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="0cdf1-112">Incluye las mismas ventajas de separación de conceptos y capacidad de prueba que se encuentran entre un controlador y una vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="0cdf1-113">Puede tener parámetros y lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="0cdf1-114">Normalmente se invoca desde una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="0cdf1-115">Los componentes de vista están diseñados para cualquier lugar que tenga lógica de representación reutilizable demasiado compleja para una vista parcial, como:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="0cdf1-116">Menús de navegación dinámica</span><span class="sxs-lookup"><span data-stu-id="0cdf1-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="0cdf1-117">Nube de etiquetas (donde consulta la base de datos)</span><span class="sxs-lookup"><span data-stu-id="0cdf1-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="0cdf1-118">Panel de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="0cdf1-118">Login panel</span></span>
* <span data-ttu-id="0cdf1-119">Carro de la compra</span><span class="sxs-lookup"><span data-stu-id="0cdf1-119">Shopping cart</span></span>
* <span data-ttu-id="0cdf1-120">Artículos publicados recientemente</span><span class="sxs-lookup"><span data-stu-id="0cdf1-120">Recently published articles</span></span>
* <span data-ttu-id="0cdf1-121">Contenido de la barra lateral de un blog típico</span><span class="sxs-lookup"><span data-stu-id="0cdf1-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="0cdf1-122">Un panel de inicio de sesión que se representa en cada página y muestra los vínculos para iniciar o cerrar sesión, según el estado del usuario</span><span class="sxs-lookup"><span data-stu-id="0cdf1-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="0cdf1-123">Un componente de vista consta de dos partes: la clase (normalmente derivada de [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) y el resultado que devuelve (por lo general, una vista).</span><span class="sxs-lookup"><span data-stu-id="0cdf1-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="0cdf1-124">Al igual que los controladores, un componente de vista puede ser un POCO, pero la mayoría de los desarrolladores prefieren aprovechar las ventajas que ofrecen los métodos y las propiedades disponibles al derivar de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="0cdf1-125">Crear un componente de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-125">Creating a view component</span></span>

<span data-ttu-id="0cdf1-126">Esta sección contiene los requisitos de alto nivel para crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="0cdf1-127">Más adelante en el artículo examinaremos cada paso en detalle y crearemos un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="0cdf1-128">La clase de componente de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-128">The view component class</span></span>

<span data-ttu-id="0cdf1-129">Es posible crear una clase de componente de vista mediante una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="0cdf1-130">Derivar de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="0cdf1-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="0cdf1-131">Decorar una clase con el atributo `[ViewComponent]` o derivar de una clase con el atributo `[ViewComponent]`</span><span class="sxs-lookup"><span data-stu-id="0cdf1-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="0cdf1-132">Crear una clase cuyo nombre termina con el sufijo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="0cdf1-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="0cdf1-133">Al igual que los controladores, los componentes de vista deben ser clases públicas, no anidadas y no abstractas.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="0cdf1-134">El nombre del componente de vista es el nombre de la clase sin el sufijo "ViewComponent".</span><span class="sxs-lookup"><span data-stu-id="0cdf1-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="0cdf1-135">También se puede especificar explícitamente mediante la propiedad `ViewComponentAttribute.Name`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="0cdf1-136">Una clase de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-136">A view component class:</span></span>

* <span data-ttu-id="0cdf1-137">Es totalmente compatible con la [inserción de dependencias](../../fundamentals/dependency-injection.md) de constructor.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="0cdf1-138">No participa en el ciclo de vida del controlador, lo que significa que no se pueden usar [filtros](../controllers/filters.md) en un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="0cdf1-139">Métodos de componente de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-139">View component methods</span></span>

<span data-ttu-id="0cdf1-140">Un componente de vista define su lógica en un método `InvokeAsync` que devuelve `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="0cdf1-141">Los parámetros proceden directamente de la invocación del componente de vista, no del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="0cdf1-142">Un componente de vista nunca controla directamente una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-142">A view component never directly handles a request.</span></span> <span data-ttu-id="0cdf1-143">Por lo general, un componente de vista inicializa un modelo y lo pasa a una vista mediante una llamada al método `View`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="0cdf1-144">En resumen, los métodos de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-144">In summary, view component methods:</span></span>

* <span data-ttu-id="0cdf1-145">Definen un método `InvokeAsync` que devuelve `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="0cdf1-146">Por lo general, inicializan un modelo y lo pasan a una vista mediante una llamada al método `ViewComponent` `View`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="0cdf1-147">Los parámetros provienen del método que realiza la llamada, y no de HTTP, ya que no hay ningún enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="0cdf1-148">No son accesibles directamente como un punto de conexión HTTP, sino que se invocan desde el código (normalmente en una vista).</span><span class="sxs-lookup"><span data-stu-id="0cdf1-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="0cdf1-149">Un componente de vista nunca controla una solicitud.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-149">A view component never handles a request</span></span>
* <span data-ttu-id="0cdf1-150">Se sobrecargan en la firma, en lugar de los detalles de la solicitud HTTP actual.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="0cdf1-151">Ruta de búsqueda de la vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-151">View search path</span></span>

<span data-ttu-id="0cdf1-152">El tiempo de ejecución busca la vista en las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="0cdf1-153">Views/\<nombre_del_contrlador>/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista></span><span class="sxs-lookup"><span data-stu-id="0cdf1-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="0cdf1-154">Views/Shared/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista></span><span class="sxs-lookup"><span data-stu-id="0cdf1-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="0cdf1-155">El nombre de vista predeterminado para un componente de vista es *Default*, lo que significa que el archivo de vista normalmente se denominará *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="0cdf1-156">Puede especificar un nombre de vista diferente al crear el resultado del componente de vista o al llamar al método `View`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="0cdf1-157">Se recomienda que asigne al archivo de vista el nombre *Default.cshtml* y que use la ruta de acceso *Views/Shared/Components/\<nombre_del_componente_de_vista>/\<nombre_de_vista>*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="0cdf1-158">El componente de vista `PriorityList` usado en este ejemplo usa *Views/Shared/Components/PriorityList/Default.cshtml* para la vista del componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="0cdf1-159">Invocar un componente de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-159">Invoking a view component</span></span>

<span data-ttu-id="0cdf1-160">Para usar el componente de vista, llame a lo siguiente dentro de una vista:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="0cdf1-161">Los parámetros se pasarán al método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="0cdf1-162">El componente de vista `PriorityList` desarrollado en el artículo se invoca desde el archivo de vista *Views/Todo/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="0cdf1-163">En la tabla siguiente, se llama al método `InvokeAsync` con dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="0cdf1-164">Invocación de un componente de vista como una aplicación auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="0cdf1-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="0cdf1-165">Para ASP.NET Core 1.1 y versiones posteriores, puede invocar un componente de vista como una [aplicación auxiliar de etiquetas](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="0cdf1-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="0cdf1-166">Los parámetros de clase y método con grafía Pascal para las aplicaciones auxiliares de etiquetas se convierten a su [grafía kebab en minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="0cdf1-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="0cdf1-167">La aplicación auxiliar de etiquetas que va a invocar un componente de vista usa el elemento `<vc></vc>`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="0cdf1-168">El componente de vista se especifica de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="0cdf1-169">Nota: Para poder usar un componente de vista como una aplicación auxiliar de etiquetas, debe registrar el ensamblado que contiene el componente de vista mediante la directiva `@addTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="0cdf1-170">Por ejemplo, si el componente de vista está en un ensamblado denominado "MyWebApp", agregue la directiva siguiente al archivo `_ViewImports.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="0cdf1-171">Puede registrar un componente de vista como una aplicación auxiliar de etiquetas en cualquier archivo que haga referencia al componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="0cdf1-172">Vea [Administración del ámbito de las aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para más información sobre cómo registrar aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="0cdf1-173">El método `InvokeAsync` usado en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="0cdf1-174">En el marcado de la aplicación auxiliar de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="0cdf1-175">En el ejemplo anterior, el componente de vista `PriorityList` se convierte en `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="0cdf1-176">Los parámetros para el componente de vista se pasan como atributos en grafía kebab en minúsculas.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="0cdf1-177">Invocar un componente de vista directamente desde un controlador</span><span class="sxs-lookup"><span data-stu-id="0cdf1-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="0cdf1-178">Los componentes de vista suelen invocarse desde una vista, pero se pueden invocar directamente desde un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="0cdf1-179">Aunque los componentes de vista no definen puntos de conexión como controladores, se puede implementar fácilmente una acción del controlador que devuelva el contenido de `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="0cdf1-180">En este ejemplo, se llama al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="0cdf1-181">Tutorial: Creación de un componente de vista simple</span><span class="sxs-lookup"><span data-stu-id="0cdf1-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="0cdf1-182">[Descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compile y pruebe el código de inicio.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="0cdf1-183">Se trata de un proyecto simple con un controlador `Todo` que muestra una lista de *tareas pendientes*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de tareas pendientes](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="0cdf1-185">Agregar una clase ViewComponent</span><span class="sxs-lookup"><span data-stu-id="0cdf1-185">Add a ViewComponent class</span></span>

<span data-ttu-id="0cdf1-186">Cree una carpeta *ViewComponents* y agregue la siguiente clase `PriorityListViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="0cdf1-187">Notas sobre el código:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-187">Notes on the code:</span></span>

* <span data-ttu-id="0cdf1-188">Las clases de componentes de vista pueden estar contenidas en **cualquier** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="0cdf1-189">Dado que el nombre de clase PriorityList**ViewComponent** acaba con el sufijo **ViewComponent**, el tiempo de ejecución usará la cadena "PriorityList" cuando haga referencia al componente de clase desde una vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="0cdf1-190">Más adelante explicaremos esta cuestión con más detalle.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="0cdf1-191">El atributo `[ViewComponent]` puede cambiar el nombre usado para hacer referencia a un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="0cdf1-192">Por ejemplo, podríamos haber asignado a la clase el nombre `XYZ` y aplicado el atributo `ViewComponent`:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="0cdf1-193">El atributo `[ViewComponent]` anterior le indica al selector de componentes de vista que use el nombre `PriorityList` al buscar las vistas asociadas al componente y que use la cadena "PriorityList" al hacer referencia al componente de clase desde una vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="0cdf1-194">Más adelante explicaremos esta cuestión con más detalle.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="0cdf1-195">El componente usa la [inserción de dependencias](../../fundamentals/dependency-injection.md) para que el contexto de datos esté disponible.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="0cdf1-196">`InvokeAsync` expone un método al que se puede llamar desde una vista y puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="0cdf1-197">El método `InvokeAsync` devuelve el conjunto de elementos `ToDo` que cumplen los parámetros `isDone` y `maxPriority`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="0cdf1-198">Crear la vista de Razor del componente de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-198">Create the view component Razor view</span></span>

* <span data-ttu-id="0cdf1-199">Cree la carpeta *Views/Shared/Components*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="0cdf1-200">Esta carpeta **debe** denominarse *Components*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="0cdf1-201">Cree la carpeta *Views/Shared/Components/PriorityList*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="0cdf1-202">El nombre de esta carpeta debe coincidir con el nombre de la clase de componente de vista o con el nombre de la clase sin el sufijo (si se ha seguido la convención y se ha usado el sufijo *ViewComponent* en el nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="0cdf1-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="0cdf1-203">Si ha usado el atributo `ViewComponent`, el nombre de clase debe coincidir con la designación del atributo.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="0cdf1-204">Cree una vista de Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="0cdf1-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="0cdf1-205">La vista de Razor toma una lista de `TodoItem` y muestra estos elementos.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="0cdf1-206">Si el método `InvokeAsync` del componente de vista no pasa el nombre de la vista (como en nuestro ejemplo), se usa *Default* para el nombre de vista, según la convención.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="0cdf1-207">Más adelante en el tutorial veremos cómo pasar el nombre de la vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="0cdf1-208">Para reemplazar el estilo predeterminado de un controlador concreto, agregue una vista a la carpeta de vistas específicas del controlador (por ejemplo, *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="0cdf1-209">Si el componente de vista es específico del controlador, puede agregarlo a la carpeta específica del controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0cdf1-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="0cdf1-210">Agregue un elemento `div` que contenga una llamada al componente de lista de prioridad en la parte inferior del archivo *Views/Todo/index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="0cdf1-211">El marcado `@await Component.InvokeAsync` muestra la sintaxis para llamar a los componentes de vista.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="0cdf1-212">El primer argumento es el nombre del componente que se quiere invocar o llamar.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="0cdf1-213">Los parámetros siguientes se pasan al componente.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="0cdf1-214">`InvokeAsync` puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="0cdf1-215">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-215">Test the app.</span></span> <span data-ttu-id="0cdf1-216">En la imagen siguiente se muestra la lista de tareas pendientes y los elementos de prioridad:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-216">The following image shows the ToDo list and the priority items:</span></span>

![lista de tareas pendientes y elementos de prioridad](view-components/_static/pi.png)

<span data-ttu-id="0cdf1-218">También puede llamar al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementos de prioridad de la acción IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="0cdf1-220">Especificar un nombre de vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-220">Specifying a view name</span></span>

<span data-ttu-id="0cdf1-221">En algunos casos, puede ser necesario que un componente de vista complejo especifique una vista no predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="0cdf1-222">En el código siguiente se muestra cómo especificar la vista "PVC" desde el método `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="0cdf1-223">Actualice el método `InvokeAsync` en la clase `PriorityListViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="0cdf1-224">Copie el archivo *Views/Shared/Components/PriorityList/Default.cshtml* en una vista denominada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="0cdf1-225">Agregue un encabezado para indicar que se está usando la vista PVC.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="0cdf1-226">Actualice *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="0cdf1-227">Ejecute la aplicación y compruebe la vista PVC.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-227">Run the app and verify PVC view.</span></span>

![Componente de vista de prioridad](view-components/_static/pvc.png)

<span data-ttu-id="0cdf1-229">Si la vista PVC no se representa, compruebe que está llamando al componente de vista con prioridad cuatro o superior.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="0cdf1-230">Examinar la ruta de acceso de la vista</span><span class="sxs-lookup"><span data-stu-id="0cdf1-230">Examine the view path</span></span>

* <span data-ttu-id="0cdf1-231">Cambie el parámetro de prioridad a tres o menos para que no se devuelva la vista de prioridad.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="0cdf1-232">Cambie temporalmente el nombre de *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="0cdf1-233">Pruebe la aplicación. Obtendrá el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="0cdf1-234">Copie *Views/Todo/Components/PriorityList/1Default.cshtml* en *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="0cdf1-235">Agregue algún marcado a la vista de componentes de vista de la lista de tareas pendientes *Shared* para indicar que la vista está en la carpeta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="0cdf1-236">Pruebe la vista de componentes **Shared**.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-236">Test the **Shared** component view.</span></span>

![Salida de la lista de tareas pendientes con la vista de componentes Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="0cdf1-238">Evitar cadenas mágicas</span><span class="sxs-lookup"><span data-stu-id="0cdf1-238">Avoiding magic strings</span></span>

<span data-ttu-id="0cdf1-239">Si busca seguridad en tiempo de compilación, puede reemplazar el nombre del componente de vista codificado de forma rígida por el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="0cdf1-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="0cdf1-240">Cree el componente de vista sin el sufijo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="0cdf1-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="0cdf1-241">Agregue una instrucción `using` para su archivo de vista de Razor y use el operador `nameof`:</span><span class="sxs-lookup"><span data-stu-id="0cdf1-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="0cdf1-242">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0cdf1-242">Additional resources</span></span>

* [<span data-ttu-id="0cdf1-243">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="0cdf1-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
