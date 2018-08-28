---
title: Identidad de scaffold en proyectos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo aplicar la técnica scaffolding en un proyecto de ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e35836fa9c20729da7c857243410833749b3a595
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055853"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="de8ff-103">Identidad de scaffold en proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de8ff-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="de8ff-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="de8ff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="de8ff-105">ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="de8ff-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="de8ff-106">Las aplicaciones que incluyen identidad pueden aplicar el proveedor de scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clase de Razor de identidad (RCL).</span><span class="sxs-lookup"><span data-stu-id="de8ff-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="de8ff-107">Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento;</span><span class="sxs-lookup"><span data-stu-id="de8ff-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="de8ff-108">así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro.</span><span class="sxs-lookup"><span data-stu-id="de8ff-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="de8ff-109">Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity.</span><span class="sxs-lookup"><span data-stu-id="de8ff-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="de8ff-110">Para obtener el control completo de la interfaz de usuario y no utilice el valor predeterminado RCL, consulte la sección [crear origen de la interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="de8ff-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="de8ff-111">Aplicaciones que hacen lo **no** incluyen autenticación puede aplicar el proveedor de scaffolding para agregar el paquete RCL identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="de8ff-112">Existe la posibilidad de seleccionar el código de Identity que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="de8ff-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="de8ff-113">Aunque el proveedor de scaffolding genera la mayor parte del código necesario, tendrá que actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="de8ff-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="de8ff-114">Este documento explica los pasos necesarios para completar una actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="de8ff-115">Cuando se ejecuta el proveedor de scaffolding de identidad, un *ScaffoldingReadme.txt* archivo se crea en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="de8ff-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="de8ff-116">El *ScaffoldingReadme.txt* archivo contiene instrucciones generales sobre lo que se necesita para completar la actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="de8ff-117">Este documento contiene instrucciones más completas que la *ScaffoldingReadme.txt* archivo.</span><span class="sxs-lookup"><span data-stu-id="de8ff-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="de8ff-118">Se recomienda usar un sistema de control de código fuente que se muestra las diferencias de archivo y le permite revertir los cambios.</span><span class="sxs-lookup"><span data-stu-id="de8ff-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="de8ff-119">Inspeccione los cambios después de ejecutar el proveedor de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="de8ff-120">Los servicios son necesarios para usar [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), [confirmación y la contraseña de recuperación de la cuenta](xref:security/authentication/accconfirm)y otras características de seguridad con la identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="de8ff-121">Códigos auxiliares de servicio o servicios no se generan cuando el scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="de8ff-122">Servicios para habilitar estas características deben agregarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="de8ff-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="de8ff-123">Por ejemplo, vea [requerir confirmación por correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="de8ff-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="de8ff-124">Identidad de scaffold en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="de8ff-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="de8ff-125">Agregue las siguientes llamadas resaltadas para el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="de8ff-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="de8ff-126">Identidad de scaffold en un proyecto de Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="de8ff-126">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="de8ff-127">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="de8ff-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="de8ff-128">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="de8ff-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="de8ff-129">Las migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="de8ff-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="de8ff-130">Habilitar la autenticación</span><span class="sxs-lookup"><span data-stu-id="de8ff-130">Enable authentication</span></span>

<span data-ttu-id="de8ff-131">En el `Configure` método de la `Startup` class, llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="de8ff-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="de8ff-132">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="de8ff-132">Layout changes</span></span>

<span data-ttu-id="de8ff-133">Opcional: Agregue el inicio de sesión parcial (`_LoginPartial`) para el archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="de8ff-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="de8ff-134">Identidad de scaffold en un proyecto de Razor sin autorización</span><span class="sxs-lookup"><span data-stu-id="de8ff-134">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="de8ff-135">Algunas opciones de identidad se configuran en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="de8ff-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="de8ff-136">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="de8ff-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="de8ff-137">Identidad de scaffold en un proyecto MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="de8ff-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="de8ff-138">Opcional: Agregue el inicio de sesión parcial (`_LoginPartial`) a la *Views/Shared/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="de8ff-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="de8ff-139">Mover el *Pages/Shared/_LoginPartial.cshtml* del archivo a *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="de8ff-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="de8ff-140">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="de8ff-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="de8ff-141">Para obtener más información, consulte IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="de8ff-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="de8ff-142">Llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="de8ff-142">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="de8ff-143">Identidad de scaffold en un proyecto MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="de8ff-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="de8ff-144">Eliminar el *páginas/Shared* carpeta y los archivos en esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="de8ff-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="de8ff-145">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="de8ff-145">Create full identity UI source</span></span>

<span data-ttu-id="de8ff-146">Para mantener el control completo de la interfaz de usuario de identidad, ejecute el proveedor de scaffolding de identidad y seleccione **reemplazar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="de8ff-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="de8ff-147">El código resaltado siguiente muestra los cambios para reemplazar el valor predeterminado de la interfaz de usuario de identidad con la identidad en una aplicación web de ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="de8ff-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="de8ff-148">Es posible que desee hacer esto para tener control total sobre la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="de8ff-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="de8ff-149">Se reemplaza el valor predeterminado de identidad en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="de8ff-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="de8ff-150">El siguiente el código establece la [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="de8ff-150">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="de8ff-151">Registrar un `IEmailSender` implementación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="de8ff-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
