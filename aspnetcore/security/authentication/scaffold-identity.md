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
ms.openlocfilehash: 80cd39af61e856d3ce92db1c26e70788bcdca83d
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725824"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identidad de scaffolding en proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:mvc/razor-pages/ui-class). Las aplicaciones que incluyen la identidad pueden aplicar el scaffolder para agregar el código de fuente contenido en la biblioteca de clase de Razor de identidad (RCL) de forma selectiva. Puede generar código fuente para que pueda modificar el código y cambiar el comportamiento. Por ejemplo, podría indicar a la scaffolder para generar el código que se utiliza en el registro. Código generado tiene prioridad sobre el mismo código en el RCL de identidad. Para obtener el control total de la interfaz de usuario y no utilice el valor predeterminado RCL, vea la sección [crear origen de interfaz de usuario de identidad completa](#full).

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

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
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

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Crear origen de la interfaz de usuario de identidad completa

Para mantener el control completo de la interfaz de usuario de identidad, ejecute el scaffolder de identidad y seleccione **invalidar todos los archivos**.

El código resaltado siguiente muestra los cambios para reemplazar el valor predeterminado de interfaz de usuario de identidad con la identidad en una aplicación web de ASP.NET Core 2.1. Puede hacer esto para tener control total de la interfaz de usuario de identidad.

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Se reemplaza el valor predeterminado de identidad en el código siguiente: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

El siguiente código configura ASP.NET Core para autorizar las páginas de identidad que requieren autorización: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

El siguiente el código establece la cookie de identidad al usar la ruta de acceso de páginas de identidad correcta.
[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Registrar un `IEmailSender` implementación, por ejemplo:

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet4)]