---
title: Convenciones de autorización de las páginas de Razor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo controlar el acceso a las páginas con las convenciones que autorizar a los usuarios y permitir que los usuarios anónimos tener acceso a las páginas o las carpetas de páginas.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207269"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e9372-103">Convenciones de autorización de las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9372-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="e9372-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e9372-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e9372-105">Una manera de controlar el acceso en la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio.</span><span class="sxs-lookup"><span data-stu-id="e9372-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e9372-106">Estas convenciones permiten autorizar a los usuarios y permite que los usuarios anónimos tener acceso a páginas individuales o carpetas de páginas.</span><span class="sxs-lookup"><span data-stu-id="e9372-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e9372-107">Aplican las convenciones descritas en este tema automáticamente [los filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.</span><span class="sxs-lookup"><span data-stu-id="e9372-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e9372-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9372-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e9372-109">La aplicación de ejemplo usa [autenticación de cookies sin ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e9372-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e9372-110">La cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e9372-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="e9372-111">Utilice el nombre de usuario de correo electrónico "maria.rodriguez@contoso.com" y cualquier contraseña para iniciar sesión el usuario.</span><span class="sxs-lookup"><span data-stu-id="e9372-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="e9372-112">El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="e9372-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="e9372-113">En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e9372-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="e9372-114">Para usar ASP.NET Core Identity, siga las instrucciones de la [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity) tema.</span><span class="sxs-lookup"><span data-stu-id="e9372-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="e9372-115">Los conceptos y ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="e9372-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e9372-116">Requerir autorización para acceder a una página</span><span class="sxs-lookup"><span data-stu-id="e9372-116">Require authorization to access a page</span></span>

<span data-ttu-id="e9372-117">Use la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e9372-118">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="e9372-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e9372-119">Un [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e9372-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="e9372-120">Un `AuthorizeFilter` puede aplicarse a una clase de modelo de página con el `[Authorize]` atributo de filtro.</span><span class="sxs-lookup"><span data-stu-id="e9372-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e9372-121">Para obtener más información, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e9372-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e9372-122">Requerir autorización para acceder a una carpeta de páginas</span><span class="sxs-lookup"><span data-stu-id="e9372-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e9372-123">Use la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e9372-124">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="e9372-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e9372-125">Un [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e9372-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="e9372-126">Requerir autorización para acceder a una página de área</span><span class="sxs-lookup"><span data-stu-id="e9372-126">Require authorization to access an area page</span></span>

<span data-ttu-id="e9372-127">Use la [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página de área en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="e9372-128">El nombre de página es la ruta de acceso del archivo sin extensión relativa al directorio raíz de páginas para el área especificada.</span><span class="sxs-lookup"><span data-stu-id="e9372-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e9372-129">Por ejemplo, el nombre de página para el archivo *Areas/Identity/Pages/Manage/Accounts.cshtml* es *cuentas/administración/*.</span><span class="sxs-lookup"><span data-stu-id="e9372-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="e9372-130">Un [AuthorizeAreaPage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e9372-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="e9372-131">Requerir autorización para acceder a una carpeta de áreas</span><span class="sxs-lookup"><span data-stu-id="e9372-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="e9372-132">Use la [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las áreas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="e9372-133">La ruta de acceso de carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de las páginas del área especificada.</span><span class="sxs-lookup"><span data-stu-id="e9372-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e9372-134">Por ejemplo, la ruta de carpeta para los archivos en *áreas/identidad/páginas/administrar o* es */administrar*.</span><span class="sxs-lookup"><span data-stu-id="e9372-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="e9372-135">Un [AuthorizeAreaFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.</span><span class="sxs-lookup"><span data-stu-id="e9372-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e9372-136">Permitir el acceso anónimo a una página</span><span class="sxs-lookup"><span data-stu-id="e9372-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="e9372-137">Use la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e9372-138">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.</span><span class="sxs-lookup"><span data-stu-id="e9372-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e9372-139">Permitir el acceso anónimo a la carpeta páginas</span><span class="sxs-lookup"><span data-stu-id="e9372-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e9372-140">Use la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta en la ruta de acceso especificada:</span><span class="sxs-lookup"><span data-stu-id="e9372-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e9372-141">La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.</span><span class="sxs-lookup"><span data-stu-id="e9372-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e9372-142">Tenga en cuenta sobre la combinación de acceso autorizado y anónimo</span><span class="sxs-lookup"><span data-stu-id="e9372-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e9372-143">Es perfectamente válido para especificar que una carpeta de páginas requieren autorización y especificar que una página dentro de esa carpeta permite el acceso anónimo:</span><span class="sxs-lookup"><span data-stu-id="e9372-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e9372-144">Sin embargo, no a la inversa.</span><span class="sxs-lookup"><span data-stu-id="e9372-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="e9372-145">No se puede declarar una carpeta de páginas para el acceso anónimo y especificar una página dentro de autorización:</span><span class="sxs-lookup"><span data-stu-id="e9372-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="e9372-146">Requerir autorización en la página privada no funcionará porque cuando tanto el `AllowAnonymousFilter` y `AuthorizeFilter` filtros se aplican a la página, el `AllowAnonymousFilter` wins y controla el acceso.</span><span class="sxs-lookup"><span data-stu-id="e9372-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9372-147">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e9372-147">Additional resources</span></span>

* [<span data-ttu-id="e9372-148">Proveedores personalizados de rutas y modelos de página de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="e9372-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="e9372-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) clase</span><span class="sxs-lookup"><span data-stu-id="e9372-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
