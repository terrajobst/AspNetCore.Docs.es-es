---
title: Componentes con plantilla de Blazor en ASP.NET Core
author: guardrex
description: Descubra cómo los componentes con plantilla pueden aceptar una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/templated-components
ms.openlocfilehash: b57e3fe186402723607e90b1628062f602c77632
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "79989493"
---
# <a name="aspnet-core-opno-locblazor-templated-components"></a><span data-ttu-id="e5329-103">Componentes con plantilla de Blazor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5329-103">ASP.NET Core Blazor templated components</span></span>

<span data-ttu-id="e5329-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e5329-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="e5329-105">Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="e5329-105">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="e5329-106">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="e5329-106">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="e5329-107">Estos son un par de ejemplos:</span><span class="sxs-lookup"><span data-stu-id="e5329-107">A couple of examples include:</span></span>

* <span data-ttu-id="e5329-108">Un componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.</span><span class="sxs-lookup"><span data-stu-id="e5329-108">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="e5329-109">Un componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="e5329-109">A list component that allows a user to specify a template for rendering items in a list.</span></span>

## <a name="template-parameters"></a><span data-ttu-id="e5329-110">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="e5329-110">Template parameters</span></span>

<span data-ttu-id="e5329-111">Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="e5329-111">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="e5329-112">Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="e5329-112">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="e5329-113">`RenderFragment<T>` toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="e5329-113">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="e5329-114">Componente `TableTemplate`:</span><span class="sxs-lookup"><span data-stu-id="e5329-114">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="e5329-115">Al usar un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="e5329-115">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

> [!NOTE]
> <span data-ttu-id="e5329-116">La restricciones de tipo genérico se admitirán en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="e5329-116">Generic type constraints will be supported in a future release.</span></span> <span data-ttu-id="e5329-117">Para obtener más información, vea [Permitir restricciones de tipo genérico (dotnet/aspnetcore #8433)](https://github.com/dotnet/aspnetcore/issues/8433).</span><span class="sxs-lookup"><span data-stu-id="e5329-117">For more information, see [Allow generic type constraints (dotnet/aspnetcore #8433)](https://github.com/dotnet/aspnetcore/issues/8433).</span></span>

## <a name="template-context-parameters"></a><span data-ttu-id="e5329-118">Parámetros de contexto de la plantilla</span><span class="sxs-lookup"><span data-stu-id="e5329-118">Template context parameters</span></span>

<span data-ttu-id="e5329-119">Los argumentos de los componentes de tipo `RenderFragment<T>` pasados como elementos tienen un parámetro implícito denominado `context` (por ejemplo, en el ejemplo de código anterior, `@context.PetId`), pero puede cambiar el nombre del parámetro mediante el atributo `Context` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="e5329-119">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="e5329-120">En el ejemplo siguiente, el atributo `RowTemplate` del elemento `Context` especifica el parámetro `pet`:</span><span class="sxs-lookup"><span data-stu-id="e5329-120">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="e5329-121">También puede especificar el atributo `Context` en el elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="e5329-121">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="e5329-122">El atributo `Context` especificado se aplica a todos los parámetros de plantilla especificados.</span><span class="sxs-lookup"><span data-stu-id="e5329-122">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="e5329-123">Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste).</span><span class="sxs-lookup"><span data-stu-id="e5329-123">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="e5329-124">En el ejemplo siguiente, el atributo `Context` aparece en el elemento `TableTemplate` y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="e5329-124">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

## <a name="generic-typed-components"></a><span data-ttu-id="e5329-125">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="e5329-125">Generic-typed components</span></span>

<span data-ttu-id="e5329-126">Los componentes con plantilla suelen tener tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="e5329-126">Templated components are often generically typed.</span></span> <span data-ttu-id="e5329-127">Por ejemplo, se puede usar un componente `ListViewTemplate` genérico para representar valores `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="e5329-127">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="e5329-128">Para definir un componente genérico, utilice la directiva [`@typeparam`](xref:mvc/views/razor#typeparam) para especificar parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="e5329-128">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="e5329-129">Al usar componentes de tipo genérico, el parámetro de tipo se infiere si es posible:</span><span class="sxs-lookup"><span data-stu-id="e5329-129">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="e5329-130">De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="e5329-130">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="e5329-131">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="e5329-131">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```
