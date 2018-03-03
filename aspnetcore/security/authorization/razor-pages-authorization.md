---
title: "Convenciones de autorización de páginas de Razor en ASP.NET Core"
author: guardrex
description: "Obtener información sobre cómo controlar el acceso a las páginas con las convenciones que autorizan a los usuarios y permitir que los usuarios anónimos pueden tener acceso a páginas o carpetas de páginas."
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: bbef653c6cf968527e753df9c853f5972640cc03
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="bc40c-103">Convenciones de autorización de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc40c-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="bc40c-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bc40c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bc40c-105">Una manera de controlar el acceso de la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="bc40c-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="bc40c-106">Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos pueden tener acceso a las páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="bc40c-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="bc40c-107">Se aplican las convenciones descritas en este tema automáticamente [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="bc40c-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="bc40c-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bc40c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="bc40c-109">Requerir autorización para acceder a una página</span><span class="sxs-lookup"><span data-stu-id="bc40c-109">Require authorization to access a page</span></span>

<span data-ttu-id="bc40c-110">Use la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="bc40c-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="bc40c-111">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="bc40c-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="bc40c-112">Un [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="bc40c-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="bc40c-113">Requerir autorización para acceder a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="bc40c-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="bc40c-114">Use la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="bc40c-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="bc40c-115">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="bc40c-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="bc40c-116">Un [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="bc40c-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="bc40c-117">Permitir el acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="bc40c-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="bc40c-118">Use la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="bc40c-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="bc40c-119">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="bc40c-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="bc40c-120">Permitir el acceso anónimo a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="bc40c-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="bc40c-121">Use la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="bc40c-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="bc40c-122">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="bc40c-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="bc40c-123">Tenga en cuenta en combinar el acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="bc40c-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="bc40c-124">Es perfectamente válido para especificar que una carpeta de páginas requieren autorización y especificar que una página dentro de esa carpeta permite el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="bc40c-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="bc40c-125">Sin embargo, lo contrario, no es cierto.</span><span class="sxs-lookup"><span data-stu-id="bc40c-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="bc40c-126">No se puede declarar una carpeta de páginas para el acceso anónimo y especificar una página dentro de autorización:</span><span class="sxs-lookup"><span data-stu-id="bc40c-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="bc40c-127">Que requieren la autorización en la página privada no funcionará porque cuando tanto el `AllowAnonymousFilter` y `AuthorizeFilter` filtros se aplican a la página, el `AllowAnonymousFilter` wins y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="bc40c-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="bc40c-128">Vea también</span><span class="sxs-lookup"><span data-stu-id="bc40c-128">See also</span></span>

* [<span data-ttu-id="bc40c-129">Proveedores personalizados de rutas y modelos de página de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="bc40c-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="bc40c-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) (clase)</span><span class="sxs-lookup"><span data-stu-id="bc40c-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
