---
title: Componentes de la vista
author: rick-anderson
description: "Componentes de la vista pretenden en cualquier lugar que tiene lógica de representación reutilizable."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 65074ca02a1365db278d348d4e024121a6eb4634
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="view-components"></a><span data-ttu-id="b55d1-103">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-103">View components</span></span>

<span data-ttu-id="b55d1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b55d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b55d1-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b55d1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="b55d1-106">Componentes de la vista de presentación</span><span class="sxs-lookup"><span data-stu-id="b55d1-106">Introducing view components</span></span>

<span data-ttu-id="b55d1-107">Nuevo en MVC de ASP.NET Core, ver componentes son similares a las vistas parciales, pero son mucho más eficaces.</span><span class="sxs-lookup"><span data-stu-id="b55d1-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="b55d1-108">Componentes de la vista no usar enlace de modelos y sólo dependen de los datos proporcionados al llamar a en él.</span><span class="sxs-lookup"><span data-stu-id="b55d1-108">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="b55d1-109">Un componente de vista:</span><span class="sxs-lookup"><span data-stu-id="b55d1-109">A view component:</span></span>

* <span data-ttu-id="b55d1-110">Representa un fragmento en lugar de una respuesta toda</span><span class="sxs-lookup"><span data-stu-id="b55d1-110">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="b55d1-111">Incluye la misma separación de problemas y los beneficios de capacidad de prueba se encuentra entre un controlador y la vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-111">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="b55d1-112">Puede tener parámetros y lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="b55d1-112">Can have parameters and business logic</span></span>
* <span data-ttu-id="b55d1-113">Normalmente se invoca desde una página de diseño</span><span class="sxs-lookup"><span data-stu-id="b55d1-113">Is typically invoked from a layout page</span></span>

<span data-ttu-id="b55d1-114">Componentes de la vista se han diseñado en cualquier lugar que tiene lógica de representación reutilizable que es demasiado compleja para una vista parcial, como:</span><span class="sxs-lookup"><span data-stu-id="b55d1-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="b55d1-115">Menús de navegación dinámica</span><span class="sxs-lookup"><span data-stu-id="b55d1-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="b55d1-116">Nube de etiquetas (que consulta la base de datos)</span><span class="sxs-lookup"><span data-stu-id="b55d1-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="b55d1-117">Panel de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="b55d1-117">Login panel</span></span>
* <span data-ttu-id="b55d1-118">Carro de la compra</span><span class="sxs-lookup"><span data-stu-id="b55d1-118">Shopping cart</span></span>
* <span data-ttu-id="b55d1-119">Artículos publicados recientemente</span><span class="sxs-lookup"><span data-stu-id="b55d1-119">Recently published articles</span></span>
* <span data-ttu-id="b55d1-120">Contenido de Sidebar en un blog típico</span><span class="sxs-lookup"><span data-stu-id="b55d1-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="b55d1-121">Un panel de inicio de sesión que se representan en cada página y mostrar los vínculos entre ambos para cerrar la sesión o de registro, según el registro de estado del usuario</span><span class="sxs-lookup"><span data-stu-id="b55d1-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="b55d1-122">Un componente de vista consta de dos partes: la clase (normalmente derivado de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) y lo devuelve (normalmente una vista).</span><span class="sxs-lookup"><span data-stu-id="b55d1-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="b55d1-123">Al igual que los controladores, un componente de vista puede ser un POCO, pero la mayoría de los desarrolladores desee aprovechar las ventajas de los métodos y propiedades disponibles derivando de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="b55d1-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="b55d1-124">Crear un componente de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-124">Creating a view component</span></span>

<span data-ttu-id="b55d1-125">Esta sección contiene los requisitos de alto nivel para crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="b55d1-126">Más adelante en el artículo, se deberá examinar cada paso en detalle y crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="b55d1-127">La clase de componente de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-127">The view component class</span></span>

<span data-ttu-id="b55d1-128">Una clase de componente de vista se puede crear por cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="b55d1-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="b55d1-129">Derivar de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="b55d1-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="b55d1-130">Al decorar una clase con el `[ViewComponent]` atributo o derivar de una clase con el `[ViewComponent]` atributo</span><span class="sxs-lookup"><span data-stu-id="b55d1-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="b55d1-131">Crear una clase donde el nombre termina con el sufijo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="b55d1-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="b55d1-132">Como controladores, componentes de la vista deben ser clases públicas, no anidada y no abstracta.</span><span class="sxs-lookup"><span data-stu-id="b55d1-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="b55d1-133">El nombre del componente de vista es el nombre de clase con el sufijo "ViewComponent" quitado.</span><span class="sxs-lookup"><span data-stu-id="b55d1-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="b55d1-134">También se puede especificar explícitamente mediante el `ViewComponentAttribute.Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b55d1-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="b55d1-135">Una clase de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="b55d1-135">A view component class:</span></span>

* <span data-ttu-id="b55d1-136">Es totalmente compatible con constructor [inyección de dependencia](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="b55d1-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="b55d1-137">No participar en el ciclo de vida de controlador, lo que significa que no se puede utilizar [filtros](../controllers/filters.md) en un componente de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="b55d1-138">Ver los métodos de componente</span><span class="sxs-lookup"><span data-stu-id="b55d1-138">View component methods</span></span>

<span data-ttu-id="b55d1-139">Un componente de vista define su lógica en una `InvokeAsync` método que devuelve un `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="b55d1-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="b55d1-140">Parámetros proceden directamente de invocación del componente de vista, no desde el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="b55d1-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="b55d1-141">Un componente de vista nunca directamente controla una solicitud.</span><span class="sxs-lookup"><span data-stu-id="b55d1-141">A view component never directly handles a request.</span></span> <span data-ttu-id="b55d1-142">Por lo general, inicializa un modelo de un componente de vista y la pasa a una vista mediante una llamada a la `View` método.</span><span class="sxs-lookup"><span data-stu-id="b55d1-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="b55d1-143">En resumen, ver los métodos de componente:</span><span class="sxs-lookup"><span data-stu-id="b55d1-143">In summary, view component methods:</span></span>

* <span data-ttu-id="b55d1-144">Definir una `InvokeAsync` método que devuelva una`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="b55d1-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="b55d1-145">Inicializa un modelo normalmente y la pasa a una vista mediante una llamada a la `ViewComponent` `View` (método)</span><span class="sxs-lookup"><span data-stu-id="b55d1-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="b55d1-146">Parámetros provienen del método que realiza la llamada, y no HTTP, no hay ningún enlace de modelo</span><span class="sxs-lookup"><span data-stu-id="b55d1-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="b55d1-147">Son no es accesible directamente como un extremo HTTP, le invoca desde el código (normalmente en una vista).</span><span class="sxs-lookup"><span data-stu-id="b55d1-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="b55d1-148">Un componente de vista nunca controla una solicitud</span><span class="sxs-lookup"><span data-stu-id="b55d1-148">A view component never handles a request</span></span>
* <span data-ttu-id="b55d1-149">Están sobrecargados en la firma en lugar de los detalles de la solicitud HTTP actual</span><span class="sxs-lookup"><span data-stu-id="b55d1-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="b55d1-150">Ruta de acceso de búsqueda de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-150">View search path</span></span>

<span data-ttu-id="b55d1-151">El tiempo de ejecución busca la vista en las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="b55d1-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="b55d1-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="b55d1-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="b55d1-153">Views/Shared/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="b55d1-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="b55d1-154">El nombre de vista predeterminado para un componente de vista es *predeterminado*, lo que significa que el archivo de vista se suele denominar *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="b55d1-155">Puede especificar un nombre de vista diferente al crear el resultado del componente de vista o cuando se llama a la `View` método.</span><span class="sxs-lookup"><span data-stu-id="b55d1-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="b55d1-156">Se recomienda que un nombre al archivo de vista *Default.cshtml* y use la *vistas / / componentes de uso compartido/\<view_component_name > /\<view_name >* ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="b55d1-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="b55d1-157">El `PriorityList` componente de vista utilizado en este ejemplo utiliza *Views/Shared/Components/PriorityList/Default.cshtml* para la vista de componentes de la vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="b55d1-158">Invocar un componente de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-158">Invoking a view component</span></span>

<span data-ttu-id="b55d1-159">Para utilizar el componente de vista, llame a los siguientes dentro de una vista:</span><span class="sxs-lookup"><span data-stu-id="b55d1-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="b55d1-160">Los parámetros se pasará a la `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="b55d1-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="b55d1-161">El `PriorityList` desarrollado en el artículo del componente de vista se invoca desde la *Views/Todo/Index.cshtml* ver el archivo.</span><span class="sxs-lookup"><span data-stu-id="b55d1-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="b55d1-162">En la siguiente tabla, el `InvokeAsync` se denomina método con dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="b55d1-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="b55d1-163">Invocar un componente de vista como una aplicación auxiliar de etiqueta</span><span class="sxs-lookup"><span data-stu-id="b55d1-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="b55d1-164">Para ASP.NET Core 1.1 y versiones posteriores, puede invocar un componente de vista como una [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="b55d1-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="b55d1-165">Parámetros de clase y método la grafía Pascal para aplicaciones auxiliares de etiquetas se convierten en sus [reducir caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="b55d1-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="b55d1-166">La aplicación auxiliar de etiquetas para invocar un componente de vista utiliza la `<vc></vc>` elemento.</span><span class="sxs-lookup"><span data-stu-id="b55d1-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="b55d1-167">El componente de vista se especifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="b55d1-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="b55d1-168">Nota: Para poder usar un componente de vista como una aplicación auxiliar de etiquetas, debe registrar el ensamblado que contiene el componente de vista mediante el `@addTagHelper` directiva.</span><span class="sxs-lookup"><span data-stu-id="b55d1-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="b55d1-169">Por ejemplo, si el componente de la vista está en un ensamblado denominado "MyWebApp", agregue la siguiente directiva a la `_ViewImports.cshtml` archivo:</span><span class="sxs-lookup"><span data-stu-id="b55d1-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="b55d1-170">Puede registrar un componente de vista como una aplicación auxiliar de etiqueta a cualquier archivo que hace referencia al componente de vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="b55d1-171">Vea [administración de ámbito de aplicación auxiliar de la etiqueta](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obtener más información sobre cómo registrar aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="b55d1-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="b55d1-172">El `InvokeAsync` método utilizado en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="b55d1-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="b55d1-173">En el marcado de la aplicación auxiliar de etiqueta:</span><span class="sxs-lookup"><span data-stu-id="b55d1-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="b55d1-174">En el ejemplo anterior, el `PriorityList` del componente vista se convierte en `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="b55d1-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="b55d1-175">Los parámetros para el componente de vista se pasan como atributos en minúsculas kebab.</span><span class="sxs-lookup"><span data-stu-id="b55d1-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="b55d1-176">Invocar un componente de vista directamente desde un controlador</span><span class="sxs-lookup"><span data-stu-id="b55d1-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="b55d1-177">Ver componentes normalmente se invocan desde una vista, pero se puede invocar directamente desde un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="b55d1-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="b55d1-178">Mientras que los componentes de la vista no definen extremos como controladores, puede implementar fácilmente una acción de controlador que devuelve el contenido de un `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="b55d1-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="b55d1-179">En este ejemplo, el componente de vista se denomina directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="b55d1-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="b55d1-180">Tutorial: Crear un componente de vista simple</span><span class="sxs-lookup"><span data-stu-id="b55d1-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="b55d1-181">[Descargar](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilar y probar el código de inicio.</span><span class="sxs-lookup"><span data-stu-id="b55d1-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="b55d1-182">Es un proyecto simple con un `Todo` controlador que muestra una lista de *tareas* elementos.</span><span class="sxs-lookup"><span data-stu-id="b55d1-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="b55d1-184">Agregue una clase ViewComponent</span><span class="sxs-lookup"><span data-stu-id="b55d1-184">Add a ViewComponent class</span></span>

<span data-ttu-id="b55d1-185">Crear un *ViewComponents* carpeta y agregue las siguientes `PriorityListViewComponent` clase:</span><span class="sxs-lookup"><span data-stu-id="b55d1-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="b55d1-186">Notas sobre el código:</span><span class="sxs-lookup"><span data-stu-id="b55d1-186">Notes on the code:</span></span>

* <span data-ttu-id="b55d1-187">Clases de componentes de vista pueden estar contenidas en **cualquier** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b55d1-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="b55d1-188">Dado que el nombre de clase PriorityList**ViewComponent** termina con el sufijo **ViewComponent**, el tiempo de ejecución utilizará la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="b55d1-189">Explicaremos con más detalle más adelante.</span><span class="sxs-lookup"><span data-stu-id="b55d1-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="b55d1-190">El `[ViewComponent]` atributos pueden cambiar el nombre utilizado para hacer referencia a un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="b55d1-191">Por ejemplo, se podríamos haber con el nombre la clase `XYZ` y aplica la `ViewComponent` atributo:</span><span class="sxs-lookup"><span data-stu-id="b55d1-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="b55d1-192">El `[ViewComponent]` atributo anterior indica el selector de componente de vista para utilizar el nombre `PriorityList` al buscar las vistas asociadas con el componente y utilizar la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="b55d1-193">Explicaremos con más detalle más adelante.</span><span class="sxs-lookup"><span data-stu-id="b55d1-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="b55d1-194">El componente usa [inyección de dependencia](../../fundamentals/dependency-injection.md) para que esté disponible el contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="b55d1-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="b55d1-195">`InvokeAsync`expone un método que se puede llamar desde una vista y puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="b55d1-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="b55d1-196">El `InvokeAsync` método devuelve el conjunto de `ToDo` los elementos que cumplen el `isDone` y `maxPriority` parámetros.</span><span class="sxs-lookup"><span data-stu-id="b55d1-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="b55d1-197">Crear la vista de Razor del componente de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-197">Create the view component Razor view</span></span>

* <span data-ttu-id="b55d1-198">Crear el *vistas/Shared/componentes* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b55d1-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="b55d1-199">Esta carpeta **debe** denominarse *componentes*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="b55d1-200">Crear el *vistas/Shared/componentes/PriorityList* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b55d1-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="b55d1-201">Este nombre de carpeta debe coincidir con el nombre de la clase de componente de vista o el nombre de la clase menos el sufijo (si se siguió convención y usa el *ViewComponent* sufijo en el nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="b55d1-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="b55d1-202">Si ha usado la `ViewComponent` atributo, necesitaría el nombre de clase para que coincida con la designación de atributo.</span><span class="sxs-lookup"><span data-stu-id="b55d1-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="b55d1-203">Crear un *Views/Shared/Components/PriorityList/Default.cshtml* vista Razor:[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b55d1-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="b55d1-204">La vista Razor toma una lista de `TodoItem` y los muestra.</span><span class="sxs-lookup"><span data-stu-id="b55d1-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="b55d1-205">Si el componente de vista `InvokeAsync` método no pasa el nombre de la vista (como en nuestro ejemplo), *predeterminado* por convención, se utiliza para el nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="b55d1-206">Más adelante en el tutorial, mostraré cómo pasar el nombre de la vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="b55d1-207">Para reemplazar el estilo predeterminado de un controlador concreto, agregar una vista a la carpeta de la vista específica del controlador (por ejemplo *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="b55d1-208">Si el componente de vista es específicas del controlador, puede agregarlo a la carpeta específica del controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b55d1-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="b55d1-209">Agregar un `div` que contiene una llamada al componente de lista de prioridad para la parte inferior de la *Views/Todo/index.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="b55d1-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="b55d1-210">El marcado `@await Component.InvokeAsync` muestra la sintaxis para llamar a los componentes de la vista.</span><span class="sxs-lookup"><span data-stu-id="b55d1-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="b55d1-211">El primer argumento es el nombre del componente que desea invocar o llamar a.</span><span class="sxs-lookup"><span data-stu-id="b55d1-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="b55d1-212">Los parámetros siguientes se pasan al componente.</span><span class="sxs-lookup"><span data-stu-id="b55d1-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="b55d1-213">`InvokeAsync`puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="b55d1-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="b55d1-214">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b55d1-214">Test the app.</span></span> <span data-ttu-id="b55d1-215">La siguiente imagen muestra la lista de tareas y los elementos de prioridad:</span><span class="sxs-lookup"><span data-stu-id="b55d1-215">The following image shows the ToDo list and the priority items:</span></span>

![elementos de lista y la prioridad de tareas](view-components/_static/pi.png)

<span data-ttu-id="b55d1-217">También puede llamar al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="b55d1-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementos de prioridad de acción IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="b55d1-219">Especificar un nombre de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-219">Specifying a view name</span></span>

<span data-ttu-id="b55d1-220">Un componente de vista complejas que necesite especificar una vista no predeterminado en determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="b55d1-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="b55d1-221">El código siguiente muestra cómo especificar la vista de "Conexión virtual permanente" desde el `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="b55d1-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="b55d1-222">Actualización de la `InvokeAsync` método en la `PriorityListViewComponent` clase.</span><span class="sxs-lookup"><span data-stu-id="b55d1-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="b55d1-223">Copia la *Views/Shared/Components/PriorityList/Default.cshtml* archivo a una vista denominada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="b55d1-224">Agregar un encabezado para indicar que se está usando la vista de circuito virtual permanente.</span><span class="sxs-lookup"><span data-stu-id="b55d1-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="b55d1-225">Actualización *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b55d1-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="b55d1-226">Ejecute la aplicación y comprobar la vista de circuito virtual permanente.</span><span class="sxs-lookup"><span data-stu-id="b55d1-226">Run the app and verify PVC view.</span></span>

![Componente de vista de prioridad](view-components/_static/pvc.png)

<span data-ttu-id="b55d1-228">Si esta conexión virtual permanente no represente, compruebe que está llamando el componente de vista con una prioridad de 4 o posterior.</span><span class="sxs-lookup"><span data-stu-id="b55d1-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="b55d1-229">Examine la ruta de acceso de vista</span><span class="sxs-lookup"><span data-stu-id="b55d1-229">Examine the view path</span></span>

* <span data-ttu-id="b55d1-230">Cambie el parámetro de prioridad a tres o menos, por lo que no se devuelve la vista priority.</span><span class="sxs-lookup"><span data-stu-id="b55d1-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="b55d1-231">Cambiar temporalmente el nombre del *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="b55d1-232">Probar la aplicación, obtendrá el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="b55d1-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="b55d1-233">Copia *Views/Todo/Components/PriorityList/1Default.cshtml* a *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b55d1-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="b55d1-234">Agregar algún marcado para la *Shared* proviene de la vista de componentes de vista de lista de tareas para indicar la vista la *Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="b55d1-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="b55d1-235">Prueba la **Shared** vista de componentes.</span><span class="sxs-lookup"><span data-stu-id="b55d1-235">Test the **Shared** component view.</span></span>

![Salida de la lista de tareas con vista de componentes compartidos](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="b55d1-237">Evitar cadenas mágicas</span><span class="sxs-lookup"><span data-stu-id="b55d1-237">Avoiding magic strings</span></span>

<span data-ttu-id="b55d1-238">Si desea que la seguridad de tiempo de compilación, puede reemplazar el nombre del componente vista codificado de forma rígida con el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="b55d1-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="b55d1-239">Crear el componente de vista sin el sufijo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="b55d1-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="b55d1-240">Agregar un `using` instrucción para su Razor Ver archivo y usar el `nameof` operador:</span><span class="sxs-lookup"><span data-stu-id="b55d1-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="b55d1-241">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b55d1-241">Additional Resources</span></span>

* [<span data-ttu-id="b55d1-242">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="b55d1-242">Dependency injection into views</span></span>](dependency-injection.md)
