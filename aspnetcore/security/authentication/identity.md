---
title: Introducción a la identidad en ASP.NET Core
author: rick-anderson
description: Usar Identity con una aplicación ASP.NET Core. Obtenga información sobre cómo establecer los requisitos de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 10/15/2019
uid: security/authentication/identity
ms.openlocfilehash: 8da13ca5f74a9c829eb8137d33af0684ff88266d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333549"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="449d1-104">Introducción a la identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="449d1-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="449d1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="449d1-106">ASP.NET Core identidad es un sistema de pertenencia que admite la funcionalidad de inicio de sesión de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-106">ASP.NET Core Identity is a membership system that supports user interface (UI) login functionality.</span></span> <span data-ttu-id="449d1-107">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="449d1-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="449d1-108">Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="449d1-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="449d1-109">La identidad puede configurarse mediante una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="449d1-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="449d1-110">Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="449d1-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="449d1-111">En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-111">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="449d1-112">Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="449d1-112">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="449d1-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([Cómo descargar)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="449d1-113">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="449d1-114">Creación de una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="449d1-114">Create a Web app with authentication</span></span>

<span data-ttu-id="449d1-115">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="449d1-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="449d1-117">Seleccione **archivo** > **nuevo** **proyecto**de >.</span><span class="sxs-lookup"><span data-stu-id="449d1-117">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="449d1-118">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="449d1-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="449d1-119">Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="449d1-119">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="449d1-120">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="449d1-120">Click **OK**.</span></span>
* <span data-ttu-id="449d1-121">Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="449d1-121">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="449d1-122">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="449d1-122">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-123">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="449d1-124">El comando anterior crea una aplicación Web de Razor con SQLite.</span><span class="sxs-lookup"><span data-stu-id="449d1-124">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="449d1-125">Para crear la aplicación web con LocalDB, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="449d1-125">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="449d1-126">El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="449d1-126">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="449d1-127">La biblioteca de clases de Razor de identidad expone puntos de conexión con el área de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="449d1-127">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="449d1-128">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="449d1-128">For example:</span></span>

* <span data-ttu-id="449d1-129">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="449d1-129">/Identity/Account/Login</span></span>
* <span data-ttu-id="449d1-130">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="449d1-130">/Identity/Account/Logout</span></span>
* <span data-ttu-id="449d1-131">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="449d1-131">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="449d1-132">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="449d1-132">Apply migrations</span></span>

<span data-ttu-id="449d1-133">Aplique las migraciones para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="449d1-133">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-134">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="449d1-135">Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):</span><span class="sxs-lookup"><span data-stu-id="449d1-135">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-136">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="449d1-137">No es necesario realizar migraciones en este paso al usar SQLite.</span><span class="sxs-lookup"><span data-stu-id="449d1-137">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="449d1-138">Para LocalDB, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="449d1-138">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="449d1-139">Probar registro e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-139">Test Register and Login</span></span>

<span data-ttu-id="449d1-140">Ejecute la aplicación y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-140">Run the app and register a user.</span></span> <span data-ttu-id="449d1-141">En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .</span><span class="sxs-lookup"><span data-stu-id="449d1-141">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="449d1-142">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-142">Configure Identity services</span></span>

<span data-ttu-id="449d1-143">Los servicios se agregan en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="449d1-143">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="449d1-144">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="449d1-144">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="449d1-145">El código resaltado anterior configura la identidad con valores de opción predeterminados.</span><span class="sxs-lookup"><span data-stu-id="449d1-145">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="449d1-146">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="449d1-146">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="449d1-147">La identidad se habilita llamando a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="449d1-147">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="449d1-148">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="449d1-148">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="449d1-149">La aplicación generada por la plantilla no utiliza la [autorización](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="449d1-149">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="449d1-150">`app.UseAuthorization` se incluye para asegurarse de que se agrega en el orden correcto, en caso de que la aplicación agregue autorización.</span><span class="sxs-lookup"><span data-stu-id="449d1-150">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="449d1-151">se debe llamar a `UseRouting`, `UseAuthentication`, `UseAuthorization` y `UseEndpoints` en el orden mostrado en el código anterior.</span><span class="sxs-lookup"><span data-stu-id="449d1-151">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="449d1-152">Para obtener más información sobre `IdentityOptions` y `Startup`, consulte <xref:Microsoft.AspNetCore.Identity.IdentityOptions> y el inicio de la [aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="449d1-152">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="449d1-153">Registro de scaffolding, Inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-153">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="449d1-155">Agregue los archivos de registro, Inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="449d1-155">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="449d1-156">Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.</span><span class="sxs-lookup"><span data-stu-id="449d1-156">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-157">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-157">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="449d1-158">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="449d1-158">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="449d1-159">De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="449d1-159">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="449d1-160">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="449d1-160">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="449d1-161">Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="449d1-161">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="449d1-162">Para obtener más información sobre la identidad de scaffolding, vea [scaffolding Identity into a Razor Project with Authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="449d1-162">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="449d1-163">Examinar registro</span><span class="sxs-lookup"><span data-stu-id="449d1-163">Examine Register</span></span>

<span data-ttu-id="449d1-164">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción de `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="449d1-165">El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="449d1-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="449d1-166">la inserción de dependencias proporciona `_userManager`:</span><span class="sxs-lookup"><span data-stu-id="449d1-166">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="449d1-167">Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="449d1-168">Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="449d1-168">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="449d1-169">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-169">Log in</span></span>

<span data-ttu-id="449d1-170">El formulario de inicio de sesión se muestra cuando:</span><span class="sxs-lookup"><span data-stu-id="449d1-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="449d1-171">Se selecciona el vínculo **iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="449d1-171">The **Log in** link is selected.</span></span>
* <span data-ttu-id="449d1-172">Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="449d1-172">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="449d1-173">Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="449d1-174">se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="449d1-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="449d1-175">La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="449d1-175">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="449d1-176">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="449d1-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="449d1-177">Para obtener más información, vea <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="449d1-177">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="449d1-178">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-178">Log out</span></span>

<span data-ttu-id="449d1-179">El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="449d1-179">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="449d1-180">En el código anterior, el código `return RedirectToPage();` debe ser un redireccionamiento para que el explorador realice una nueva solicitud y se actualice la identidad del usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-180">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="449d1-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="449d1-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="449d1-182">Post se especifica en *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="449d1-182">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="449d1-183">Identidad de prueba</span><span class="sxs-lookup"><span data-stu-id="449d1-183">Test Identity</span></span>

<span data-ttu-id="449d1-184">Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="449d1-184">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="449d1-185">Para probar la identidad, agregue [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="449d1-185">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="449d1-186">Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** .</span><span class="sxs-lookup"><span data-stu-id="449d1-186">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="449d1-187">Se le redirigirá a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="449d1-187">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="449d1-188">Explorar identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-188">Explore Identity</span></span>

<span data-ttu-id="449d1-189">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="449d1-189">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="449d1-190">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="449d1-190">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="449d1-191">Examine el origen de cada página y recorra el depurador.</span><span class="sxs-lookup"><span data-stu-id="449d1-191">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="449d1-192">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-192">Identity Components</span></span>

<span data-ttu-id="449d1-193">Todos los paquetes de NuGet que dependen de la identidad se incluyen en la [ASP.net Core marco de trabajo compartido](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="449d1-193">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="449d1-194">El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="449d1-194">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="449d1-195">Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="449d1-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="449d1-196">Migrar a ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-196">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="449d1-197">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="449d1-197">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="449d1-198">Establecer la seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="449d1-198">Setting password strength</span></span>

<span data-ttu-id="449d1-199">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.</span><span class="sxs-lookup"><span data-stu-id="449d1-199">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="449d1-200">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="449d1-200">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="449d1-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> se presentó en ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="449d1-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="449d1-202">Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="449d1-202">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="449d1-203">Vea [origen de AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="449d1-203">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="449d1-204">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="449d1-204">Next Steps</span></span>

* [<span data-ttu-id="449d1-205">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="449d1-205">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="449d1-206">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="449d1-206">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="449d1-207">ASP.NET Core identidad es un sistema de pertenencia que agrega funcionalidad de inicio de sesión a ASP.NET Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="449d1-207">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="449d1-208">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="449d1-208">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="449d1-209">Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="449d1-209">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="449d1-210">La identidad puede configurarse mediante una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="449d1-210">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="449d1-211">Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="449d1-211">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="449d1-212">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Cómo descargar)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="449d1-212">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="449d1-213">En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-213">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="449d1-214">Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="449d1-214">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="449d1-215">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="449d1-215">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="449d1-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> se presentó en ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="449d1-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="449d1-217">Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="449d1-217">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="449d1-218">Vea [origen de AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="449d1-218">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="449d1-219">Creación de una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="449d1-219">Create a Web app with authentication</span></span>

<span data-ttu-id="449d1-220">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="449d1-220">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="449d1-222">Seleccione **archivo** > **nuevo** **proyecto**de >.</span><span class="sxs-lookup"><span data-stu-id="449d1-222">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="449d1-223">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="449d1-223">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="449d1-224">Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="449d1-224">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="449d1-225">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="449d1-225">Click **OK**.</span></span>
* <span data-ttu-id="449d1-226">Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="449d1-226">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="449d1-227">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="449d1-227">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-228">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-228">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="449d1-229">El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="449d1-229">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="449d1-230">La biblioteca de clases de Razor de identidad expone puntos de conexión con el área de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="449d1-230">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="449d1-231">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="449d1-231">For example:</span></span>

* <span data-ttu-id="449d1-232">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="449d1-232">/Identity/Account/Login</span></span>
* <span data-ttu-id="449d1-233">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="449d1-233">/Identity/Account/Logout</span></span>
* <span data-ttu-id="449d1-234">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="449d1-234">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="449d1-235">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="449d1-235">Apply migrations</span></span>

<span data-ttu-id="449d1-236">Aplique las migraciones para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="449d1-236">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-237">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-237">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="449d1-238">Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):</span><span class="sxs-lookup"><span data-stu-id="449d1-238">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-239">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-239">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="449d1-240">Probar registro e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-240">Test Register and Login</span></span>

<span data-ttu-id="449d1-241">Ejecute la aplicación y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="449d1-241">Run the app and register a user.</span></span> <span data-ttu-id="449d1-242">En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .</span><span class="sxs-lookup"><span data-stu-id="449d1-242">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="449d1-243">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-243">Configure Identity services</span></span>

<span data-ttu-id="449d1-244">Los servicios se agregan en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="449d1-244">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="449d1-245">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="449d1-245">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="449d1-246">El código anterior configura la identidad con valores de opción predeterminados.</span><span class="sxs-lookup"><span data-stu-id="449d1-246">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="449d1-247">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="449d1-247">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="449d1-248">La identidad se habilita llamando a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="449d1-248">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="449d1-249">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="449d1-249">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="449d1-250">Para obtener más información, vea la [clase IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y el inicio de la [aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="449d1-250">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="449d1-251">Registro de scaffolding, Inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-251">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="449d1-252">Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.</span><span class="sxs-lookup"><span data-stu-id="449d1-252">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="449d1-253">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449d1-253">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="449d1-254">Agregue los archivos de registro, Inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="449d1-254">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="449d1-255">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="449d1-255">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="449d1-256">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="449d1-256">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="449d1-257">De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="449d1-257">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="449d1-258">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="449d1-258">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="449d1-259">Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="449d1-259">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="449d1-260">Examinar registro</span><span class="sxs-lookup"><span data-stu-id="449d1-260">Examine Register</span></span>

<span data-ttu-id="449d1-261">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción de `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-261">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="449d1-262">El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="449d1-262">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="449d1-263">la inserción de dependencias proporciona `_userManager`:</span><span class="sxs-lookup"><span data-stu-id="449d1-263">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="449d1-264">Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-264">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="449d1-265">**Nota:** Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="449d1-265">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="449d1-266">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-266">Log in</span></span>

<span data-ttu-id="449d1-267">El formulario de inicio de sesión se muestra cuando:</span><span class="sxs-lookup"><span data-stu-id="449d1-267">The Login form is displayed when:</span></span>

* <span data-ttu-id="449d1-268">Se selecciona el vínculo **iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="449d1-268">The **Log in** link is selected.</span></span>
* <span data-ttu-id="449d1-269">Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="449d1-269">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="449d1-270">Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="449d1-270">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="449d1-271">se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="449d1-271">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="449d1-272">La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="449d1-272">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="449d1-273">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="449d1-273">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="449d1-274">Para obtener más información, vea <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="449d1-274">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="449d1-275">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="449d1-275">Log out</span></span>

<span data-ttu-id="449d1-276">El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="449d1-276">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="449d1-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="449d1-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="449d1-278">Post se especifica en *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="449d1-278">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="449d1-279">Identidad de prueba</span><span class="sxs-lookup"><span data-stu-id="449d1-279">Test Identity</span></span>

<span data-ttu-id="449d1-280">Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="449d1-280">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="449d1-281">Para probar la identidad, agregue [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página de privacidad.</span><span class="sxs-lookup"><span data-stu-id="449d1-281">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="449d1-282">Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** .</span><span class="sxs-lookup"><span data-stu-id="449d1-282">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="449d1-283">Se le redirigirá a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="449d1-283">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="449d1-284">Explorar identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-284">Explore Identity</span></span>

<span data-ttu-id="449d1-285">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="449d1-285">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="449d1-286">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="449d1-286">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="449d1-287">Examine el origen de cada página y recorra el depurador.</span><span class="sxs-lookup"><span data-stu-id="449d1-287">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="449d1-288">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-288">Identity Components</span></span>

<span data-ttu-id="449d1-289">Todos los paquetes de NuGet que dependen de la identidad se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="449d1-289">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="449d1-290">El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="449d1-290">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="449d1-291">Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="449d1-291">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="449d1-292">Migrar a ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="449d1-292">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="449d1-293">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="449d1-293">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="449d1-294">Establecer la seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="449d1-294">Setting password strength</span></span>

<span data-ttu-id="449d1-295">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.</span><span class="sxs-lookup"><span data-stu-id="449d1-295">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="449d1-296">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="449d1-296">Next Steps</span></span>

* [<span data-ttu-id="449d1-297">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="449d1-297">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
