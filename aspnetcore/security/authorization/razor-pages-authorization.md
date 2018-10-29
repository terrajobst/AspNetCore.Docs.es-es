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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenciones de autorización de las páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Una manera de controlar el acceso en la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio. Estas convenciones permiten autorizar a los usuarios y permite que los usuarios anónimos tener acceso a páginas individuales o carpetas de páginas. Aplican las convenciones descritas en este tema automáticamente [los filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo usa [autenticación de cookies sin ASP.NET Core Identity](xref:security/authentication/cookie). La cuenta de usuario para el usuario hipotética, María Rodriguez, está codificado en la aplicación. Utilice el nombre de usuario de correo electrónico "maria.rodriguez@contoso.com" y cualquier contraseña para iniciar sesión el usuario. El usuario se autentica en el `AuthenticateUser` método en el *Pages/Account/Login.cshtml.cs* archivo. En un ejemplo del mundo real, el usuario podría autenticarse en una base de datos. Para usar ASP.NET Core Identity, siga las instrucciones de la [Introducción a la identidad en ASP.NET Core](xref:security/authentication/identity) tema. Los conceptos y ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core Identity.

## <a name="require-authorization-to-access-a-page"></a>Requerir autorización para acceder a una página

Use la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.

Un [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Un `AuthorizeFilter` puede aplicarse a una clase de modelo de página con el `[Authorize]` atributo de filtro. Para obtener más información, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requerir autorización para acceder a una carpeta de páginas

Use la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las páginas en una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

Un [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Requerir autorización para acceder a una página de área

Use la [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a la página de área en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

El nombre de página es la ruta de acceso del archivo sin extensión relativa al directorio raíz de páginas para el área especificada. Por ejemplo, el nombre de página para el archivo *Areas/Identity/Pages/Manage/Accounts.cshtml* es *cuentas/administración/*.

Un [AuthorizeAreaPage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requerir autorización para acceder a una carpeta de áreas

Use la [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a todas las áreas en una carpeta en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

La ruta de acceso de carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de las páginas del área especificada. Por ejemplo, la ruta de carpeta para los archivos en *áreas/identidad/páginas/administrar o* es */administrar*.

Un [AuthorizeAreaFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) está disponible si tiene que especificar una directiva de autorización.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Permitir el acceso anónimo a una página

Use la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir el acceso anónimo a la carpeta páginas

Use la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convención a través de [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para agregar un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a todas las páginas en una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Tenga en cuenta sobre la combinación de acceso autorizado y anónimo

Es perfectamente válido para especificar que una carpeta de páginas requieren autorización y especificar que una página dentro de esa carpeta permite el acceso anónimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Sin embargo, no a la inversa. No se puede declarar una carpeta de páginas para el acceso anónimo y especificar una página dentro de autorización:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Requerir autorización en la página privada no funcionará porque cuando tanto el `AllowAnonymousFilter` y `AuthorizeFilter` filtros se aplican a la página, el `AllowAnonymousFilter` wins y controla el acceso.

## <a name="additional-resources"></a>Recursos adicionales

* [Proveedores personalizados de rutas y modelos de página de páginas de Razor](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) clase
