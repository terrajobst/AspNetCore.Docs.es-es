---
title: Identidad de scaffolding en proyectos de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo aplicar la técnica scaffolding identidad en un proyecto de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276323"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="a80d5-103">Identidad de scaffolding en proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a80d5-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="a80d5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a80d5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a80d5-105">ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a80d5-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a80d5-106">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolder para agregar el código de fuente contenido en la biblioteca de clase de Razor de identidad (RCL) de forma selectiva.</span><span class="sxs-lookup"><span data-stu-id="a80d5-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="a80d5-107">Puede generar código fuente para que pueda modificar el código y cambiar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="a80d5-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="a80d5-108">Por ejemplo, podría indicar a la scaffolder para generar el código que se utiliza en el registro.</span><span class="sxs-lookup"><span data-stu-id="a80d5-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="a80d5-109">Código generado tiene prioridad sobre el mismo código en el RCL de identidad.</span><span class="sxs-lookup"><span data-stu-id="a80d5-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="a80d5-110">Para obtener el control total de la interfaz de usuario y no utilice el valor predeterminado RCL, vea la sección [crear origen de interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="a80d5-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="a80d5-111">Las aplicaciones que **no** incluyen autenticación puede aplicar el scaffolder para agregar el paquete de identidad RCL.</span><span class="sxs-lookup"><span data-stu-id="a80d5-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="a80d5-112">Tiene la opción de seleccionar el código de identidad que se genere.</span><span class="sxs-lookup"><span data-stu-id="a80d5-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="a80d5-113">Aunque el scaffolder genera la mayoría del código necesario, tendrá que actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="a80d5-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="a80d5-114">Este documento explican los pasos necesarios para completar una actualización de la técnica scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="a80d5-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="a80d5-115">Cuando se ejecuta el scaffolder de identidad, un *ScaffoldingReadme.txt* archivo se crea en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a80d5-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="a80d5-116">El *ScaffoldingReadme.txt* archivo contiene instrucciones generales sobre lo que se necesita para completar la actualización de la técnica scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="a80d5-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="a80d5-117">Este documento contiene instrucciones más completas que la *ScaffoldingReadme.txt* archivo.</span><span class="sxs-lookup"><span data-stu-id="a80d5-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="a80d5-118">Se recomienda utilizar un sistema de control de código fuente que se muestran las diferencias de archivo y le permite revertir los cambios.</span><span class="sxs-lookup"><span data-stu-id="a80d5-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="a80d5-119">Inspeccionar los cambios después de ejecutar al scaffolder de identidad.</span><span class="sxs-lookup"><span data-stu-id="a80d5-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="a80d5-120">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="a80d5-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="a80d5-121">Agregue las siguientes llamadas resaltadas a la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="a80d5-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="a80d5-122">Identidad de scaffolding en un proyecto de Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="a80d5-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="a80d5-123">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a80d5-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a80d5-124">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a80d5-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="a80d5-125">Las migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="a80d5-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a80d5-126">En el `Configure` método de la `Startup` clase, llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a80d5-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="a80d5-127">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="a80d5-127">Layout changes</span></span>

<span data-ttu-id="a80d5-128">Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) para el archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="a80d5-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="a80d5-129">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="a80d5-129">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="a80d5-130">Algunas opciones de identidad se configuran en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a80d5-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a80d5-131">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a80d5-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="a80d5-132">Identidad de scaffolding en un proyecto MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="a80d5-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="a80d5-133">Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) a la *Views/Shared/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="a80d5-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="a80d5-134">Mover el *Pages/Shared/_LoginPartial.cshtml* del archivo a *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a80d5-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="a80d5-135">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a80d5-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a80d5-136">Para obtener más información, consulte IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="a80d5-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a80d5-137">Llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a80d5-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="a80d5-138">Identidad de scaffolding en un proyecto MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="a80d5-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="a80d5-139">Eliminar el *páginas/Shared* carpeta y los archivos de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="a80d5-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="a80d5-140">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="a80d5-140">Create full identity UI source</span></span>

<span data-ttu-id="a80d5-141">Para mantener el control completo de la interfaz de usuario de identidad, ejecute el scaffolder de identidad y seleccione **invalidar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="a80d5-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="a80d5-142">El código resaltado siguiente muestra los cambios para reemplazar el valor predeterminado de interfaz de usuario de identidad con la identidad en una aplicación web de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a80d5-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="a80d5-143">Puede hacer esto para tener control total de la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="a80d5-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="a80d5-144">Se reemplaza el valor predeterminado de identidad en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a80d5-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="a80d5-145">El siguiente el código establece la [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="a80d5-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="a80d5-146">Registrar un `IEmailSender` implementación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a80d5-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
