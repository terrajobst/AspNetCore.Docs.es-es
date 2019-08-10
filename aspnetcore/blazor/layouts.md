---
title: ASP.NET Core diseños increíbles
author: guardrex
description: Aprenda a crear componentes de diseño reutilizables para aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/layouts
ms.openlocfilehash: 2d652e149381f0a93e3135da978ab5737d47c6f1
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "68948225"
---
# <a name="aspnet-core-blazor-layouts"></a><span data-ttu-id="7ffd0-103">ASP.NET Core diseños increíbles</span><span class="sxs-lookup"><span data-stu-id="7ffd0-103">ASP.NET Core Blazor layouts</span></span>

<span data-ttu-id="7ffd0-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="7ffd0-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="7ffd0-105">Algunos elementos de la aplicación, como los menús, los mensajes de copyright y los logotipos de la empresa, normalmente forman parte del diseño general de la aplicación y se usan en todos los componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-105">Some app elements, such as menus, copyright messages, and company logos, are usually part of app's overall layout and used by every component in the app.</span></span> <span data-ttu-id="7ffd0-106">Copiar el código de estos elementos en todos los componentes de una aplicación no es un enfoque&mdash;eficaz cada vez que uno de los elementos requiere una actualización, todos los componentes deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-106">Copying the code of these elements into all of the components of an app isn't an efficient approach&mdash;every time one of the elements requires an update, every component must be updated.</span></span> <span data-ttu-id="7ffd0-107">Esta duplicación es difícil de mantener y puede dar lugar a contenido incoherente a lo largo del tiempo.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-107">Such duplication is difficult to maintain and can lead to inconsistent content over time.</span></span> <span data-ttu-id="7ffd0-108">Los *diseños* solucionan este problema.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-108">*Layouts* solve this problem.</span></span>

<span data-ttu-id="7ffd0-109">Técnicamente, un diseño es simplemente otro componente.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-109">Technically, a layout is just another component.</span></span> <span data-ttu-id="7ffd0-110">Un diseño se define en una plantilla de Razor o C# en el código y puede usar el [enlace de datos](xref:blazor/components#data-binding), la [inserción](xref:blazor/dependency-injection)de dependencias y otros escenarios de componentes.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-110">A layout is defined in a Razor template or in C# code and can use [data binding](xref:blazor/components#data-binding), [dependency injection](xref:blazor/dependency-injection), and other component scenarios.</span></span>

<span data-ttu-id="7ffd0-111">Para convertir un *componente* en un *diseño*, el componente:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-111">To turn a *component* into a *layout*, the component:</span></span>

* <span data-ttu-id="7ffd0-112">Hereda de `LayoutComponentBase`, que define una `Body` propiedad para el contenido representado dentro del diseño.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-112">Inherits from `LayoutComponentBase`, which defines a `Body` property for the rendered content inside the layout.</span></span>
* <span data-ttu-id="7ffd0-113">Utiliza el sintaxis Razor `@Body` para especificar la ubicación en el marcado de diseño donde se representa el contenido.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-113">Uses the Razor syntax `@Body` to specify the location in the layout markup where the content is rendered.</span></span>

<span data-ttu-id="7ffd0-114">En el ejemplo de código siguiente se muestra la plantilla de Razor de un componente de diseño, *MainLayout. Razor*.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-114">The following code sample shows the Razor template of a layout component, *MainLayout.razor*.</span></span> <span data-ttu-id="7ffd0-115">El diseño hereda `LayoutComponentBase` y `@Body` establece entre la barra de navegación y el pie de página:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-115">The layout inherits `LayoutComponentBase` and sets the `@Body` between the navigation bar and the footer:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

## <a name="specify-a-layout-in-a-component"></a><span data-ttu-id="7ffd0-116">Especificar un diseño en un componente</span><span class="sxs-lookup"><span data-stu-id="7ffd0-116">Specify a layout in a component</span></span>

<span data-ttu-id="7ffd0-117">Use la directiva `@layout` Razor para aplicar un diseño a un componente.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-117">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="7ffd0-118">El compilador `@layout` convierte `LayoutAttribute`en, que se aplica a la clase de componente.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-118">The compiler converts `@layout` into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="7ffd0-119">El contenido del componente siguiente, *MasterList. Razor*, se inserta en la `MainLayout` posición de: `@Body`</span><span class="sxs-lookup"><span data-stu-id="7ffd0-119">The content of the following component, *MasterList.razor*, is inserted into the `MainLayout` at the position of `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

## <a name="centralized-layout-selection"></a><span data-ttu-id="7ffd0-120">Selección de diseño centralizado</span><span class="sxs-lookup"><span data-stu-id="7ffd0-120">Centralized layout selection</span></span>

<span data-ttu-id="7ffd0-121">Cada carpeta de una aplicación puede contener opcionalmente un archivo de plantilla denominado *_Imports. Razor*.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-121">Every folder of an app can optionally contain a template file named *_Imports.razor*.</span></span> <span data-ttu-id="7ffd0-122">El compilador incluye las directivas especificadas en el archivo de importaciones en todas las plantillas de Razor de la misma carpeta y de forma recursiva en todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-122">The compiler includes the directives specified in the imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="7ffd0-123">Por lo tanto, un archivo *_Imports. Razor* que contenga `@layout MainLayout` garantiza que todos los componentes de `MainLayout`una carpeta usan.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-123">Therefore, an *_Imports.razor* file containing `@layout MainLayout` ensures that all of the components in a folder use `MainLayout`.</span></span> <span data-ttu-id="7ffd0-124">No es necesario agregar `@layout MainLayout` varias veces a todos los archivos *. Razor* dentro de la carpeta y las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-124">There's no need to repeatedly add `@layout MainLayout` to all of the *.razor* files within the folder and subfolders.</span></span> <span data-ttu-id="7ffd0-125">`@using`las directivas también se aplican a los componentes de la misma manera.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-125">`@using` directives are also applied to components in the same way.</span></span>

<span data-ttu-id="7ffd0-126">El siguiente archivo *_Imports. Razor* importa:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-126">The following *_Imports.razor* file imports:</span></span>

* <span data-ttu-id="7ffd0-127">`MainLayout`</span><span class="sxs-lookup"><span data-stu-id="7ffd0-127">`MainLayout`.</span></span>
* <span data-ttu-id="7ffd0-128">Todos los componentes de Razor en la misma carpeta y en todas las subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-128">All Razor components in the same folder and any subfolders.</span></span>
* <span data-ttu-id="7ffd0-129">El espacio de nombres `BlazorApp1.Data` .</span><span class="sxs-lookup"><span data-stu-id="7ffd0-129">The `BlazorApp1.Data` namespace.</span></span>
 
[!code-cshtml[](layouts/sample_snapshot/3.x/_Imports.razor)]

<span data-ttu-id="7ffd0-130">El archivo *_Imports. Razor* es similar al [archivo _ViewImports. cshtml para las vistas y páginas de Razor,](xref:mvc/views/layout#importing-shared-directives) pero se aplica específicamente a los archivos de componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-130">The *_Imports.razor* file is similar to the [_ViewImports.cshtml file for Razor views and pages](xref:mvc/views/layout#importing-shared-directives) but applied specifically to Razor component files.</span></span>

<span data-ttu-id="7ffd0-131">Las plantillas extraordinarias usan archivos *_Imports. Razor* para la selección del diseño.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-131">The Blazor templates use *_Imports.razor* files for layout selection.</span></span> <span data-ttu-id="7ffd0-132">Una aplicación creada a partir de una plantilla extraordinaria contiene el archivo *_Imports. Razor* en la raíz del proyecto y en la carpeta *páginas* .</span><span class="sxs-lookup"><span data-stu-id="7ffd0-132">An app created from a Blazor template contains the *_Imports.razor* file in the root of the project and in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="7ffd0-133">Diseños anidados</span><span class="sxs-lookup"><span data-stu-id="7ffd0-133">Nested layouts</span></span>

<span data-ttu-id="7ffd0-134">Las aplicaciones pueden estar formadas por diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-134">Apps can consist of nested layouts.</span></span> <span data-ttu-id="7ffd0-135">Un componente puede hacer referencia a un diseño que a su vez hace referencia a otro diseño.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-135">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="7ffd0-136">Por ejemplo, los diseños anidados se usan para crear una estructura de menú de varios niveles.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-136">For example, nesting layouts are used to create a multi-level menu structure.</span></span>

<span data-ttu-id="7ffd0-137">En el ejemplo siguiente se muestra cómo usar diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-137">The following example shows how to use nested layouts.</span></span> <span data-ttu-id="7ffd0-138">El archivo *EpisodesComponent. Razor* es el componente que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-138">The *EpisodesComponent.razor* file is the component to display.</span></span> <span data-ttu-id="7ffd0-139">El componente hace referencia `MasterListLayout`a:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-139">The component references the `MasterListLayout`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

<span data-ttu-id="7ffd0-140">El archivo *MasterListLayout. Razor* proporciona el `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-140">The *MasterListLayout.razor* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="7ffd0-141">El diseño hace referencia a otro `MasterLayout`diseño,, donde se representa.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-141">The layout references another layout, `MasterLayout`, where it's rendered.</span></span> <span data-ttu-id="7ffd0-142">`EpisodesComponent`se representa donde `@Body` aparece:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-142">`EpisodesComponent` is rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

<span data-ttu-id="7ffd0-143">Por último `MasterLayout` , en *MasterLayout. Razor* contiene los elementos de diseño de nivel superior, como el encabezado, el menú principal y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="7ffd0-143">Finally, `MasterLayout` in *MasterLayout.razor* contains the top-level layout elements, such as the header, main menu, and footer.</span></span> <span data-ttu-id="7ffd0-144">`MasterListLayout`con `EpisodesComponent` se representan donde `@Body` aparece:</span><span class="sxs-lookup"><span data-stu-id="7ffd0-144">`MasterListLayout` with `EpisodesComponent` are rendered where `@Body` appears:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="additional-resources"></a><span data-ttu-id="7ffd0-145">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7ffd0-145">Additional resources</span></span>

* <xref:mvc/views/layout>
