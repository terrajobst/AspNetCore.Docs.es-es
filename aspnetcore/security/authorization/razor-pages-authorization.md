---
title: Razor Pages convenciones de autorización en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo controlar el acceso a las páginas con convenciones que autorizan a los usuarios y permiten a los usuarios anónimos acceder a páginas o carpetas de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994032"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="ef387-103">Razor Pages convenciones de autorización en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef387-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="ef387-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ef387-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ef387-105">Una manera de controlar el acceso en la aplicación Razor Pages es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="ef387-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="ef387-106">Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos tengan acceso a páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="ef387-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="ef387-107">Las convenciones descritas en este tema aplican automáticamente los [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="ef387-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="ef387-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef387-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ef387-109">La aplicación de ejemplo usa la [autenticación de cookies sin ASP.net Core identidad](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="ef387-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="ef387-110">Los conceptos y los ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core identidad.</span><span class="sxs-lookup"><span data-stu-id="ef387-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="ef387-111">Para usar ASP.NET Core identidad, siga las instrucciones de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="ef387-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="ef387-112">Requerir autorización para tener acceso a una página</span><span class="sxs-lookup"><span data-stu-id="ef387-112">Require authorization to access a page</span></span>

<span data-ttu-id="ef387-113">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="ef387-114">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="ef387-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="ef387-115">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="ef387-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="ef387-116">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se puede aplicar a una clase de modelo de página `[Authorize]` con el atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="ef387-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="ef387-117">Para obtener más información, consulte [Authorize Filter Attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="ef387-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="ef387-118">Requerir autorización para tener acceso a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="ef387-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="ef387-119">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="ef387-120">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ef387-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="ef387-121">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="ef387-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="ef387-122">Requerir autorización para tener acceso a una página de área</span><span class="sxs-lookup"><span data-stu-id="ef387-122">Require authorization to access an area page</span></span>

<span data-ttu-id="ef387-123">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página de área en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="ef387-124">El nombre de página es la ruta de acceso del archivo sin una extensión relativa al directorio raíz de páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="ef387-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef387-125">Por ejemplo, el nombre de página de las *áreas de archivo/Identity/pages/Manage/accounts. cshtml* es */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="ef387-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="ef387-126">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="ef387-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="ef387-127">Requerir autorización para tener acceso a una carpeta de áreas</span><span class="sxs-lookup"><span data-stu-id="ef387-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="ef387-128">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las áreas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="ef387-129">La ruta de acceso de la carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="ef387-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef387-130">Por ejemplo, la ruta de acceso de la carpeta para los archivos en *areas/Identity/pages/Manage/* es */Manage*.</span><span class="sxs-lookup"><span data-stu-id="ef387-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="ef387-131">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="ef387-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="ef387-132">Permitir acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="ef387-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="ef387-133">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a una página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="ef387-134">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="ef387-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="ef387-135">Permitir acceso anónimo a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="ef387-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="ef387-136">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="ef387-137">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ef387-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="ef387-138">Nota sobre cómo combinar el acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="ef387-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="ef387-139">Es válido especificar que una carpeta de páginas que requieran autorización y que especifique que una página de esa carpeta permita el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="ef387-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="ef387-140">Sin embargo, el inverso no es válido.</span><span class="sxs-lookup"><span data-stu-id="ef387-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="ef387-141">No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especificar una página dentro de esa carpeta que requiera autorización:</span><span class="sxs-lookup"><span data-stu-id="ef387-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="ef387-142">Se produce un error en la solicitud de autorización en la página privada.</span><span class="sxs-lookup"><span data-stu-id="ef387-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="ef387-143"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Cuando y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, tiene prioridad <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="ef387-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef387-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ef387-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ef387-145">Una manera de controlar el acceso en la aplicación Razor Pages es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="ef387-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="ef387-146">Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos tengan acceso a páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="ef387-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="ef387-147">Las convenciones descritas en este tema aplican automáticamente los [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="ef387-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="ef387-148">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef387-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ef387-149">La aplicación de ejemplo usa la [autenticación de cookies sin ASP.net Core identidad](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="ef387-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="ef387-150">Los conceptos y los ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core identidad.</span><span class="sxs-lookup"><span data-stu-id="ef387-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="ef387-151">Para usar ASP.NET Core identidad, siga las instrucciones de <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="ef387-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="ef387-152">Requerir autorización para tener acceso a una página</span><span class="sxs-lookup"><span data-stu-id="ef387-152">Require authorization to access a page</span></span>

<span data-ttu-id="ef387-153">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="ef387-154">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="ef387-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="ef387-155">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="ef387-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="ef387-156">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se puede aplicar a una clase de modelo de página `[Authorize]` con el atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="ef387-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="ef387-157">Para obtener más información, consulte [Authorize Filter Attribute](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="ef387-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="ef387-158">Requerir autorización para tener acceso a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="ef387-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="ef387-159">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="ef387-160">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ef387-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="ef387-161">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="ef387-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="ef387-162">Requerir autorización para tener acceso a una página de área</span><span class="sxs-lookup"><span data-stu-id="ef387-162">Require authorization to access an area page</span></span>

<span data-ttu-id="ef387-163">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página de área en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="ef387-164">El nombre de página es la ruta de acceso del archivo sin una extensión relativa al directorio raíz de páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="ef387-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef387-165">Por ejemplo, el nombre de página de las *áreas de archivo/Identity/pages/Manage/accounts. cshtml* es */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="ef387-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="ef387-166">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="ef387-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="ef387-167">Requerir autorización para tener acceso a una carpeta de áreas</span><span class="sxs-lookup"><span data-stu-id="ef387-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="ef387-168">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las áreas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="ef387-169">La ruta de acceso de la carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="ef387-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef387-170">Por ejemplo, la ruta de acceso de la carpeta para los archivos en *areas/Identity/pages/Manage/* es */Manage*.</span><span class="sxs-lookup"><span data-stu-id="ef387-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="ef387-171">Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="ef387-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="ef387-172">Permitir acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="ef387-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="ef387-173">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a una página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="ef387-174">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="ef387-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="ef387-175">Permitir acceso anónimo a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="ef387-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="ef387-176">Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="ef387-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="ef387-177">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ef387-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="ef387-178">Nota sobre cómo combinar el acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="ef387-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="ef387-179">Es válido especificar que una carpeta de páginas que requieran autorización y que especifique que una página de esa carpeta permita el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="ef387-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="ef387-180">Sin embargo, el inverso no es válido.</span><span class="sxs-lookup"><span data-stu-id="ef387-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="ef387-181">No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especificar una página dentro de esa carpeta que requiera autorización:</span><span class="sxs-lookup"><span data-stu-id="ef387-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="ef387-182">Se produce un error en la solicitud de autorización en la página privada.</span><span class="sxs-lookup"><span data-stu-id="ef387-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="ef387-183"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Cuando y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, tiene prioridad <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="ef387-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef387-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ef387-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
