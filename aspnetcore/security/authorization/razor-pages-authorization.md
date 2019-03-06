---
title: Convenciones de autorización de las páginas de Razor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo controlar el acceso a las páginas con las convenciones que autorizar a los usuarios y permitir que los usuarios anónimos tener acceso a las páginas o las carpetas de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345514"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="a68f2-103">Convenciones de autorización de las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a68f2-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="a68f2-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a68f2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a68f2-105">Una manera de controlar el acceso en la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="a68f2-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="a68f2-106">Estas convenciones permiten autorizar a los usuarios y permite que los usuarios anónimos tener acceso a páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="a68f2-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="a68f2-107">Aplican las convenciones descritas en este tema automáticamente [los filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="a68f2-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="a68f2-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a68f2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a68f2-109">La aplicación de ejemplo usa [autenticación de cookies sin ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="a68f2-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="a68f2-110">Los conceptos y ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="a68f2-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="a68f2-111">Para usar ASP.NET Core Identity, siga las instrucciones de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="a68f2-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="a68f2-112">Requerir autorización para acceder a una página</span><span class="sxs-lookup"><span data-stu-id="a68f2-112">Require authorization to access a page</span></span>

<span data-ttu-id="a68f2-113">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a la página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="a68f2-114">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="a68f2-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="a68f2-115">Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizePage sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="a68f2-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="a68f2-116">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> puede aplicarse a una clase de modelo de página con el `[Authorize]` atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="a68f2-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="a68f2-117">Para obtener más información, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="a68f2-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="a68f2-118">Requerir autorización para acceder a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="a68f2-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="a68f2-119">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a todas las páginas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="a68f2-120">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="a68f2-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="a68f2-121">Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeFolder sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="a68f2-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="a68f2-122">Requerir autorización para acceder a una página de área</span><span class="sxs-lookup"><span data-stu-id="a68f2-122">Require authorization to access an area page</span></span>

<span data-ttu-id="a68f2-123">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a la página de área en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="a68f2-124">El nombre de página es la ruta de acceso del archivo sin extensión relativa al directorio raíz de páginas para el área especificada.</span><span class="sxs-lookup"><span data-stu-id="a68f2-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="a68f2-125">Por ejemplo, el nombre de página para el archivo *Areas/Identity/Pages/Manage/Accounts.cshtml* es *cuentas/administración/*.</span><span class="sxs-lookup"><span data-stu-id="a68f2-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="a68f2-126">Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeAreaPage sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="a68f2-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="a68f2-127">Requerir autorización para acceder a una carpeta de áreas</span><span class="sxs-lookup"><span data-stu-id="a68f2-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="a68f2-128">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a todas las áreas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="a68f2-129">La ruta de acceso de carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de las páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="a68f2-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="a68f2-130">Por ejemplo, la ruta de carpeta para los archivos en *áreas/identidad/páginas/administrar o* es */administrar*.</span><span class="sxs-lookup"><span data-stu-id="a68f2-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="a68f2-131">Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeAreaFolder sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="a68f2-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="a68f2-132">Permitir el acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="a68f2-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="a68f2-133">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar una <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a una página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="a68f2-134">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="a68f2-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="a68f2-135">Permitir el acceso anónimo a la carpeta páginas</span><span class="sxs-lookup"><span data-stu-id="a68f2-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="a68f2-136">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a todas las páginas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="a68f2-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="a68f2-137">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="a68f2-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="a68f2-138">Tenga en cuenta sobre la combinación de acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="a68f2-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="a68f2-139">Es válido para especificar que una carpeta de páginas que requieren autorización y de especificar que una página dentro de esa carpeta permite el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="a68f2-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="a68f2-140">Sin embargo, la inversa, no es válida.</span><span class="sxs-lookup"><span data-stu-id="a68f2-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="a68f2-141">No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especifique una página dentro de esa carpeta que requiere autorización:</span><span class="sxs-lookup"><span data-stu-id="a68f2-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="a68f2-142">Se produce un error que requiere autorización en la página privada.</span><span class="sxs-lookup"><span data-stu-id="a68f2-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="a68f2-143">Cuando tanto el <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, el <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> tiene prioridad y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="a68f2-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a68f2-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a68f2-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
