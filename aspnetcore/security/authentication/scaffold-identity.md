---
title: Identidad de scaffolding en proyectos de ASP.NET Core
author: rick-anderson
description: Aprenda a aplicar una identidad scaffolding en un proyecto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: b3e077aeac11e62d9e992884100476f7be35b59a
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2020
ms.locfileid: "76972045"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="64236-103">Identidad de scaffolding en proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64236-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="64236-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64236-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="64236-105">ASP.NET Core proporciona [ASP.net Core Identity](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="64236-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="64236-106">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clases Razor de identidad (RCL).</span><span class="sxs-lookup"><span data-stu-id="64236-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="64236-107">Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento;</span><span class="sxs-lookup"><span data-stu-id="64236-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="64236-108">así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro.</span><span class="sxs-lookup"><span data-stu-id="64236-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="64236-109">Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity.</span><span class="sxs-lookup"><span data-stu-id="64236-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="64236-110">Para obtener el control total de la interfaz de usuario y no usar el valor predeterminado de RCL, consulte la sección creación de un origen de la [interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="64236-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="64236-111">Las aplicaciones que **no** incluyen autenticación pueden aplicar el scaffolding para agregar el paquete de identidad de RCL.</span><span class="sxs-lookup"><span data-stu-id="64236-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="64236-112">Existe la posibilidad de seleccionar el código de Identity que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="64236-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="64236-113">Aunque el scaffolding genera la mayor parte del código necesario, debe actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="64236-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="64236-114">En este documento se explican los pasos necesarios para completar una actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="64236-115">Se recomienda usar un sistema de control de código fuente que muestre las diferencias de archivos y le permite deshacer los cambios.</span><span class="sxs-lookup"><span data-stu-id="64236-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="64236-116">Inspeccione los cambios después de ejecutar el scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="64236-117">Los servicios son necesarios cuando se usa la [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), la [confirmación de la cuenta y la recuperación de contraseña](xref:security/authentication/accconfirm)y otras características de seguridad con identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="64236-118">Los servicios o los códigos auxiliares de servicio no se generan cuando se trata de una identidad de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64236-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="64236-119">Los servicios para habilitar estas características deben agregarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="64236-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="64236-120">Por ejemplo, vea [requerir confirmación de correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="64236-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="64236-121">Este documento contiene instrucciones más completas que el archivo *ScaffoldingReadme. txt* que se genera al ejecutar el scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64236-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="64236-122">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="64236-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="64236-123">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="64236-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="64236-124">Identidad scaffolding en un proyecto Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="64236-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="64236-125">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-126">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="64236-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="64236-127">Migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="64236-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="64236-128">Habilitar autenticación</span><span class="sxs-lookup"><span data-stu-id="64236-128">Enable authentication</span></span>

<span data-ttu-id="64236-129">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="64236-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="64236-130">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="64236-130">Layout changes</span></span>

<span data-ttu-id="64236-131">Opcional: agregar el inicio de sesión parcial (`_LoginPartial`) al archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="64236-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="64236-132">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="64236-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="64236-133">Algunas opciones de identidad están configuradas en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-134">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="64236-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="64236-135">Identidad de scaffolding en un proyecto de MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="64236-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="64236-136">Opcional: agregue el inicio de sesión parcial (`_LoginPartial`) al archivo *views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="64236-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="64236-137">Mueva el archivo *pages/Shared/_LoginPartial. cshtml* a *views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="64236-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="64236-138">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-139">Para obtener más información, vea IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="64236-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="64236-140">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="64236-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="64236-141">Identidad de scaffolding en un proyecto de MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="64236-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="64236-142">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="64236-142">Create full identity UI source</span></span>

<span data-ttu-id="64236-143">Para mantener el control total de la interfaz de usuario de identidad, ejecute el scaffolding de identidad y seleccione **invalidar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="64236-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="64236-144">En el código resaltado siguiente se muestran los cambios para reemplazar la interfaz de usuario de identidad predeterminada por la identidad en una aplicación Web de ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="64236-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="64236-145">Puede que desee hacer esto para tener un control total de la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="64236-146">La identidad predeterminada se reemplaza en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="64236-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="64236-147">El código siguiente establece los valores de [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="64236-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="64236-148">Registre una implementación de `IEmailSender`, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="64236-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="64236-149">Deshabilitar registro (página)</span><span class="sxs-lookup"><span data-stu-id="64236-149">Disable register page</span></span>

<span data-ttu-id="64236-150">Para deshabilitar el registro de usuario:</span><span class="sxs-lookup"><span data-stu-id="64236-150">To disable user registration:</span></span>

* <span data-ttu-id="64236-151">Identidad scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64236-151">Scaffold Identity.</span></span> <span data-ttu-id="64236-152">Incluya account. Register, Account. login y account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="64236-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="64236-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="64236-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="64236-154">Actualice *areas/Identity/pages/Account/Register. cshtml. CS* para que los usuarios no puedan registrarse desde este punto de conexión:</span><span class="sxs-lookup"><span data-stu-id="64236-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="64236-155">Actualice *areas/Identity/pages/Account/Register. cshtml* para que sean coherentes con los cambios anteriores:</span><span class="sxs-lookup"><span data-stu-id="64236-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="64236-156">Comente o quite el vínculo de registro de las *áreas/identidad/páginas/cuenta/inicio de sesión. cshtml.*</span><span class="sxs-lookup"><span data-stu-id="64236-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="64236-157">Actualice la página *áreas/identidad/páginas/cuenta/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="64236-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="64236-158">Quite el código y los vínculos del archivo cshtml.</span><span class="sxs-lookup"><span data-stu-id="64236-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="64236-159">Quite el código de confirmación del `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="64236-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="64236-160">Usar otra aplicación para agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="64236-160">Use another app to add users</span></span>

<span data-ttu-id="64236-161">Proporcionar un mecanismo para agregar usuarios fuera de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="64236-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="64236-162">Entre las opciones para agregar usuarios se incluyen:</span><span class="sxs-lookup"><span data-stu-id="64236-162">Options to add users include:</span></span>

* <span data-ttu-id="64236-163">Una aplicación Web de administración dedicada.</span><span class="sxs-lookup"><span data-stu-id="64236-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="64236-164">Una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="64236-164">A console app.</span></span>

<span data-ttu-id="64236-165">En el código siguiente se describe un enfoque para agregar usuarios:</span><span class="sxs-lookup"><span data-stu-id="64236-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="64236-166">Una lista de usuarios se lee en la memoria.</span><span class="sxs-lookup"><span data-stu-id="64236-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="64236-167">Se genera una contraseña única segura para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="64236-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="64236-168">El usuario se agrega a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="64236-169">Se notifica al usuario y se le indica que cambie la contraseña.</span><span class="sxs-lookup"><span data-stu-id="64236-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="64236-170">En el código siguiente se describe cómo agregar un usuario:</span><span class="sxs-lookup"><span data-stu-id="64236-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="64236-171">Se puede seguir un enfoque similar para escenarios de producción.</span><span class="sxs-lookup"><span data-stu-id="64236-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="64236-172">Impedir la publicación de recursos de identidad estáticos</span><span class="sxs-lookup"><span data-stu-id="64236-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="64236-173">Para evitar la publicación de recursos de identidad estáticos en la raíz web, consulte <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="64236-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64236-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64236-174">Additional resources</span></span>

* [<span data-ttu-id="64236-175">Cambios en el código de autenticación en ASP.NET Core 2,1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="64236-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="64236-176">ASP.NET Core 2,1 y versiones posteriores proporcionan [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="64236-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="64236-177">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clases Razor de identidad (RCL).</span><span class="sxs-lookup"><span data-stu-id="64236-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="64236-178">Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento;</span><span class="sxs-lookup"><span data-stu-id="64236-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="64236-179">así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro.</span><span class="sxs-lookup"><span data-stu-id="64236-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="64236-180">Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity.</span><span class="sxs-lookup"><span data-stu-id="64236-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="64236-181">Para obtener el control total de la interfaz de usuario y no usar el valor predeterminado de RCL, consulte la sección creación de un origen de la [interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="64236-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="64236-182">Las aplicaciones que **no** incluyen autenticación pueden aplicar el scaffolding para agregar el paquete de identidad de RCL.</span><span class="sxs-lookup"><span data-stu-id="64236-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="64236-183">Existe la posibilidad de seleccionar el código de Identity que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="64236-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="64236-184">Aunque el scaffolding genera la mayor parte del código necesario, tendrá que actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="64236-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="64236-185">En este documento se explican los pasos necesarios para completar una actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="64236-186">Cuando se ejecuta el scaffolding de identidad, se crea un archivo *ScaffoldingReadme. txt* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="64236-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="64236-187">El archivo *ScaffoldingReadme. txt* contiene instrucciones generales sobre lo que se necesita para completar la actualización del scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="64236-188">Este documento contiene instrucciones más completas que el archivo *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="64236-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="64236-189">Se recomienda usar un sistema de control de código fuente que muestre las diferencias de archivos y le permite deshacer los cambios.</span><span class="sxs-lookup"><span data-stu-id="64236-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="64236-190">Inspeccione los cambios después de ejecutar el scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="64236-191">Los servicios son necesarios cuando se usa la [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), la [confirmación de la cuenta y la recuperación de contraseña](xref:security/authentication/accconfirm)y otras características de seguridad con identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="64236-192">Los servicios o los códigos auxiliares de servicio no se generan cuando se trata de una identidad de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64236-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="64236-193">Los servicios para habilitar estas características deben agregarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="64236-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="64236-194">Por ejemplo, vea [requerir confirmación de correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="64236-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="64236-195">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="64236-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="64236-196">Agregue las siguientes llamadas resaltadas a la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="64236-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="64236-197">Identidad scaffolding en un proyecto Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="64236-197">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="64236-198">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-199">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="64236-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="64236-200">Migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="64236-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="64236-201">Habilitar autenticación</span><span class="sxs-lookup"><span data-stu-id="64236-201">Enable authentication</span></span>

<span data-ttu-id="64236-202">En el método `Configure` de la clase `Startup`, llame a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después de `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="64236-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="64236-203">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="64236-203">Layout changes</span></span>

<span data-ttu-id="64236-204">Opcional: agregar el inicio de sesión parcial (`_LoginPartial`) al archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="64236-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="64236-205">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="64236-205">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="64236-206">Algunas opciones de identidad están configuradas en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-207">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="64236-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="64236-208">Identidad de scaffolding en un proyecto de MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="64236-208">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="64236-209">Opcional: agregue el inicio de sesión parcial (`_LoginPartial`) al archivo *views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="64236-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="64236-210">Mueva el archivo *pages/Shared/_LoginPartial. cshtml* a *views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="64236-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="64236-211">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="64236-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="64236-212">Para obtener más información, vea IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="64236-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="64236-213">Llame a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después de `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="64236-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="64236-214">Identidad de scaffolding en un proyecto de MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="64236-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="64236-215">Elimine las *páginas/carpetas compartidas* y los archivos de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="64236-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="64236-216">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="64236-216">Create full identity UI source</span></span>

<span data-ttu-id="64236-217">Para mantener el control total de la interfaz de usuario de identidad, ejecute el scaffolding de identidad y seleccione **invalidar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="64236-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="64236-218">En el código resaltado siguiente se muestran los cambios para reemplazar la interfaz de usuario de identidad predeterminada por la identidad en una aplicación Web de ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="64236-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="64236-219">Puede que desee hacer esto para tener un control total de la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="64236-220">La identidad predeterminada se reemplaza en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="64236-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="64236-221">El código siguiente establece los valores de [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="64236-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="64236-222">Registre una implementación de `IEmailSender`, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="64236-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="64236-223">Deshabilitar registro (página)</span><span class="sxs-lookup"><span data-stu-id="64236-223">Disable register page</span></span>

<span data-ttu-id="64236-224">Para deshabilitar el registro de usuario:</span><span class="sxs-lookup"><span data-stu-id="64236-224">To disable user registration:</span></span>

* <span data-ttu-id="64236-225">Identidad scaffolding.</span><span class="sxs-lookup"><span data-stu-id="64236-225">Scaffold Identity.</span></span> <span data-ttu-id="64236-226">Incluya account. Register, Account. login y account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="64236-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="64236-227">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="64236-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="64236-228">Actualice *areas/Identity/pages/Account/Register. cshtml. CS* para que los usuarios no puedan registrarse desde este punto de conexión:</span><span class="sxs-lookup"><span data-stu-id="64236-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="64236-229">Actualice *areas/Identity/pages/Account/Register. cshtml* para que sean coherentes con los cambios anteriores:</span><span class="sxs-lookup"><span data-stu-id="64236-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="64236-230">Comente o quite el vínculo de registro de las *áreas/identidad/páginas/cuenta/inicio de sesión. cshtml.*</span><span class="sxs-lookup"><span data-stu-id="64236-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="64236-231">Actualice la página *áreas/identidad/páginas/cuenta/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="64236-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="64236-232">Quite el código y los vínculos del archivo cshtml.</span><span class="sxs-lookup"><span data-stu-id="64236-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="64236-233">Quite el código de confirmación del `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="64236-233">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="64236-234">Usar otra aplicación para agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="64236-234">Use another app to add users</span></span>

<span data-ttu-id="64236-235">Proporcionar un mecanismo para agregar usuarios fuera de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="64236-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="64236-236">Entre las opciones para agregar usuarios se incluyen:</span><span class="sxs-lookup"><span data-stu-id="64236-236">Options to add users include:</span></span>

* <span data-ttu-id="64236-237">Una aplicación Web de administración dedicada.</span><span class="sxs-lookup"><span data-stu-id="64236-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="64236-238">Una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="64236-238">A console app.</span></span>

<span data-ttu-id="64236-239">En el código siguiente se describe un enfoque para agregar usuarios:</span><span class="sxs-lookup"><span data-stu-id="64236-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="64236-240">Una lista de usuarios se lee en la memoria.</span><span class="sxs-lookup"><span data-stu-id="64236-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="64236-241">Se genera una contraseña única segura para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="64236-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="64236-242">El usuario se agrega a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="64236-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="64236-243">Se notifica al usuario y se le indica que cambie la contraseña.</span><span class="sxs-lookup"><span data-stu-id="64236-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="64236-244">En el código siguiente se describe cómo agregar un usuario:</span><span class="sxs-lookup"><span data-stu-id="64236-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="64236-245">Se puede seguir un enfoque similar para escenarios de producción.</span><span class="sxs-lookup"><span data-stu-id="64236-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64236-246">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64236-246">Additional resources</span></span>

* [<span data-ttu-id="64236-247">Cambios en el código de autenticación en ASP.NET Core 2,1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="64236-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end