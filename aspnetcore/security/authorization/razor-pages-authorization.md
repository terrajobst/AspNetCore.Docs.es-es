---
title: Convenciones de autorización de páginas de Razor en ASP.NET Core
author: guardrex
description: Obtener información sobre cómo controlar el acceso a las páginas con las convenciones que autorizan a los usuarios y permitir que los usuarios anónimos pueden tener acceso a páginas o carpetas de páginas.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 35a21156c001d8703e09e604129c4c2c500fe25f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734658"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e51d5-103">Convenciones de autorización de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e51d5-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="e51d5-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e51d5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e51d5-105">Una manera de controlar el acceso de la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="e51d5-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e51d5-106">Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos pueden tener acceso a las páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="e51d5-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e51d5-107">Se aplican las convenciones descritas en este tema automáticamente [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="e51d5-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e51d5-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e51d5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e51d5-109">Los usos de la aplicación de ejemplo [autenticación con cookies sin ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e51d5-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e51d5-110">La cuenta de usuario para el usuario hipotética, Maria Rodríguez, está codificada en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e51d5-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="e51d5-111">Use el nombre de usuario de correo electrónico "maria.rodriguez@contoso.com" y una contraseña para iniciar sesión en el usuario.</span><span class="sxs-lookup"><span data-stu-id="e51d5-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="e51d5-112">El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="e51d5-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="e51d5-113">En un ejemplo del mundo real, se debería autenticar el usuario en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e51d5-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="e51d5-114">Para usar ASP.NET Core Identity, siga las instrucciones de la [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity) tema.</span><span class="sxs-lookup"><span data-stu-id="e51d5-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="e51d5-115">Los conceptos y ejemplos que se muestran en este tema se aplican por igual a las aplicaciones que usan ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="e51d5-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e51d5-116">Requerir autorización para acceder a una página</span><span class="sxs-lookup"><span data-stu-id="e51d5-116">Require authorization to access a page</span></span>

<span data-ttu-id="e51d5-117">Use la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="e51d5-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e51d5-118">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="e51d5-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e51d5-119">Un [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e51d5-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="e51d5-120">Un `AuthorizeFilter` se puede aplicar a una clase de modelo de página con el `[Authorize]` atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="e51d5-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e51d5-121">Para obtener más información, consulte [atributo de filtro de autorizar](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e51d5-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e51d5-122">Requerir autorización para acceder a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="e51d5-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e51d5-123">Use la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="e51d5-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e51d5-124">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="e51d5-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e51d5-125">Un [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e51d5-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e51d5-126">Permitir el acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="e51d5-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="e51d5-127">Use la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="e51d5-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e51d5-128">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="e51d5-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e51d5-129">Permitir el acceso anónimo a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="e51d5-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e51d5-130">Use la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar una [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta en la ruta especificada:</span><span class="sxs-lookup"><span data-stu-id="e51d5-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e51d5-131">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="e51d5-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e51d5-132">Tenga en cuenta en combinar el acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="e51d5-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e51d5-133">Es perfectamente válido para especificar que una carpeta de páginas requieren autorización y especificar que una página dentro de esa carpeta permite el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="e51d5-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e51d5-134">Sin embargo, lo contrario, no es cierto.</span><span class="sxs-lookup"><span data-stu-id="e51d5-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="e51d5-135">No se puede declarar una carpeta de páginas para el acceso anónimo y especificar una página dentro de autorización:</span><span class="sxs-lookup"><span data-stu-id="e51d5-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="e51d5-136">Que requieren la autorización en la página privada no funcionará porque cuando tanto el `AllowAnonymousFilter` y `AuthorizeFilter` filtros se aplican a la página, el `AllowAnonymousFilter` wins y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="e51d5-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e51d5-137">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e51d5-137">Additional resources</span></span>

* [<span data-ttu-id="e51d5-138">Proveedores personalizados de rutas y modelos de página de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="e51d5-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="e51d5-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) (clase)</span><span class="sxs-lookup"><span data-stu-id="e51d5-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
