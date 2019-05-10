---
title: Convenciones de autorización de las páginas de Razor en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo controlar el acceso a las páginas con las convenciones que autorizar a los usuarios y permitir que los usuarios anónimos tener acceso a las páginas o las carpetas de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: ff061f96f30cd893b903403de760a172c924cf06
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895422"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenciones de autorización de las páginas de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

Una manera de controlar el acceso en la aplicación de las páginas de Razor es usar las convenciones de autorización en el inicio. Estas convenciones permiten autorizar a los usuarios y permite que los usuarios anónimos tener acceso a páginas individuales o carpetas de páginas. Aplican las convenciones descritas en este tema automáticamente [los filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo usa [autenticación de cookies sin ASP.NET Core Identity](xref:security/authentication/cookie). Los conceptos y ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core Identity. Para usar ASP.NET Core Identity, siga las instrucciones de <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Requerir autorización para acceder a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a la página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.

Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizePage sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> puede aplicarse a una clase de modelo de página con el `[Authorize]` atributo de filtro. Para obtener más información, consulte [atributo de filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requerir autorización para acceder a una carpeta de páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a todas las páginas en una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeFolder sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Requerir autorización para acceder a una página de área

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a la página de área en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

El nombre de página es la ruta de acceso del archivo sin extensión relativa al directorio raíz de páginas para el área especificada. Por ejemplo, el nombre de página para el archivo *Areas/Identity/Pages/Manage/Accounts.cshtml* es *cuentas/administración/*.

Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeAreaPage sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requerir autorización para acceder a una carpeta de áreas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a todas las áreas en una carpeta en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

La ruta de acceso de carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de las páginas del área especificada. Por ejemplo, la ruta de carpeta para los archivos en *áreas/identidad/páginas/administrar o* es */administrar*.

Para especificar un [directiva de autorización](xref:security/authorization/policies), utilice un [AuthorizeAreaFolder sobrecarga](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Permitir el acceso anónimo a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar una <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a una página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta relativa de raíz de las páginas de Razor sin una extensión y que contiene solo barras diagonales.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir el acceso anónimo a la carpeta páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convención a través de <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> para agregar un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a todas las páginas en una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de las páginas de Razor raíz.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Tenga en cuenta sobre la combinación de acceso autorizado y anónimo

Es válido para especificar que una carpeta de páginas que requieren autorización y de especificar que una página dentro de esa carpeta permite el acceso anónimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Sin embargo, la inversa, no es válida. No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especifique una página dentro de esa carpeta que requiere autorización:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Se produce un error que requiere autorización en la página privada. Cuando tanto el <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, el <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> tiene prioridad y controla el acceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
