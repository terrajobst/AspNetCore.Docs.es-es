---
title: Componentes de la vista
author: rick-anderson
description: "Componentes de la vista pretenden en cualquier lugar que tiene lógica de representación reutilizable."
keywords: "Núcleo de ASP.NET, componentes de la vista, vista parcial"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: bb8a889c66ec9ca0c0aec7b4a4184d7c19858d78
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="view-components"></a><span data-ttu-id="f30c3-104">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-104">View components</span></span>

<span data-ttu-id="f30c3-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f30c3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="f30c3-106">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f30c3-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)

## <a name="introducing-view-components"></a><span data-ttu-id="f30c3-107">Componentes de la vista de presentación</span><span class="sxs-lookup"><span data-stu-id="f30c3-107">Introducing view components</span></span>

<span data-ttu-id="f30c3-108">Nuevo en MVC de ASP.NET Core, ver componentes son similares a las vistas parciales, pero son mucho más eficaces.</span><span class="sxs-lookup"><span data-stu-id="f30c3-108">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="f30c3-109">Componentes de la vista no usar enlace de modelos y sólo dependen de los datos proporcionados al llamar a en él.</span><span class="sxs-lookup"><span data-stu-id="f30c3-109">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="f30c3-110">Un componente de vista:</span><span class="sxs-lookup"><span data-stu-id="f30c3-110">A view component:</span></span>

* <span data-ttu-id="f30c3-111">Representa un fragmento en lugar de una respuesta toda</span><span class="sxs-lookup"><span data-stu-id="f30c3-111">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="f30c3-112">Incluye la misma separación de problemas y los beneficios de capacidad de prueba se encuentra entre un controlador y la vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-112">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="f30c3-113">Puede tener parámetros y lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="f30c3-113">Can have parameters and business logic</span></span>
* <span data-ttu-id="f30c3-114">Normalmente se invoca desde una página de diseño</span><span class="sxs-lookup"><span data-stu-id="f30c3-114">Is typically invoked from a layout page</span></span>

<span data-ttu-id="f30c3-115">Componentes de la vista se han diseñado en cualquier lugar que tiene lógica de representación reutilizable que es demasiado compleja para una vista parcial, como:</span><span class="sxs-lookup"><span data-stu-id="f30c3-115">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="f30c3-116">Menús de navegación dinámica</span><span class="sxs-lookup"><span data-stu-id="f30c3-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="f30c3-117">Nube de etiquetas (que consulta la base de datos)</span><span class="sxs-lookup"><span data-stu-id="f30c3-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="f30c3-118">Panel de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="f30c3-118">Login panel</span></span>
* <span data-ttu-id="f30c3-119">Carro de la compra</span><span class="sxs-lookup"><span data-stu-id="f30c3-119">Shopping cart</span></span>
* <span data-ttu-id="f30c3-120">Artículos publicados recientemente</span><span class="sxs-lookup"><span data-stu-id="f30c3-120">Recently published articles</span></span>
* <span data-ttu-id="f30c3-121">Contenido de Sidebar en un blog típico</span><span class="sxs-lookup"><span data-stu-id="f30c3-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="f30c3-122">Un panel de inicio de sesión que se representan en cada página y mostrar los vínculos entre ambos para cerrar la sesión o de registro, según el registro de estado del usuario</span><span class="sxs-lookup"><span data-stu-id="f30c3-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="f30c3-123">Un componente de vista consta de dos partes: la clase (normalmente derivado de [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) y lo devuelve (normalmente una vista).</span><span class="sxs-lookup"><span data-stu-id="f30c3-123">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="f30c3-124">Al igual que los controladores, un componente de vista puede ser un POCO, pero la mayoría de los desarrolladores desee aprovechar las ventajas de los métodos y propiedades disponibles derivando de `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="f30c3-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="f30c3-125">Crear un componente de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-125">Creating a view component</span></span>

<span data-ttu-id="f30c3-126">Esta sección contiene los requisitos de alto nivel para crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="f30c3-127">Más adelante en el artículo, se deberá examinar cada paso en detalle y crear un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="f30c3-128">La clase de componente de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-128">The view component class</span></span>

<span data-ttu-id="f30c3-129">Una clase de componente de vista se puede crear por cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="f30c3-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="f30c3-130">Derivar de *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="f30c3-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="f30c3-131">Al decorar una clase con el `[ViewComponent]` atributo o derivar de una clase con el `[ViewComponent]` atributo</span><span class="sxs-lookup"><span data-stu-id="f30c3-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="f30c3-132">Crear una clase donde el nombre termina con el sufijo *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="f30c3-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="f30c3-133">Como controladores, componentes de la vista deben ser clases públicas, no anidada y no abstracta.</span><span class="sxs-lookup"><span data-stu-id="f30c3-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="f30c3-134">El nombre del componente de vista es el nombre de clase con el sufijo "ViewComponent" quitado.</span><span class="sxs-lookup"><span data-stu-id="f30c3-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="f30c3-135">También se puede especificar explícitamente mediante el `ViewComponentAttribute.Name` propiedad.</span><span class="sxs-lookup"><span data-stu-id="f30c3-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="f30c3-136">Una clase de componente de vista:</span><span class="sxs-lookup"><span data-stu-id="f30c3-136">A view component class:</span></span>

* <span data-ttu-id="f30c3-137">Es totalmente compatible con constructor [inyección de dependencia](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="f30c3-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="f30c3-138">No participar en el ciclo de vida de controlador, lo que significa que no se puede utilizar [filtros](../controllers/filters.md) en un componente de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-138">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="f30c3-139">Ver los métodos de componente</span><span class="sxs-lookup"><span data-stu-id="f30c3-139">View component methods</span></span>

<span data-ttu-id="f30c3-140">Un componente de vista define su lógica en una `InvokeAsync` método que devuelve un `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="f30c3-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="f30c3-141">Parámetros proceden directamente de invocación del componente de vista, no desde el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="f30c3-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="f30c3-142">Un componente de vista nunca directamente controla una solicitud.</span><span class="sxs-lookup"><span data-stu-id="f30c3-142">A view component never directly handles a request.</span></span> <span data-ttu-id="f30c3-143">Por lo general, inicializa un modelo de un componente de vista y la pasa a una vista mediante una llamada a la `View` método.</span><span class="sxs-lookup"><span data-stu-id="f30c3-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="f30c3-144">En resumen, ver los métodos de componente:</span><span class="sxs-lookup"><span data-stu-id="f30c3-144">In summary, view component methods:</span></span>

* <span data-ttu-id="f30c3-145">Definir una `InvokeAsync` método que devuelva una`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="f30c3-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="f30c3-146">Inicializa un modelo normalmente y la pasa a una vista mediante una llamada a la `ViewComponent` `View` (método)</span><span class="sxs-lookup"><span data-stu-id="f30c3-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="f30c3-147">Parámetros provienen del método que realiza la llamada, y no HTTP, no hay ningún enlace de modelo</span><span class="sxs-lookup"><span data-stu-id="f30c3-147">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="f30c3-148">Son accesibles directamente como un extremo HTTP, que se invoquen desde el código (normalmente en una vista).</span><span class="sxs-lookup"><span data-stu-id="f30c3-148">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="f30c3-149">Un componente de vista nunca controla una solicitud</span><span class="sxs-lookup"><span data-stu-id="f30c3-149">A view component never handles a request</span></span>
* <span data-ttu-id="f30c3-150">Están sobrecargados en la firma en lugar de los detalles de la solicitud HTTP actual</span><span class="sxs-lookup"><span data-stu-id="f30c3-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="f30c3-151">Ruta de acceso de búsqueda de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-151">View search path</span></span>

<span data-ttu-id="f30c3-152">El tiempo de ejecución busca la vista en las rutas de acceso siguientes:</span><span class="sxs-lookup"><span data-stu-id="f30c3-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="f30c3-153">Vistas /\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="f30c3-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="f30c3-154">Vistas / / componentes de uso compartido/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="f30c3-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="f30c3-155">El nombre de vista predeterminado para un componente de vista es *predeterminado*, lo que significa que el archivo de vista se suele denominar *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="f30c3-156">Puede especificar un nombre de vista diferente al crear el resultado del componente de vista o cuando se llama a la `View` método.</span><span class="sxs-lookup"><span data-stu-id="f30c3-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="f30c3-157">Se recomienda que un nombre al archivo de vista *Default.cshtml* y use la *vistas / / componentes de uso compartido/\<view_component_name > /\<view_name >* ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="f30c3-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="f30c3-158">El `PriorityList` componente de vista utilizado en este ejemplo utiliza *Views/Shared/Components/PriorityList/Default.cshtml* para la vista de componentes de la vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="f30c3-159">Invocar un componente de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-159">Invoking a view component</span></span>

<span data-ttu-id="f30c3-160">Para utilizar el componente de vista, llame a los siguientes dentro de una vista:</span><span class="sxs-lookup"><span data-stu-id="f30c3-160">To use the view component, call the following inside a view:</span></span>

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="f30c3-161">Los parámetros se pasará a la `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="f30c3-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="f30c3-162">El `PriorityList` desarrollado en el artículo del componente de vista se invoca desde la *Views/Todo/Index.cshtml* ver el archivo.</span><span class="sxs-lookup"><span data-stu-id="f30c3-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="f30c3-163">En la siguiente tabla, el `InvokeAsync` se denomina método con dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="f30c3-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="f30c3-164">Invocar un componente de vista como una aplicación auxiliar de etiqueta</span><span class="sxs-lookup"><span data-stu-id="f30c3-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="f30c3-165">Para ASP.NET Core 1.1 y versiones posteriores, puede invocar un componente de vista como una [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="f30c3-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="f30c3-166">Parámetros de clase y método la grafía Pascal para aplicaciones auxiliares de etiquetas se convierten en sus [reducir caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="f30c3-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="f30c3-167">La aplicación auxiliar de etiquetas para invocar un componente de vista utiliza la `<vc></vc>` elemento.</span><span class="sxs-lookup"><span data-stu-id="f30c3-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="f30c3-168">El componente de vista se especifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="f30c3-168">The view component is specified as follows:</span></span>

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="f30c3-169">Nota: Para poder usar un componente de vista como una aplicación auxiliar de etiquetas, debe registrar el ensamblado que contiene el componente de vista mediante el `@addTagHelper` directiva.</span><span class="sxs-lookup"><span data-stu-id="f30c3-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="f30c3-170">Por ejemplo, si el componente de la vista está en un ensamblado denominado "MyWebApp", agregue la siguiente directiva a la `_ViewImports.cshtml` archivo:</span><span class="sxs-lookup"><span data-stu-id="f30c3-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```csharp
@addTagHelper *, MyWebApp
```

<span data-ttu-id="f30c3-171">Puede registrar un componente de vista como una aplicación auxiliar de etiqueta a cualquier archivo que hace referencia al componente de vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="f30c3-172">Vea [administración de ámbito de aplicación auxiliar de la etiqueta](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obtener más información sobre cómo registrar aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="f30c3-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="f30c3-173">El `InvokeAsync` método utilizado en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="f30c3-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="f30c3-174">En el marcado de la aplicación auxiliar de etiqueta:</span><span class="sxs-lookup"><span data-stu-id="f30c3-174">In Tag Helper markup:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="f30c3-175">En el ejemplo anterior, el `PriorityList` del componente vista se convierte en `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="f30c3-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="f30c3-176">Los parámetros para el componente de vista se pasan como atributos en minúsculas kebab.</span><span class="sxs-lookup"><span data-stu-id="f30c3-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="f30c3-177">Invocar un componente de vista directamente desde un controlador</span><span class="sxs-lookup"><span data-stu-id="f30c3-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="f30c3-178">Ver componentes normalmente se invocan desde una vista, pero se puede invocar directamente desde un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="f30c3-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="f30c3-179">Mientras los componentes de la vista no definen como controladores, puede implementar fácilmente una acción de controlador que devuelve el contenido de un `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="f30c3-179">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="f30c3-180">En este ejemplo, el componente de vista se denomina directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="f30c3-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="f30c3-181">Tutorial: Crear un componente de vista simple</span><span class="sxs-lookup"><span data-stu-id="f30c3-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="f30c3-182">[Descargar](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compilar y probar el código de inicio.</span><span class="sxs-lookup"><span data-stu-id="f30c3-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="f30c3-183">Es un proyecto simple con un `Todo` controlador que muestra una lista de *tareas* elementos.</span><span class="sxs-lookup"><span data-stu-id="f30c3-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="f30c3-185">Agregue una clase ViewComponent</span><span class="sxs-lookup"><span data-stu-id="f30c3-185">Add a ViewComponent class</span></span>

<span data-ttu-id="f30c3-186">Crear un *ViewComponents* carpeta y agregue las siguientes `PriorityListViewComponent` clase:</span><span class="sxs-lookup"><span data-stu-id="f30c3-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="f30c3-187">Notas sobre el código:</span><span class="sxs-lookup"><span data-stu-id="f30c3-187">Notes on the code:</span></span>

* <span data-ttu-id="f30c3-188">Clases de componentes de vista pueden estar contenidas en **cualquier** carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f30c3-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="f30c3-189">Dado que el nombre de clase PriorityList**ViewComponent** termina con el sufijo **ViewComponent**, el tiempo de ejecución utilizará la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="f30c3-190">Explicaremos con más detalle más adelante.</span><span class="sxs-lookup"><span data-stu-id="f30c3-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="f30c3-191">El `[ViewComponent]` atributos pueden cambiar el nombre utilizado para hacer referencia a un componente de vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="f30c3-192">Por ejemplo, podríamos podríamos cambiar el nombre la clase `XYZ` y aplica la `ViewComponent` atributo:</span><span class="sxs-lookup"><span data-stu-id="f30c3-192">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="f30c3-193">El `[ViewComponent]` atributo anterior indica el selector de componente de vista para utilizar el nombre `PriorityList` al buscar las vistas asociadas con el componente y utilizar la cadena "PriorityList" cuando se hace referencia el componente de la clase de una vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="f30c3-194">Explicaremos con más detalle más adelante.</span><span class="sxs-lookup"><span data-stu-id="f30c3-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="f30c3-195">El componente usa [inyección de dependencia](../../fundamentals/dependency-injection.md) para que esté disponible el contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="f30c3-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="f30c3-196">`InvokeAsync`expone un método que se puede llamar desde una vista y puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="f30c3-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="f30c3-197">El `InvokeAsync` método devuelve el conjunto de `ToDo` los elementos que cumplen el `isDone` y `maxPriority` parámetros.</span><span class="sxs-lookup"><span data-stu-id="f30c3-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="f30c3-198">Crear la vista de Razor del componente de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-198">Create the view component Razor view</span></span>

* <span data-ttu-id="f30c3-199">Crear el *vistas/Shared/componentes* carpeta.</span><span class="sxs-lookup"><span data-stu-id="f30c3-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="f30c3-200">Esta carpeta **debe** denominarse *componentes*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="f30c3-201">Crear el *vistas/Shared/componentes/PriorityList* carpeta.</span><span class="sxs-lookup"><span data-stu-id="f30c3-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="f30c3-202">Este nombre de carpeta debe coincidir con el nombre de la clase de componente de vista o el nombre de la clase menos el sufijo (si se siguió convención y usa el *ViewComponent* sufijo en el nombre de clase).</span><span class="sxs-lookup"><span data-stu-id="f30c3-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="f30c3-203">Si ha usado la `ViewComponent` atributo, necesitaría el nombre de clase para que coincida con la designación de atributo.</span><span class="sxs-lookup"><span data-stu-id="f30c3-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="f30c3-204">Crear un *Views/Shared/Components/PriorityList/Default.cshtml* vista Razor:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="f30c3-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="f30c3-205">La vista Razor toma una lista de `TodoItem` y los muestra.</span><span class="sxs-lookup"><span data-stu-id="f30c3-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="f30c3-206">Si el componente de vista `InvokeAsync` método no pasa el nombre de la vista (como en nuestro ejemplo), *predeterminado* por convención, se utiliza para el nombre de vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="f30c3-207">Más adelante en el tutorial, mostraré cómo pasar el nombre de la vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="f30c3-208">Para reemplazar el estilo predeterminado de un controlador concreto, agregar una vista a la carpeta de la vista específica del controlador (por ejemplo *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="f30c3-209">Si el componente de vista es específicas del controlador, puede agregarlo a la carpeta específica del controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f30c3-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="f30c3-210">Agregar un `div` que contiene una llamada al componente de lista de prioridad para la parte inferior de la *Views/Todo/index.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="f30c3-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="f30c3-211">El marcado `@await Component.InvokeAsync` muestra la sintaxis para llamar a los componentes de la vista.</span><span class="sxs-lookup"><span data-stu-id="f30c3-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="f30c3-212">El primer argumento es el nombre del componente que desea invocar o llamar a.</span><span class="sxs-lookup"><span data-stu-id="f30c3-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="f30c3-213">Los parámetros siguientes se pasan al componente.</span><span class="sxs-lookup"><span data-stu-id="f30c3-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="f30c3-214">`InvokeAsync`puede tomar un número arbitrario de argumentos.</span><span class="sxs-lookup"><span data-stu-id="f30c3-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="f30c3-215">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f30c3-215">Test the app.</span></span> <span data-ttu-id="f30c3-216">La siguiente imagen muestra la lista de tareas y los elementos de prioridad:</span><span class="sxs-lookup"><span data-stu-id="f30c3-216">The following image shows the ToDo list and the priority items:</span></span>

![elementos de lista y la prioridad de tareas](view-components/_static/pi.png)

<span data-ttu-id="f30c3-218">También puede llamar al componente de vista directamente desde el controlador:</span><span class="sxs-lookup"><span data-stu-id="f30c3-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![elementos de prioridad de acción IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="f30c3-220">Especificar un nombre de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-220">Specifying a view name</span></span>

<span data-ttu-id="f30c3-221">Un componente de vista complejas que necesite especificar una vista no predeterminado en determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="f30c3-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="f30c3-222">El código siguiente muestra cómo especificar la vista de "Conexión virtual permanente" desde el `InvokeAsync` método.</span><span class="sxs-lookup"><span data-stu-id="f30c3-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="f30c3-223">Actualización de la `InvokeAsync` método en la `PriorityListViewComponent` clase.</span><span class="sxs-lookup"><span data-stu-id="f30c3-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="f30c3-224">Copia la *Views/Shared/Components/PriorityList/Default.cshtml* archivo a una vista denominada *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="f30c3-225">Agregar un encabezado para indicar que se está usando la vista de circuito virtual permanente.</span><span class="sxs-lookup"><span data-stu-id="f30c3-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="f30c3-226">Actualización *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f30c3-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="f30c3-227">Ejecute la aplicación y comprobar la vista de circuito virtual permanente.</span><span class="sxs-lookup"><span data-stu-id="f30c3-227">Run the app and verify PVC view.</span></span>

![Componente de vista de prioridad](view-components/_static/pvc.png)

<span data-ttu-id="f30c3-229">Si no se representa la vista de circuito virtual permanente, compruebe que está llamando el componente de vista con una prioridad de 4 o posterior.</span><span class="sxs-lookup"><span data-stu-id="f30c3-229">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="f30c3-230">Examine la ruta de acceso de vista</span><span class="sxs-lookup"><span data-stu-id="f30c3-230">Examine the view path</span></span>

* <span data-ttu-id="f30c3-231">Cambie el parámetro de prioridad a tres o menos, por lo que no se devuelve la vista priority.</span><span class="sxs-lookup"><span data-stu-id="f30c3-231">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="f30c3-232">Cambiar temporalmente el nombre del *Views/Todo/Components/PriorityList/Default.cshtml* a *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="f30c3-233">Probar la aplicación, obtendrá el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="f30c3-233">Test the app, you'll get the following error:</span></span>

   <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="f30c3-234">Copia *Views/Todo/Components/PriorityList/1Default.cshtml* a *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f30c3-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="f30c3-235">Agregar algún marcado para la *Shared* proviene de la vista de componentes de vista de lista de tareas para indicar la vista la *Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="f30c3-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="f30c3-236">Prueba la **Shared** vista de componentes.</span><span class="sxs-lookup"><span data-stu-id="f30c3-236">Test the **Shared** component view.</span></span>

![Salida de la lista de tareas con vista de componentes compartidos](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="f30c3-238">Evitar cadenas mágicas</span><span class="sxs-lookup"><span data-stu-id="f30c3-238">Avoiding magic strings</span></span>

<span data-ttu-id="f30c3-239">Si desea que la seguridad de tiempo de compilación, puede reemplazar el nombre del componente vista codificado de forma rígida con el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="f30c3-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="f30c3-240">Crear el componente de vista sin el sufijo "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="f30c3-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="f30c3-241">Agregar un `using` instrucción para su Razor Ver archivo y usar el `nameof` operador:</span><span class="sxs-lookup"><span data-stu-id="f30c3-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="f30c3-242">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f30c3-242">Additional Resources</span></span>

* [<span data-ttu-id="f30c3-243">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="f30c3-243">Dependency injection into views</span></span>](dependency-injection.md)
