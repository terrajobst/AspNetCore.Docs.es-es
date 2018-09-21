---
title: Identidad de scaffold en proyectos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo aplicar la técnica scaffolding en un proyecto de ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 37ad9897fbc5eb1822ed2413334b4fce9050296b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523043"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identidad de scaffold en proyectos de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:razor-pages/ui-class). Las aplicaciones que incluyen identidad pueden aplicar el proveedor de scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clase de Razor de identidad (RCL). Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento; así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro. Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity. Para obtener el control completo de la interfaz de usuario y no utilice el valor predeterminado RCL, consulte la sección [crear origen de la interfaz de usuario de identidad completa](#full).

Aplicaciones que hacen lo **no** incluyen autenticación puede aplicar el proveedor de scaffolding para agregar el paquete RCL identidad. Existe la posibilidad de seleccionar el código de Identity que se va a generar.

Aunque el proveedor de scaffolding genera la mayor parte del código necesario, tendrá que actualizar el proyecto para completar el proceso. Este documento explica los pasos necesarios para completar una actualización de scaffolding de identidad.

Cuando se ejecuta el proveedor de scaffolding de identidad, un *ScaffoldingReadme.txt* archivo se crea en el directorio del proyecto. El *ScaffoldingReadme.txt* archivo contiene instrucciones generales sobre lo que se necesita para completar la actualización de scaffolding de identidad. Este documento contiene instrucciones más completas que la *ScaffoldingReadme.txt* archivo.

Se recomienda usar un sistema de control de código fuente que se muestra las diferencias de archivo y le permite revertir los cambios. Inspeccione los cambios después de ejecutar el proveedor de scaffolding de identidad.

> [!NOTE]
> Los servicios son necesarios para usar [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), [confirmación y la contraseña de recuperación de la cuenta](xref:security/authentication/accconfirm)y otras características de seguridad con la identidad. Códigos auxiliares de servicio o servicios no se generan cuando el scaffolding de identidad. Servicios para habilitar estas características deben agregarse manualmente. Por ejemplo, vea [requerir confirmación por correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Identidad de scaffold en un proyecto vacío

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Agregue las siguientes llamadas resaltadas para el `Startup` clase:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Identidad de scaffold en un proyecto de Razor sin autorización existente

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

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Habilitar la autenticación

En el `Configure` método de la `Startup` class, llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Cambios de diseño

Opcional: Agregue el inicio de sesión parcial (`_LoginPartial`) para el archivo de diseño:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Identidad de scaffold en un proyecto de Razor sin autorización

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Algunas opciones de identidad se configuran en *Areas/Identity/IdentityHostingStartup.cs*. Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Identidad de scaffold en un proyecto MVC sin autorización existente

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

Opcional: Agregue el inicio de sesión parcial (`_LoginPartial`) a la *Views/Shared/_Layout.cshtml* archivo:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Mover el *Pages/Shared/_LoginPartial.cshtml* del archivo a *Views/Shared/_LoginPartial.cshtml*

Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*. Para obtener más información, consulte IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Identidad de scaffold en un proyecto MVC con autorización

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Eliminar el *páginas/Shared* carpeta y los archivos en esa carpeta.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Crear origen de la interfaz de usuario de identidad completa

Para mantener el control completo de la interfaz de usuario de identidad, ejecute el proveedor de scaffolding de identidad y seleccione **reemplazar todos los archivos**.

El código resaltado siguiente muestra los cambios para reemplazar el valor predeterminado de la interfaz de usuario de identidad con la identidad en una aplicación web de ASP.NET Core 2.1. Es posible que desee hacer esto para tener control total sobre la interfaz de usuario de identidad.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Se reemplaza el valor predeterminado de identidad en el código siguiente:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

El siguiente el código establece la [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Registrar un `IEmailSender` implementación, por ejemplo:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Recursos adicionales

* [Cambios en el código de autenticación en ASP.NET Core 2.1 y versiones posteriores](xref:migration/20_21#changes-to-authentication-code)