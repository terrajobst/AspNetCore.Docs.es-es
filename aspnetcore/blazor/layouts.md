---
title: ASP.NET Core diseños increíbles
author: guardrex
description: Aprenda a crear componentes de diseño reutilizables para aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/layouts
ms.openlocfilehash: 064ffafb8e7f3e76e5bd7bde0e06ef271fe92542
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211588"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="57298-103">ASP.NET Core diseños increíbles</span><span class="sxs-lookup"><span data-stu-id="57298-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="57298-104">Por [Rainer Stropek](https://www.timecockpit.com) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="57298-104">By [Rainer Stropek](https://www.timecockpit.com) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="57298-105">Algunos elementos de la aplicación, como los menús, los mensajes de copyright y los logotipos de la empresa, normalmente forman parte del diseño general de la aplicación y se usan en todos los componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57298-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="57298-106">Copiar el código de estos elementos en todos los componentes de una aplicación no es un enfoque&mdash;eficaz cada vez que uno de los elementos requiere una actualización, todos los componentes deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="57298-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="57298-107">Esta duplicación es difícil de mantener y puede dar lugar a contenido incoherente a lo largo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="57298-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="57298-108">Los *diseños* solucionan este problema.</span><span class="sxs-lookup"><span data-stu-id="57298-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="57298-109">Técnicamente, un diseño es simplemente otro componente.</span><span class="sxs-lookup"><span data-stu-id="57298-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="57298-110">Un diseño se define en una plantilla de Razor o C# en el código y puede usar el [enlace de datos](xref:blazor/components#data-binding), la [inserción de dependencias](xref:blazor/dependency-injection)y otros escenarios de componentes.</span><span class="sxs-lookup"><span data-stu-id="57298-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="57298-111">Para convertir un *componente* en un *diseño*, el componente:</span><span class="sxs-lookup"><span data-stu-id="57298-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="57298-112">Hereda de `LayoutComponentBase`, que define una `Body` propiedad para el contenido representado dentro del diseño.</span><span class="sxs-lookup"><span data-stu-id="57298-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="57298-113">Utiliza el sintaxis Razor `@Body` para especificar la ubicación en el marcado de diseño donde se representa el contenido.</span><span class="sxs-lookup"><span data-stu-id="57298-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="57298-114">En el ejemplo de código siguiente se muestra la plantilla de Razor de un componente de diseño, *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="57298-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="57298-115">El diseño hereda `LayoutComponentBase` y `@Body` establece entre la barra de navegación y el pie de página:</span><span class="sxs-lookup"><span data-stu-id="57298-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

<span data-ttu-id="57298-116">En una aplicación basada en una de las plantillas de aplicación de extraordinarias `MainLayout` , el componente (*MainLayout. Razor*) está en la carpeta *compartida* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57298-116">In an app based on one of the Blazor app templates, the `MainLayout` component (*MainLayout.razor*) is in the app's *Shared* folder.</span></span>

## <a name="default-layout"></a><span data-ttu-id="57298-117">Diseño predeterminado</span><span class="sxs-lookup"><span data-stu-id="57298-117">Default layout</span></span>

<span data-ttu-id="57298-118">Especifique el diseño de aplicación predeterminado en `Router` el componente del archivo *app. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="57298-118">Specify the default app layout in the `Router` component in the app's *App.razor* file.</span></span> <span data-ttu-id="57298-119">El siguiente `Router` componente, que proporcionan las plantillas de increíble predeterminada, establece el diseño predeterminado para el `MainLayout` componente:</span><span class="sxs-lookup"><span data-stu-id="57298-119">The following `Router` component, which is provided by the default Blazor templates, sets the default layout to the `MainLayout` component:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

<span data-ttu-id="57298-120">Para proporcionar un diseño predeterminado para `NotFound` el contenido, especifique `LayoutView` un `NotFound` para contenido:</span><span class="sxs-lookup"><span data-stu-id="57298-120">To supply a default layout for `NotFound` content, specify a `LayoutView` for `NotFound` content:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

<span data-ttu-id="57298-121">Para obtener más información sobre `Router` el componente, <xref:blazor/routing>vea.</span><span class="sxs-lookup"><span data-stu-id="57298-121">For more information on the `Router` component, see <xref:blazor/routing>.</span></span>

<span data-ttu-id="57298-122">Especificar el diseño como un diseño predeterminado en el enrutador es una práctica útil porque se puede reemplazar por componente o por carpeta.</span><span class="sxs-lookup"><span data-stu-id="57298-122">Specifying the layout as a default layout in the router is a useful practice because it can be overridden on a per-component or per-folder basis.</span></span> <span data-ttu-id="57298-123">Prefiere usar el enrutador para establecer el diseño predeterminado de la aplicación, ya que es la técnica más general.</span><span class="sxs-lookup"><span data-stu-id="57298-123">Prefer using the router to set the app's default layout because it's the most general technique.</span></span>

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="57298-124">Especificar un diseño en un componente</span><span class="sxs-lookup"><span data-stu-id="57298-124">Specify a layout in a component</span></span>

<span data-ttu-id="57298-125">Use la directiva `@layout` Razor para aplicar un diseño a un componente.</span><span class="sxs-lookup"><span data-stu-id="57298-125">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="57298-126">El compilador `@layout` convierte `LayoutAttribute`en, que se aplica a la clase de componente.</span><span class="sxs-lookup"><span data-stu-id="57298-126">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="57298-127">El contenido del componente siguiente `MasterList` se inserta en la `MasterLayout` posición de `@Body`:</span><span class="sxs-lookup"><span data-stu-id="57298-127">The content of the following `MasterList` component is inserted into the `MasterLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

<span data-ttu-id="57298-128">Al especificar el diseño directamente en un componente, se invalida un *diseño predeterminado* establecido en el enrutador `@layout` o una directiva importada desde *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="57298-128">Specifying the layout directly in a component overrides a *default layout* set in the router or an `@layout` directive imported from *_Imports.razor*.</span></span>

## <a name="centralized-layout-selection"></a><span data-ttu-id="57298-129">Selección de diseño centralizado</span><span class="sxs-lookup"><span data-stu-id="57298-129">Centralized layout selection</span></span>

<span data-ttu-id="57298-130">Cada carpeta de una aplicación puede contener opcionalmente un archivo de plantilla denominado *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="57298-130">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="57298-131">El compilador incluye las directivas especificadas en el archivo de importaciones en todas las plantillas de Razor de la misma carpeta y de forma recursiva en todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="57298-131">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="57298-132">Por lo tanto, un archivo *_Imports. Razor* que contenga `@layout MyCoolLayout` garantiza que todos los componentes de `MyCoolLayout`una carpeta usan.</span><span class="sxs-lookup"><span data-stu-id="57298-132">Therefore, an *_Imports.razor* file containing `@layout MyCoolLayout` ensures that all of the components in a folder use `MyCoolLayout`.</span></span> <span data-ttu-id="57298-133">No es necesario agregar `@layout MyCoolLayout` varias veces a todos los archivos *. Razor* dentro de la carpeta y las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="57298-133">There's no need to repeatedly add `@layout MyCoolLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="57298-134">`@using`las directivas también se aplican a los componentes de la misma manera.</span><span class="sxs-lookup"><span data-stu-id="57298-134">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="57298-135">El siguiente archivo *_Imports. Razor* importa:</span><span class="sxs-lookup"><span data-stu-id="57298-135">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="57298-136">`MyCoolLayout`.</span><span class="sxs-lookup"><span data-stu-id="57298-136">`MyCoolLayout`.</span></span>
* <span data-ttu-id="57298-137">Todos los componentes de Razor en la misma carpeta y en todas las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="57298-137">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="57298-138">El espacio de nombres `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="57298-138">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="57298-139">El archivo *_Imports. Razor* es similar al [archivo _ViewImports. cshtml para las vistas y páginas de Razor,](xref:mvc/views/layout#importing-shared-directives) pero se aplica específicamente a los archivos de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="57298-139">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="57298-140">Al especificar un diseño en *_Imports. Razor* , se invalida un diseño especificado como el *diseño predeterminado*del enrutador.</span><span class="sxs-lookup"><span data-stu-id="57298-140">Specifying a layout in *_Imports.razor* overrides a layout specified as the router's *default layout*.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="57298-141">Diseños anidados</span><span class="sxs-lookup"><span data-stu-id="57298-141">Nested layouts</span></span>

<span data-ttu-id="57298-142">Las aplicaciones pueden estar formadas por diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="57298-142">Apps can consist of nested layouts.</span></span> <span data-ttu-id="57298-143">Un componente puede hacer referencia a un diseño que a su vez hace referencia a otro diseño.</span><span class="sxs-lookup"><span data-stu-id="57298-143">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="57298-144">Por ejemplo, los diseños anidados se usan para crear una estructura de menú de varios niveles.</span><span class="sxs-lookup"><span data-stu-id="57298-144">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="57298-145">En el ejemplo siguiente se muestra cómo usar diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="57298-145">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="57298-146">El archivo *EpisodesComponent. Razor* es el componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="57298-146">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="57298-147">El componente hace referencia `MasterListLayout`a:</span><span class="sxs-lookup"><span data-stu-id="57298-147">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="57298-148">El archivo *MasterListLayout. Razor* proporciona el `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="57298-148">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="57298-149">El diseño hace referencia a otro `MasterLayout`diseño,, donde se representa.</span><span class="sxs-lookup"><span data-stu-id="57298-149">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="57298-150">`EpisodesComponent`se representa donde `@Body` aparece:</span><span class="sxs-lookup"><span data-stu-id="57298-150">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="57298-151">Por último `MasterLayout` , en *MasterLayout. Razor* contiene los elementos de diseño de nivel superior, como el encabezado, el menú principal y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="57298-151">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="57298-152">`MasterListLayout``@Body` con, `EpisodesComponent` se representa donde aparece:</span><span class="sxs-lookup"><span data-stu-id="57298-152">`MasterListLayout` with the `EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="57298-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="57298-153">Additional resources</span></span>

* <xref:mvc/views/layout>
