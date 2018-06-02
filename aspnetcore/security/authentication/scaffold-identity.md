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
ms.openlocfilehash: a43b7bbaf1f90d3373b3846bc3f4f32be6b80bd4
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729615"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="bb5b4-103">Identidad de scaffolding en proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb5b4-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="bb5b4-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bb5b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb5b4-105">ASP.NET Core 2.1 y versiones posteriores proporciona [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="bb5b4-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="bb5b4-106">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolder para agregar el código de fuente contenido en la biblioteca de clase de Razor de identidad (RCL) de forma selectiva.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="bb5b4-107">Puede generar código fuente para que pueda modificar el código y cambiar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="bb5b4-108">Por ejemplo, podría indicar a la scaffolder para generar el código que se utiliza en el registro.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="bb5b4-109">Código generado tiene prioridad sobre el mismo código en el RCL de identidad.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="bb5b4-110">Las aplicaciones que **no** incluyen autenticación puede aplicar el scaffolder para agregar el paquete de identidad RCL.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="bb5b4-111">Tiene la opción de seleccionar el código de identidad que se genere.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="bb5b4-112">Aunque el scaffolder genera la mayoría del código necesario, tendrá que actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="bb5b4-113">Este documento explican los pasos necesarios para completar una actualización de la técnica scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="bb5b4-114">Cuando se ejecuta el scaffolder de identidad, un *ScaffoldingReadme.txt* archivo se crea en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="bb5b4-115">El *ScaffoldingReadme.txt* archivo contiene instrucciones generales sobre lo que se necesita para completar la actualización de la técnica scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="bb5b4-116">Este documento contiene instrucciones más completas que la lectura del *ScaffoldingReadme.txt* archivo.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="bb5b4-117">Se recomienda utilizar un sistema de control de código fuente que se muestran las diferencias de archivo y le permite revertir los cambios.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="bb5b4-118">Inspeccionar los cambios después de ejecutar al scaffolder de identidad.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="bb5b4-119">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="bb5b4-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="bb5b4-120">Agregue las siguientes llamadas resaltadas a la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="bb5b4-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="bb5b4-121">Identidad de scaffolding en un proyecto de Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="bb5b4-121">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="bb5b4-122">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb5b4-123">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb5b4-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="bb5b4-124">En el `Configure` método de la `Startup` clase, llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="bb5b4-124">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="bb5b4-125">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="bb5b4-125">Layout changes</span></span>

<span data-ttu-id="bb5b4-126">Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) para el archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="bb5b4-126">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="bb5b4-127">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="bb5b4-127">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="bb5b4-128">Algunas opciones de identidad se configuran en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-128">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb5b4-129">Para obtener más información, consulte [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb5b4-129">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="bb5b4-130">Identidad de scaffolding en un proyecto MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="bb5b4-130">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="bb5b4-131">Opcional: Agregar el inicio de sesión parcial (`_LoginPartial`) a la *Views/Shared/_Layout.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="bb5b4-131">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="bb5b4-132">Mover el *Pages/Shared/_LoginPartial.cshtml* del archivo a *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb5b4-132">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="bb5b4-133">Identidad se ha configurado en *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-133">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb5b4-134">Para obtener más información, consulte IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-134">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="bb5b4-135">Llame a [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="bb5b4-135">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="bb5b4-136">Identidad de scaffolding en un proyecto MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="bb5b4-136">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="bb5b4-137">Eliminar el *páginas/Shared* carpeta y los archivos de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="bb5b4-137">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
