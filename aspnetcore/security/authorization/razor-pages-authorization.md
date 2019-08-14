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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor Pages convenciones de autorización en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Una manera de controlar el acceso en la aplicación Razor Pages es usar las convenciones de autorización en el inicio. Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos tengan acceso a páginas individuales o carpetas de páginas. Las convenciones descritas en este tema aplican automáticamente los [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo usa la [autenticación de cookies sin ASP.net Core identidad](xref:security/authentication/cookie). Los conceptos y los ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core identidad. Para usar ASP.NET Core identidad, siga las instrucciones de <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Requerir autorización para tener acceso a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se puede aplicar a una clase de modelo de página `[Authorize]` con el atributo de filtro. Para obtener más información, consulte [Authorize Filter Attribute](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requerir autorización para tener acceso a una carpeta de páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Requerir autorización para tener acceso a una página de área

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página de área en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

El nombre de página es la ruta de acceso del archivo sin una extensión relativa al directorio raíz de páginas del área especificada. Por ejemplo, el nombre de página de las *áreas de archivo/Identity/pages/Manage/accounts. cshtml* es */Manage/accounts*.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requerir autorización para tener acceso a una carpeta de áreas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las áreas de una carpeta en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

La ruta de acceso de la carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de páginas del área especificada. Por ejemplo, la ruta de acceso de la carpeta para los archivos en *areas/Identity/pages/Manage/* es */Manage*.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Permitir acceso anónimo a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a una página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir acceso anónimo a una carpeta de páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sobre cómo combinar el acceso autorizado y anónimo

Es válido especificar que una carpeta de páginas que requieran autorización y que especifique que una página de esa carpeta permita el acceso anónimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Sin embargo, el inverso no es válido. No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especificar una página dentro de esa carpeta que requiera autorización:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Se produce un error en la solicitud de autorización en la página privada. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Cuando y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, tiene prioridad <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y controla el acceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Una manera de controlar el acceso en la aplicación Razor Pages es usar las convenciones de autorización en el inicio. Estas convenciones permiten autorizar a los usuarios y permitir que los usuarios anónimos tengan acceso a páginas individuales o carpetas de páginas. Las convenciones descritas en este tema aplican automáticamente los [filtros de autorización](xref:mvc/controllers/filters#authorization-filters) para controlar el acceso.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

La aplicación de ejemplo usa la [autenticación de cookies sin ASP.net Core identidad](xref:security/authentication/cookie). Los conceptos y los ejemplos que se muestran en este tema se aplican igualmente a las aplicaciones que usan ASP.NET Core identidad. Para usar ASP.NET Core identidad, siga las instrucciones de <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Requerir autorización para tener acceso a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se puede aplicar a una clase de modelo de página `[Authorize]` con el atributo de filtro. Para obtener más información, consulte [Authorize Filter Attribute](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Requerir autorización para tener acceso a una carpeta de páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Requerir autorización para tener acceso a una página de área

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a la página de área en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

El nombre de página es la ruta de acceso del archivo sin una extensión relativa al directorio raíz de páginas del área especificada. Por ejemplo, el nombre de página de las *áreas de archivo/Identity/pages/Manage/accounts. cshtml* es */Manage/accounts*.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Requerir autorización para tener acceso a una carpeta de áreas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> agregar un a todas las áreas de una carpeta en la ruta de acceso especificada:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

La ruta de acceso de la carpeta es la ruta de acceso de la carpeta relativa al directorio raíz de páginas del área especificada. Por ejemplo, la ruta de acceso de la carpeta para los archivos en *areas/Identity/pages/Manage/* es */Manage*.

Para especificar una [Directiva de autorización](xref:security/authorization/policies), use una [sobrecarga AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Permitir acceso anónimo a una página

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a una página en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la Razor Pages ruta de acceso relativa raíz sin una extensión y que solo contiene barras diagonales.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Permitir acceso anónimo a una carpeta de páginas

Use la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Convención a <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> través de para <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> agregar un a todas las páginas de una carpeta en la ruta de acceso especificada:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

La ruta de acceso especificada es la ruta de acceso del motor de vista, que es la ruta de acceso relativa de la raíz Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sobre cómo combinar el acceso autorizado y anónimo

Es válido especificar que una carpeta de páginas que requieran autorización y que especifique que una página de esa carpeta permita el acceso anónimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Sin embargo, el inverso no es válido. No se puede declarar una carpeta de páginas para el acceso anónimo y, a continuación, especificar una página dentro de esa carpeta que requiera autorización:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Se produce un error en la solicitud de autorización en la página privada. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Cuando y <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> se aplican a la página, tiene prioridad <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> y controla el acceso.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
