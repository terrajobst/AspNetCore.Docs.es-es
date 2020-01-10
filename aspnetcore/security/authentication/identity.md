---
title: Introducción a la identidad en ASP.NET Core
author: rick-anderson
description: Usar Identity con una aplicación ASP.NET Core. Obtenga información sobre cómo establecer los requisitos de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/identity
ms.openlocfilehash: d4cda43c417a0d8e1b78f8b59b7050debf017269
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828235"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="adfe0-104">Introducción a la identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="adfe0-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="adfe0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="adfe0-106">Identidad de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="adfe0-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="adfe0-107">Es una API que admite la funcionalidad de inicio de sesión de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="adfe0-108">Administra usuarios, contraseñas, datos de perfil, roles, notificaciones, tokens, confirmación de correo electrónico, etc.</span><span class="sxs-lookup"><span data-stu-id="adfe0-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="adfe0-109">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="adfe0-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="adfe0-110">Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="adfe0-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="adfe0-111">El [código fuente de identidad](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) está disponible en github.</span><span class="sxs-lookup"><span data-stu-id="adfe0-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="adfe0-112">La [identidad scaffolding](xref:security/authentication/scaffold-identity) y ver los archivos generados para revisar la interacción de la plantilla con la identidad.</span><span class="sxs-lookup"><span data-stu-id="adfe0-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="adfe0-113">La identidad se configura normalmente con una base de datos SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="adfe0-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="adfe0-114">Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="adfe0-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="adfe0-115">En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="adfe0-116">Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="adfe0-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="adfe0-117">La [plataforma de identidad de Microsoft](/azure/active-directory/develop/) es:</span><span class="sxs-lookup"><span data-stu-id="adfe0-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="adfe0-118">Evolución de la plataforma de desarrollo de Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="adfe0-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="adfe0-119">No relacionado con ASP.NET Core identidad.</span><span class="sxs-lookup"><span data-stu-id="adfe0-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="adfe0-120">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([Cómo descargar)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="adfe0-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="adfe0-121">Creación de una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="adfe0-121">Create a Web app with authentication</span></span>

<span data-ttu-id="adfe0-122">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="adfe0-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adfe0-124">Seleccione **archivo** > **nuevo** **proyecto**de >.</span><span class="sxs-lookup"><span data-stu-id="adfe0-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="adfe0-125">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="adfe0-126">Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="adfe0-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="adfe0-127">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-127">Click **OK**.</span></span>
* <span data-ttu-id="adfe0-128">Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="adfe0-129">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-130">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="adfe0-131">El comando anterior crea una aplicación Web de Razor con SQLite.</span><span class="sxs-lookup"><span data-stu-id="adfe0-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="adfe0-132">Para crear la aplicación web con LocalDB, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="adfe0-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="adfe0-133">El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="adfe0-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="adfe0-134">La biblioteca de clases de Razor de identidad expone puntos de conexión con el área de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="adfe0-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="adfe0-135">For example:</span></span>

* <span data-ttu-id="adfe0-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="adfe0-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="adfe0-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="adfe0-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="adfe0-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="adfe0-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="adfe0-139">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="adfe0-139">Apply migrations</span></span>

<span data-ttu-id="adfe0-140">Aplique las migraciones para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adfe0-142">Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):</span><span class="sxs-lookup"><span data-stu-id="adfe0-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-143">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adfe0-144">No es necesario realizar migraciones en este paso al usar SQLite.</span><span class="sxs-lookup"><span data-stu-id="adfe0-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="adfe0-145">Para LocalDB, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="adfe0-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="adfe0-146">Probar registro e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-146">Test Register and Login</span></span>

<span data-ttu-id="adfe0-147">Ejecute la aplicación y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-147">Run the app and register a user.</span></span> <span data-ttu-id="adfe0-148">En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="adfe0-149">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-149">Configure Identity services</span></span>

<span data-ttu-id="adfe0-150">Los servicios se agregan en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="adfe0-151">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="adfe0-152">El código resaltado anterior configura la identidad con valores de opción predeterminados.</span><span class="sxs-lookup"><span data-stu-id="adfe0-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="adfe0-153">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="adfe0-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="adfe0-154">La identidad se habilita llamando a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="adfe0-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="adfe0-155">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="adfe0-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="adfe0-156">La aplicación generada por la plantilla no utiliza la [autorización](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="adfe0-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="adfe0-157">`app.UseAuthorization` se incluye para asegurarse de que se agrega en el orden correcto, en caso de que la aplicación agregue autorización.</span><span class="sxs-lookup"><span data-stu-id="adfe0-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="adfe0-158">se debe llamar a `UseRouting`, `UseAuthentication`, `UseAuthorization`y `UseEndpoints` en el orden mostrado en el código anterior.</span><span class="sxs-lookup"><span data-stu-id="adfe0-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="adfe0-159">Para obtener más información sobre `IdentityOptions` y `Startup`, consulte <xref:Microsoft.AspNetCore.Identity.IdentityOptions> y el inicio de la [aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="adfe0-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="adfe0-160">Registro de scaffolding, Inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adfe0-162">Agregue los archivos de registro, Inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="adfe0-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="adfe0-163">Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.</span><span class="sxs-lookup"><span data-stu-id="adfe0-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-164">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adfe0-165">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="adfe0-166">De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="adfe0-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="adfe0-167">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="adfe0-168">Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="adfe0-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="adfe0-169">Para obtener más información sobre la identidad de scaffolding, vea [scaffolding Identity into a Razor Project with Authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="adfe0-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="adfe0-170">Examinar registro</span><span class="sxs-lookup"><span data-stu-id="adfe0-170">Examine Register</span></span>

<span data-ttu-id="adfe0-171">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción de `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="adfe0-172">El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="adfe0-173">la inserción de dependencias proporciona `_userManager`:</span><span class="sxs-lookup"><span data-stu-id="adfe0-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="adfe0-174">Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="adfe0-175">Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="adfe0-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="adfe0-176">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-176">Log in</span></span>

<span data-ttu-id="adfe0-177">El formulario de inicio de sesión se muestra cuando:</span><span class="sxs-lookup"><span data-stu-id="adfe0-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="adfe0-178">Se selecciona el vínculo **iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="adfe0-179">Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="adfe0-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="adfe0-180">Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="adfe0-181">se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="adfe0-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="adfe0-182">La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="adfe0-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="adfe0-183">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="adfe0-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="adfe0-184">Para obtener más información, vea <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="adfe0-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="adfe0-185">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-185">Log out</span></span>

<span data-ttu-id="adfe0-186">El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="adfe0-187">En el código anterior, el código `return RedirectToPage();` debe ser un redireccionamiento para que el explorador realice una nueva solicitud y se actualice la identidad del usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="adfe0-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="adfe0-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="adfe0-189">Post se especifica en *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="adfe0-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="adfe0-190">Identidad de prueba</span><span class="sxs-lookup"><span data-stu-id="adfe0-190">Test Identity</span></span>

<span data-ttu-id="adfe0-191">Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="adfe0-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="adfe0-192">Para probar la identidad, agregue [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="adfe0-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="adfe0-193">Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="adfe0-194">Se le redirige a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="adfe0-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="adfe0-195">Explorar identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-195">Explore Identity</span></span>

<span data-ttu-id="adfe0-196">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="adfe0-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="adfe0-197">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="adfe0-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="adfe0-198">Examine el origen de cada página y recorra el depurador.</span><span class="sxs-lookup"><span data-stu-id="adfe0-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="adfe0-199">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-199">Identity Components</span></span>

<span data-ttu-id="adfe0-200">Todos los paquetes de NuGet que dependen de la identidad se incluyen en la [ASP.net Core marco de trabajo compartido](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="adfe0-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="adfe0-201">El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="adfe0-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="adfe0-202">Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="adfe0-203">Migrar a ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="adfe0-204">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="adfe0-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="adfe0-205">Establecer la seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="adfe0-205">Setting password strength</span></span>

<span data-ttu-id="adfe0-206">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.</span><span class="sxs-lookup"><span data-stu-id="adfe0-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="adfe0-207">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="adfe0-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="adfe0-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> se presentó en ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="adfe0-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="adfe0-209">Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="adfe0-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="adfe0-210">Vea [origen de AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="adfe0-210">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adfe0-211">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="adfe0-211">Next Steps</span></span>

* <span data-ttu-id="adfe0-212">Consulte [este problema de github](https://github.com/aspnet/AspNetCore.Docs/issues/5131) para obtener información sobre cómo configurar la identidad mediante SQLite.</span><span class="sxs-lookup"><span data-stu-id="adfe0-212">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="adfe0-213">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="adfe0-213">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="adfe0-214">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="adfe0-214">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="adfe0-215">ASP.NET Core identidad es un sistema de pertenencia que agrega funcionalidad de inicio de sesión a ASP.NET Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="adfe0-215">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="adfe0-216">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="adfe0-216">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="adfe0-217">Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="adfe0-217">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="adfe0-218">La identidad puede configurarse mediante una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="adfe0-218">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="adfe0-219">Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="adfe0-219">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="adfe0-220">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Cómo descargar)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="adfe0-220">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="adfe0-221">En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-221">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="adfe0-222">Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="adfe0-222">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="adfe0-223">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="adfe0-223">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="adfe0-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> se presentó en ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="adfe0-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="adfe0-225">Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="adfe0-225">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="adfe0-226">Vea [origen de AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="adfe0-226">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="adfe0-227">Creación de una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="adfe0-227">Create a Web app with authentication</span></span>

<span data-ttu-id="adfe0-228">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="adfe0-228">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="adfe0-230">Seleccione **archivo** > **nuevo** **proyecto**de >.</span><span class="sxs-lookup"><span data-stu-id="adfe0-230">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="adfe0-231">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-231">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="adfe0-232">Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="adfe0-232">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="adfe0-233">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-233">Click **OK**.</span></span>
* <span data-ttu-id="adfe0-234">Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-234">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="adfe0-235">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="adfe0-235">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-236">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-236">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="adfe0-237">El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="adfe0-237">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="adfe0-238">La biblioteca de clases de Razor de identidad expone puntos de conexión con el área de `Identity`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-238">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="adfe0-239">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="adfe0-239">For example:</span></span>

* <span data-ttu-id="adfe0-240">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="adfe0-240">/Identity/Account/Login</span></span>
* <span data-ttu-id="adfe0-241">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="adfe0-241">/Identity/Account/Logout</span></span>
* <span data-ttu-id="adfe0-242">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="adfe0-242">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="adfe0-243">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="adfe0-243">Apply migrations</span></span>

<span data-ttu-id="adfe0-244">Aplique las migraciones para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-244">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adfe0-246">Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):</span><span class="sxs-lookup"><span data-stu-id="adfe0-246">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-247">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-247">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="adfe0-248">Probar registro e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-248">Test Register and Login</span></span>

<span data-ttu-id="adfe0-249">Ejecute la aplicación y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="adfe0-249">Run the app and register a user.</span></span> <span data-ttu-id="adfe0-250">En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-250">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="adfe0-251">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-251">Configure Identity services</span></span>

<span data-ttu-id="adfe0-252">Los servicios se agregan en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-252">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="adfe0-253">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-253">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="adfe0-254">El código anterior configura la identidad con valores de opción predeterminados.</span><span class="sxs-lookup"><span data-stu-id="adfe0-254">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="adfe0-255">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="adfe0-255">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="adfe0-256">La identidad se habilita llamando a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="adfe0-256">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="adfe0-257">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="adfe0-257">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="adfe0-258">Para obtener más información, vea la [clase IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y el inicio de la [aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="adfe0-258">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="adfe0-259">Registro de scaffolding, Inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-259">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="adfe0-260">Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.</span><span class="sxs-lookup"><span data-stu-id="adfe0-260">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="adfe0-261">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="adfe0-261">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="adfe0-262">Agregue los archivos de registro, Inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="adfe0-262">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="adfe0-263">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="adfe0-263">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="adfe0-264">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-264">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="adfe0-265">De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="adfe0-265">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="adfe0-266">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="adfe0-266">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="adfe0-267">Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="adfe0-267">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="adfe0-268">Examinar registro</span><span class="sxs-lookup"><span data-stu-id="adfe0-268">Examine Register</span></span>

<span data-ttu-id="adfe0-269">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción de `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-269">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="adfe0-270">El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-270">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="adfe0-271">la inserción de dependencias proporciona `_userManager`:</span><span class="sxs-lookup"><span data-stu-id="adfe0-271">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="adfe0-272">Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-272">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="adfe0-273">**Nota:** Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="adfe0-273">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="adfe0-274">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-274">Log in</span></span>

<span data-ttu-id="adfe0-275">El formulario de inicio de sesión se muestra cuando:</span><span class="sxs-lookup"><span data-stu-id="adfe0-275">The Login form is displayed when:</span></span>

* <span data-ttu-id="adfe0-276">Se selecciona el vínculo **iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-276">The **Log in** link is selected.</span></span>
* <span data-ttu-id="adfe0-277">Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="adfe0-277">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="adfe0-278">Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-278">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="adfe0-279">se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="adfe0-279">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="adfe0-280">La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="adfe0-280">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="adfe0-281">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="adfe0-281">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="adfe0-282">Para obtener más información, vea <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="adfe0-282">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="adfe0-283">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="adfe0-283">Log out</span></span>

<span data-ttu-id="adfe0-284">El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-284">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="adfe0-285">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="adfe0-285">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="adfe0-286">Post se especifica en *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="adfe0-286">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="adfe0-287">Identidad de prueba</span><span class="sxs-lookup"><span data-stu-id="adfe0-287">Test Identity</span></span>

<span data-ttu-id="adfe0-288">Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="adfe0-288">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="adfe0-289">Para probar la identidad, agregue [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página de privacidad.</span><span class="sxs-lookup"><span data-stu-id="adfe0-289">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="adfe0-290">Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** .</span><span class="sxs-lookup"><span data-stu-id="adfe0-290">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="adfe0-291">Se le redirige a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="adfe0-291">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="adfe0-292">Explorar identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-292">Explore Identity</span></span>

<span data-ttu-id="adfe0-293">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="adfe0-293">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="adfe0-294">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="adfe0-294">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="adfe0-295">Examine el origen de cada página y recorra el depurador.</span><span class="sxs-lookup"><span data-stu-id="adfe0-295">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="adfe0-296">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-296">Identity Components</span></span>

<span data-ttu-id="adfe0-297">Todos los paquetes de NuGet que dependen de la identidad se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="adfe0-297">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="adfe0-298">El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="adfe0-298">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="adfe0-299">Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="adfe0-299">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="adfe0-300">Migrar a ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="adfe0-300">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="adfe0-301">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="adfe0-301">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="adfe0-302">Establecer la seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="adfe0-302">Setting password strength</span></span>

<span data-ttu-id="adfe0-303">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.</span><span class="sxs-lookup"><span data-stu-id="adfe0-303">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="adfe0-304">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="adfe0-304">Next Steps</span></span>

* <span data-ttu-id="adfe0-305">Consulte [este problema de github](https://github.com/aspnet/AspNetCore.Docs/issues/5131) para obtener información sobre cómo configurar la identidad mediante SQLite.</span><span class="sxs-lookup"><span data-stu-id="adfe0-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="adfe0-306">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="adfe0-306">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
