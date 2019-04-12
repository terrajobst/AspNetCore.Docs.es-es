---
title: Diseños de los componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear componentes reutilizables de diseño para las aplicaciones de componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/layouts
ms.openlocfilehash: 31ed940ce40e3ae6e3744418cf241d396308f4fe
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515644"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="f8f82-103">Diseños de los componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="f8f82-103">Razor Components layouts</span></span>

<span data-ttu-id="f8f82-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="f8f82-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="f8f82-105">Normalmente, las aplicaciones contienen más de un componente.</span><span class="sxs-lookup"><span data-stu-id="f8f82-105">Apps typically contain more than one component.</span></span> <span data-ttu-id="f8f82-106">Elementos de diseño, como menús, los mensajes de copyright y logotipos, deben estar presentes en todos los componentes.</span><span class="sxs-lookup"><span data-stu-id="f8f82-106">Layout elements, such as menus, copyright messages, and logos, must be present on all components.</span></span> <span data-ttu-id="f8f82-107">Copiando el código de estos elementos de diseño en todos los componentes de una aplicación no es un enfoque eficaz.</span><span class="sxs-lookup"><span data-stu-id="f8f82-107">Copying the code of these layout elements into all of the components of an app isn't an efficient approach.</span></span> <span data-ttu-id="f8f82-108">Esta duplicación es difícil de mantener y probablemente se lleva a contenido incoherente con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f8f82-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="f8f82-109">*Los diseños* solucionar este problema.</span><span class="sxs-lookup"><span data-stu-id="f8f82-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="f8f82-110">Técnicamente, un diseño es simplemente otro componente.</span><span class="sxs-lookup"><span data-stu-id="f8f82-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="f8f82-111">Un diseño que se define en una plantilla de Razor o en C# de código y pueden contener [enlace de datos](xref:razor-components/components#data-binding), [inserción de dependencias](xref:razor-components/dependency-injection)y otras características de los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="f8f82-111">A layout is defined in a Razor template or in C# code and can contain [data binding](xref:razor-components/components#data-binding), [dependency injection](xref:razor-components/dependency-injection), and other ordinary features of components.</span></span>

<span data-ttu-id="f8f82-112">Dos aspectos adicionales activar un *componente* en un *diseño*</span><span class="sxs-lookup"><span data-stu-id="f8f82-112">Two additional aspects turn a *component* into a *layout*</span></span>

* <span data-ttu-id="f8f82-113">El componente de diseño debe heredar de `LayoutComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="f8f82-113">The layout component must inherit from `LayoutComponentBase`.</span></span> `LayoutComponentBase` <span data-ttu-id="f8f82-114">define un `Body` propiedad que contiene el contenido que se representará dentro del diseño.</span><span class="sxs-lookup"><span data-stu-id="f8f82-114">defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="f8f82-115">Usa el componente de diseño la `Body` propiedad para especificar que el contenido del cuerpo deben representar mediante la sintaxis de Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="f8f82-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="f8f82-116">Durante la representación, `@Body` se sustituye por el contenido del diseño.</span><span class="sxs-lookup"><span data-stu-id="f8f82-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="f8f82-117">Ejemplo de código siguiente muestra la plantilla de Razor de un componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="f8f82-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="f8f82-118">Tenga en cuenta el uso de `LayoutComponentBase` y `@Body`:</span><span class="sxs-lookup"><span data-stu-id="f8f82-118">Note the use of `LayoutComponentBase` and `@Body`:</span></span>

[!code-cshtml[](layouts/sample_snapshot/3.x/MasterLayout.cshtml)]

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="f8f82-119">Usar un diseño en un componente</span><span class="sxs-lookup"><span data-stu-id="f8f82-119">Use a layout in a component</span></span>

<span data-ttu-id="f8f82-120">Utilice la directiva de Razor `@layout` para aplicar un diseño a un componente.</span><span class="sxs-lookup"><span data-stu-id="f8f82-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="f8f82-121">El compilador convierte esta directiva en un `LayoutAttribute`, que se aplica a la clase de componente.</span><span class="sxs-lookup"><span data-stu-id="f8f82-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="f8f82-122">Ejemplo de código siguiente muestra el concepto.</span><span class="sxs-lookup"><span data-stu-id="f8f82-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="f8f82-123">El contenido de este componente se inserta en la *MasterLayout* en la posición del `@Body`:</span><span class="sxs-lookup"><span data-stu-id="f8f82-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```cshtml
@layout MasterLayout
@page "/master-list"

<h2>Master Episode List</h2>
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="f8f82-124">Selección del diseño centralizada</span><span class="sxs-lookup"><span data-stu-id="f8f82-124">Centralized layout selection</span></span>

<span data-ttu-id="f8f82-125">Todas las carpetas de un una aplicación puede contener opcionalmente un archivo de plantilla denominado *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f8f82-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="f8f82-126">El compilador incluye las directivas especificadas en el archivo de importaciones de vista en todas las plantillas de Razor en la misma carpeta y de forma recursiva en todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="f8f82-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="f8f82-127">Por lo tanto, un *_ViewImports.cshtml* que contiene el archivo `@layout MainLayout` garantiza que todos los componentes en una carpeta, use el *MainLayout* diseño.</span><span class="sxs-lookup"><span data-stu-id="f8f82-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="f8f82-128">No es necesario para agregar varias veces `@layout` a todos los *.razor* archivos.</span><span class="sxs-lookup"><span data-stu-id="f8f82-128">There's no need to repeatedly add `@layout` to all of the *.razor* files.</span></span>

<span data-ttu-id="f8f82-129">Tenga en cuenta que la plantilla predeterminada usa la *_ViewImports.cshtml* mecanismo para la selección de diseño.</span><span class="sxs-lookup"><span data-stu-id="f8f82-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="f8f82-130">Contiene una aplicación recién creada el *_ViewImports.cshtml* de archivos en el *componentes, páginas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="f8f82-130">A newly created app contains the *_ViewImports.cshtml* file in the *Components/Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="f8f82-131">Diseños anidados</span><span class="sxs-lookup"><span data-stu-id="f8f82-131">Nested layouts</span></span>

<span data-ttu-id="f8f82-132">Las aplicaciones pueden constar de los diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="f8f82-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="f8f82-133">Un componente puede hacer referencia a un diseño que a su vez hace referencia a otra distribución.</span><span class="sxs-lookup"><span data-stu-id="f8f82-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="f8f82-134">Por ejemplo, los diseños de anidamiento pueden usarse para reflejar una estructura de varios niveles de menú.</span><span class="sxs-lookup"><span data-stu-id="f8f82-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="f8f82-135">Ejemplos de código siguientes muestran cómo utilizar los diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="f8f82-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="f8f82-136">El *EpisodesComponent.cshtml* archivo es el componente para mostrar.</span><span class="sxs-lookup"><span data-stu-id="f8f82-136">The *EpisodesComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="f8f82-137">Tenga en cuenta que el componente hace referencia el diseño `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="f8f82-137">Note that the component references the layout `MasterListLayout`.</span></span>

<span data-ttu-id="f8f82-138">*EpisodesComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f8f82-138">*EpisodesComponent.cshtml*:</span></span>

```cshtml
@layout MasterListLayout
@page "/master-list/episodes"

<h1>Episodes</h1>
```

<span data-ttu-id="f8f82-139">El *MasterListLayout.cshtml* archivo proporciona el `MasterListLayout`.</span><span class="sxs-lookup"><span data-stu-id="f8f82-139">The *MasterListLayout.cshtml* file provides the `MasterListLayout`.</span></span> <span data-ttu-id="f8f82-140">El diseño hace referencia a otro diseño `MasterLayout`, donde se va a insertarse.</span><span class="sxs-lookup"><span data-stu-id="f8f82-140">The layout references another layout, `MasterLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="f8f82-141">*MasterListLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f8f82-141">*MasterListLayout.cshtml*:</span></span>

```cshtml
@layout MasterLayout
@inherits LayoutComponentBase

<nav>
    <!-- Menu structure of master list -->
    ...
</nav>

@Body
```

<span data-ttu-id="f8f82-142">Por último, `MasterLayout` contiene los elementos de diseño de nivel superior, como el encabezado, pie de página y el menú principal.</span><span class="sxs-lookup"><span data-stu-id="f8f82-142">Finally, `MasterLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="f8f82-143">*MasterLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f8f82-143">*MasterLayout.cshtml*:</span></span>

```cshtml
@inherits LayoutComponentBase

<header>...</header>
<nav>...</nav>

@Body
```
