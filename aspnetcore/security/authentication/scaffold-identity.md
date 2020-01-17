---
title: Identidad de scaffolding en proyectos de ASP.NET Core
author: rick-anderson
description: Aprenda a aplicar una identidad scaffolding en un proyecto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a0e9603cbca8c7f5771b0acf1a60839dffc89d4e
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146490"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="8edcf-103">Identidad de scaffolding en proyectos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8edcf-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="8edcf-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8edcf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8edcf-105">ASP.NET Core proporciona [ASP.net Core Identity](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8edcf-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8edcf-106">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clases Razor de identidad (RCL).</span><span class="sxs-lookup"><span data-stu-id="8edcf-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="8edcf-107">Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento;</span><span class="sxs-lookup"><span data-stu-id="8edcf-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="8edcf-108">así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro.</span><span class="sxs-lookup"><span data-stu-id="8edcf-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="8edcf-109">Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity.</span><span class="sxs-lookup"><span data-stu-id="8edcf-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="8edcf-110">Para obtener el control total de la interfaz de usuario y no usar el valor predeterminado de RCL, consulte la sección creación de un origen de la [interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="8edcf-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="8edcf-111">Las aplicaciones que **no** incluyen autenticación pueden aplicar el scaffolding para agregar el paquete de identidad de RCL.</span><span class="sxs-lookup"><span data-stu-id="8edcf-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="8edcf-112">Existe la posibilidad de seleccionar el código de Identity que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="8edcf-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="8edcf-113">Aunque el scaffolding genera la mayor parte del código necesario, debe actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="8edcf-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="8edcf-114">En este documento se explican los pasos necesarios para completar una actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="8edcf-115">Se recomienda usar un sistema de control de código fuente que muestre las diferencias de archivos y le permite deshacer los cambios.</span><span class="sxs-lookup"><span data-stu-id="8edcf-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="8edcf-116">Inspeccione los cambios después de ejecutar el scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="8edcf-117">Los servicios son necesarios cuando se usa la [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), la [confirmación de la cuenta y la recuperación de contraseña](xref:security/authentication/accconfirm)y otras características de seguridad con identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="8edcf-118">Los servicios o los códigos auxiliares de servicio no se generan cuando se trata de una identidad de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8edcf-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="8edcf-119">Los servicios para habilitar estas características deben agregarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="8edcf-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="8edcf-120">Por ejemplo, vea [requerir confirmación de correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="8edcf-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="8edcf-121">Este documento contiene instrucciones más completas que el archivo *ScaffoldingReadme. txt* que se genera al ejecutar el scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8edcf-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="8edcf-122">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="8edcf-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="8edcf-123">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8edcf-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="8edcf-124">Identidad scaffolding en un proyecto Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="8edcf-124">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="8edcf-125">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-126">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8edcf-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="8edcf-127">Migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="8edcf-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="8edcf-128">Habilitar autenticación</span><span class="sxs-lookup"><span data-stu-id="8edcf-128">Enable authentication</span></span>

<span data-ttu-id="8edcf-129">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8edcf-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="8edcf-130">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="8edcf-130">Layout changes</span></span>

<span data-ttu-id="8edcf-131">Opcional: agregar el inicio de sesión parcial (`_LoginPartial`) al archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="8edcf-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="8edcf-132">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="8edcf-132">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="8edcf-133">Algunas opciones de identidad están configuradas en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-134">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8edcf-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="8edcf-135">Identidad de scaffolding en un proyecto de MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="8edcf-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="8edcf-136">Opcional: agregue el inicio de sesión parcial (`_LoginPartial`) al archivo *views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="8edcf-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="8edcf-137">Mueva el archivo *pages/Shared/_LoginPartial. cshtml* a *views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="8edcf-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="8edcf-138">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-139">Para obtener más información, vea IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="8edcf-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="8edcf-140">Actualice la clase `Startup` con código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8edcf-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="8edcf-141">Identidad de scaffolding en un proyecto de MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="8edcf-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="8edcf-142">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="8edcf-142">Create full identity UI source</span></span>

<span data-ttu-id="8edcf-143">Para mantener el control total de la interfaz de usuario de identidad, ejecute el scaffolding de identidad y seleccione **invalidar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="8edcf-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="8edcf-144">En el código resaltado siguiente se muestran los cambios para reemplazar la interfaz de usuario de identidad predeterminada por la identidad en una aplicación Web de ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8edcf-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="8edcf-145">Puede que desee hacer esto para tener un control total de la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="8edcf-146">La identidad predeterminada se reemplaza en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8edcf-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="8edcf-147">El código siguiente establece los valores de [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="8edcf-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="8edcf-148">Registre una implementación de `IEmailSender`, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8edcf-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="8edcf-149">Deshabilitar registro (página)</span><span class="sxs-lookup"><span data-stu-id="8edcf-149">Disable register page</span></span>

<span data-ttu-id="8edcf-150">Para deshabilitar el registro de usuario:</span><span class="sxs-lookup"><span data-stu-id="8edcf-150">To disable user registration:</span></span>

* <span data-ttu-id="8edcf-151">Identidad scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8edcf-151">Scaffold Identity.</span></span> <span data-ttu-id="8edcf-152">Incluya account. Register, Account. login y account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="8edcf-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="8edcf-153">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8edcf-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="8edcf-154">Actualice *areas/Identity/pages/Account/Register. cshtml. CS* para que los usuarios no puedan registrarse desde este punto de conexión:</span><span class="sxs-lookup"><span data-stu-id="8edcf-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="8edcf-155">Actualice *areas/Identity/pages/Account/Register. cshtml* para que sean coherentes con los cambios anteriores:</span><span class="sxs-lookup"><span data-stu-id="8edcf-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="8edcf-156">Comente o quite el vínculo de registro de las *áreas/identidad/páginas/cuenta/inicio de sesión. cshtml.*</span><span class="sxs-lookup"><span data-stu-id="8edcf-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="8edcf-157">Actualice la página *áreas/identidad/páginas/cuenta/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="8edcf-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="8edcf-158">Quite el código y los vínculos del archivo cshtml.</span><span class="sxs-lookup"><span data-stu-id="8edcf-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="8edcf-159">Quite el código de confirmación del `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="8edcf-159">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="8edcf-160">Usar otra aplicación para agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="8edcf-160">Use another app to add users</span></span>

<span data-ttu-id="8edcf-161">Proporcionar un mecanismo para agregar usuarios fuera de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="8edcf-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="8edcf-162">Entre las opciones para agregar usuarios se incluyen:</span><span class="sxs-lookup"><span data-stu-id="8edcf-162">Options to add users include:</span></span>

* <span data-ttu-id="8edcf-163">Una aplicación Web de administración dedicada.</span><span class="sxs-lookup"><span data-stu-id="8edcf-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="8edcf-164">Una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="8edcf-164">A console app.</span></span>

<span data-ttu-id="8edcf-165">En el código siguiente se describe un enfoque para agregar usuarios:</span><span class="sxs-lookup"><span data-stu-id="8edcf-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="8edcf-166">Una lista de usuarios se lee en la memoria.</span><span class="sxs-lookup"><span data-stu-id="8edcf-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="8edcf-167">Se genera una contraseña única segura para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="8edcf-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="8edcf-168">El usuario se agrega a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="8edcf-169">Se notifica al usuario y se le indica que cambie la contraseña.</span><span class="sxs-lookup"><span data-stu-id="8edcf-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="8edcf-170">En el código siguiente se describe cómo agregar un usuario:</span><span class="sxs-lookup"><span data-stu-id="8edcf-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="8edcf-171">Se puede seguir un enfoque similar para escenarios de producción.</span><span class="sxs-lookup"><span data-stu-id="8edcf-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="8edcf-172">Impedir la publicación de recursos de identidad estáticos</span><span class="sxs-lookup"><span data-stu-id="8edcf-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="8edcf-173">Para evitar la publicación de recursos de identidad estáticos en la raíz Web, consulte <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="8edcf-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8edcf-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8edcf-174">Additional resources</span></span>

* [<span data-ttu-id="8edcf-175">Cambios en el código de autenticación en ASP.NET Core 2,1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="8edcf-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8edcf-176">ASP.NET Core 2,1 y versiones posteriores proporcionan [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8edcf-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8edcf-177">Las aplicaciones que incluyen la identidad pueden aplicar el scaffolding para agregar de forma selectiva el código fuente contenido en la biblioteca de clases Razor de identidad (RCL).</span><span class="sxs-lookup"><span data-stu-id="8edcf-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="8edcf-178">Puede que quiera generar código fuente que le permita modificar un código y cambiar el comportamiento;</span><span class="sxs-lookup"><span data-stu-id="8edcf-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="8edcf-179">así, por ejemplo, podría indicar al proveedor de scaffolding que generara el código que se usa en el registro.</span><span class="sxs-lookup"><span data-stu-id="8edcf-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="8edcf-180">Dicho código generado tendrá prioridad sobre el mismo código en el RCL de Identity.</span><span class="sxs-lookup"><span data-stu-id="8edcf-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="8edcf-181">Para obtener el control total de la interfaz de usuario y no usar el valor predeterminado de RCL, consulte la sección creación de un origen de la [interfaz de usuario de identidad completa](#full).</span><span class="sxs-lookup"><span data-stu-id="8edcf-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="8edcf-182">Las aplicaciones que **no** incluyen autenticación pueden aplicar el scaffolding para agregar el paquete de identidad de RCL.</span><span class="sxs-lookup"><span data-stu-id="8edcf-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="8edcf-183">Existe la posibilidad de seleccionar el código de Identity que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="8edcf-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="8edcf-184">Aunque el scaffolding genera la mayor parte del código necesario, tendrá que actualizar el proyecto para completar el proceso.</span><span class="sxs-lookup"><span data-stu-id="8edcf-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="8edcf-185">En este documento se explican los pasos necesarios para completar una actualización de scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="8edcf-186">Cuando se ejecuta el scaffolding de identidad, se crea un archivo *ScaffoldingReadme. txt* en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8edcf-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="8edcf-187">El archivo *ScaffoldingReadme. txt* contiene instrucciones generales sobre lo que se necesita para completar la actualización del scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="8edcf-188">Este documento contiene instrucciones más completas que el archivo *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="8edcf-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="8edcf-189">Se recomienda usar un sistema de control de código fuente que muestre las diferencias de archivos y le permite deshacer los cambios.</span><span class="sxs-lookup"><span data-stu-id="8edcf-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="8edcf-190">Inspeccione los cambios después de ejecutar el scaffolding de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="8edcf-191">Los servicios son necesarios cuando se usa la [autenticación en dos fases](xref:security/authentication/identity-enable-qrcodes), la [confirmación de la cuenta y la recuperación de contraseña](xref:security/authentication/accconfirm)y otras características de seguridad con identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="8edcf-192">Los servicios o los códigos auxiliares de servicio no se generan cuando se trata de una identidad de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8edcf-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="8edcf-193">Los servicios para habilitar estas características deben agregarse manualmente.</span><span class="sxs-lookup"><span data-stu-id="8edcf-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="8edcf-194">Por ejemplo, vea [requerir confirmación de correo electrónico](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="8edcf-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="8edcf-195">Identidad de scaffolding en un proyecto vacío</span><span class="sxs-lookup"><span data-stu-id="8edcf-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="8edcf-196">Agregue las siguientes llamadas resaltadas a la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="8edcf-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="8edcf-197">Identidad scaffolding en un proyecto Razor sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="8edcf-197">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="8edcf-198">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-199">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8edcf-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="8edcf-200">Migraciones, UseAuthentication y diseño</span><span class="sxs-lookup"><span data-stu-id="8edcf-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="8edcf-201">Habilitar autenticación</span><span class="sxs-lookup"><span data-stu-id="8edcf-201">Enable authentication</span></span>

<span data-ttu-id="8edcf-202">En el método `Configure` de la clase `Startup`, llame a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después de `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="8edcf-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="8edcf-203">Cambios de diseño</span><span class="sxs-lookup"><span data-stu-id="8edcf-203">Layout changes</span></span>

<span data-ttu-id="8edcf-204">Opcional: agregar el inicio de sesión parcial (`_LoginPartial`) al archivo de diseño:</span><span class="sxs-lookup"><span data-stu-id="8edcf-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="8edcf-205">Identidad de scaffolding en un proyecto de Razor con autorización</span><span class="sxs-lookup"><span data-stu-id="8edcf-205">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="8edcf-206">Algunas opciones de identidad están configuradas en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-207">para obtener más información, vea [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8edcf-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="8edcf-208">Identidad de scaffolding en un proyecto de MVC sin autorización existente</span><span class="sxs-lookup"><span data-stu-id="8edcf-208">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="8edcf-209">Opcional: agregue el inicio de sesión parcial (`_LoginPartial`) al archivo *views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="8edcf-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="8edcf-210">Mueva el archivo *pages/Shared/_LoginPartial. cshtml* a *views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="8edcf-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="8edcf-211">La identidad se configura en *areas/Identity/IdentityHostingStartup. CS*.</span><span class="sxs-lookup"><span data-stu-id="8edcf-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8edcf-212">Para obtener más información, vea IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="8edcf-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="8edcf-213">Llame a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) después de `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="8edcf-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="8edcf-214">Identidad de scaffolding en un proyecto de MVC con autorización</span><span class="sxs-lookup"><span data-stu-id="8edcf-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="8edcf-215">Elimine las *páginas/carpetas compartidas* y los archivos de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="8edcf-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="8edcf-216">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="8edcf-216">Create full identity UI source</span></span>

<span data-ttu-id="8edcf-217">Para mantener el control total de la interfaz de usuario de identidad, ejecute el scaffolding de identidad y seleccione **invalidar todos los archivos**.</span><span class="sxs-lookup"><span data-stu-id="8edcf-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="8edcf-218">En el código resaltado siguiente se muestran los cambios para reemplazar la interfaz de usuario de identidad predeterminada por la identidad en una aplicación Web de ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8edcf-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="8edcf-219">Puede que desee hacer esto para tener un control total de la interfaz de usuario de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="8edcf-220">La identidad predeterminada se reemplaza en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8edcf-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="8edcf-221">El código siguiente establece los valores de [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)y [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="8edcf-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="8edcf-222">Registre una implementación de `IEmailSender`, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8edcf-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="8edcf-223">Deshabilitar registro (página)</span><span class="sxs-lookup"><span data-stu-id="8edcf-223">Disable register page</span></span>

<span data-ttu-id="8edcf-224">Para deshabilitar el registro de usuario:</span><span class="sxs-lookup"><span data-stu-id="8edcf-224">To disable user registration:</span></span>

* <span data-ttu-id="8edcf-225">Identidad scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8edcf-225">Scaffold Identity.</span></span> <span data-ttu-id="8edcf-226">Incluya account. Register, Account. login y account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="8edcf-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="8edcf-227">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8edcf-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="8edcf-228">Actualice *areas/Identity/pages/Account/Register. cshtml. CS* para que los usuarios no puedan registrarse desde este punto de conexión:</span><span class="sxs-lookup"><span data-stu-id="8edcf-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="8edcf-229">Actualice *areas/Identity/pages/Account/Register. cshtml* para que sean coherentes con los cambios anteriores:</span><span class="sxs-lookup"><span data-stu-id="8edcf-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="8edcf-230">Comente o quite el vínculo de registro de las *áreas/identidad/páginas/cuenta/inicio de sesión. cshtml.*</span><span class="sxs-lookup"><span data-stu-id="8edcf-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="8edcf-231">Actualice la página *áreas/identidad/páginas/cuenta/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="8edcf-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="8edcf-232">Quite el código y los vínculos del archivo cshtml.</span><span class="sxs-lookup"><span data-stu-id="8edcf-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="8edcf-233">Quite el código de confirmación del `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="8edcf-233">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="8edcf-234">Usar otra aplicación para agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="8edcf-234">Use another app to add users</span></span>

<span data-ttu-id="8edcf-235">Proporcionar un mecanismo para agregar usuarios fuera de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="8edcf-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="8edcf-236">Entre las opciones para agregar usuarios se incluyen:</span><span class="sxs-lookup"><span data-stu-id="8edcf-236">Options to add users include:</span></span>

* <span data-ttu-id="8edcf-237">Una aplicación Web de administración dedicada.</span><span class="sxs-lookup"><span data-stu-id="8edcf-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="8edcf-238">Una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="8edcf-238">A console app.</span></span>

<span data-ttu-id="8edcf-239">En el código siguiente se describe un enfoque para agregar usuarios:</span><span class="sxs-lookup"><span data-stu-id="8edcf-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="8edcf-240">Una lista de usuarios se lee en la memoria.</span><span class="sxs-lookup"><span data-stu-id="8edcf-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="8edcf-241">Se genera una contraseña única segura para cada usuario.</span><span class="sxs-lookup"><span data-stu-id="8edcf-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="8edcf-242">El usuario se agrega a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="8edcf-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="8edcf-243">Se notifica al usuario y se le indica que cambie la contraseña.</span><span class="sxs-lookup"><span data-stu-id="8edcf-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="8edcf-244">En el código siguiente se describe cómo agregar un usuario:</span><span class="sxs-lookup"><span data-stu-id="8edcf-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="8edcf-245">Se puede seguir un enfoque similar para escenarios de producción.</span><span class="sxs-lookup"><span data-stu-id="8edcf-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8edcf-246">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8edcf-246">Additional resources</span></span>

* [<span data-ttu-id="8edcf-247">Cambios en el código de autenticación en ASP.NET Core 2,1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="8edcf-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end