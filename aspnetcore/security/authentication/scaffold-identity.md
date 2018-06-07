---
title: Identidad de scaffolding en proyectos de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo aplicar la técnica scaffolding identidad en un proyecto de ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 6202cb4d28cc8d77c1f83c8cee8c90892deef26e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34818982"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identidad de scaffolding en proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:mvc/razor-pages/ui-class). Las aplicaciones que incluyen la identidad pueden aplicar el scaffolder para agregar el código de fuente contenido en la biblioteca de clase de Razor de identidad (RCL) de forma selectiva. Puede generar código fuente para que pueda modificar el código y cambiar el comportamiento. Por ejemplo, podría indicar a la scaffolder para generar el código que se utiliza en el registro. Código generado tiene prioridad sobre el mismo código en el RCL de identidad.

Las aplicaciones que **no** incluyen autenticación puede aplicar el scaffolder para agregar el paquete de identidad RCL. Tiene la opción de seleccionar el código de identidad que se genere.

Aunque el scaffolder genera la mayoría del código necesario, tendrá que actualizar el proyecto para completar el proceso. Este documento explican los pasos necesarios para completar una actualización de la técnica scaffolding de identidad.

Cuando se ejecuta el scaffolder de identidad, un *ScaffoldingReadme.txt* archivo se crea en el directorio del proyecto. El *ScaffoldingReadme.txt* archivo contiene instrucciones generales sobre lo que se necesita para completar la actualización de la técnica scaffolding de identidad. Este documento contiene instrucciones más completas que la *ScaffoldingReadme.txt* archivo.

Se recomienda utilizar un sistema de control de código fuente que se muestran las diferencias de archivo y le permite revertir los cambios. Inspeccionar los cambios después de ejecutar al scaffolder de identidad.

## <a name="scaffold-identity-into-an-empty-project"></a>Identidad de scaffolding en un proyecto vacío

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Agregue las siguientes llamadas resaltadas a la `Startup` clase:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Identidad de scaffolding en un proyecto de Razor sin autorización existente

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*. Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Las migraciones, UseAuthentication y diseño

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

En el `Configure` método de la `Startup` clase, llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Cambios de diseño

Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) para el archivo de diseño:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Identidad de scaffolding en un proyecto de Razor con autorización

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Algunas opciones de identidad se configuran en *Areas/Identity/IdentityHostingStartup.cs*. Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Identidad de scaffolding en un proyecto MVC sin autorización existente

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) a la *Views/Shared/_Layout.cshtml* archivo:

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Mover el *Pages/Shared/_LoginPartial.cshtml* del archivo a *Views/Shared/_LoginPartial.cshtml*

Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*. Para obtener más información, consulte IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Identidad de scaffolding en un proyecto MVC con autorización

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Eliminar el *páginas/Shared* carpeta y los archivos de esa carpeta.
