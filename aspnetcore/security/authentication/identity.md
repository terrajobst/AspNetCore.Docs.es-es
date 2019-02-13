---
title: Introducción a la identidad en ASP.NET Core
author: rick-anderson
description: Usar identidad con una aplicación ASP.NET Core. Obtenga información sobre cómo establecer los requisitos de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars etc.).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 1a4e7fb3ac6a767ca17127dd58a9b9e65ed9a00b
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "55739688"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="7272a-104">Introducción a la identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7272a-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="7272a-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7272a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7272a-106">ASP.NET Core Identity es un sistema de pertenencia que agrega la funcionalidad de inicio de sesión a las aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7272a-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="7272a-107">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="7272a-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="7272a-108">Proveedores de inicio de sesión externos compatibles incluyen [Facebook, Google, Twitter y Microsoft Account](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7272a-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7272a-109">Identidad puede configurarse con una base de datos de SQL Server para almacenar los nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="7272a-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="7272a-110">Como alternativa, otro almacén persistente puede usarse, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="7272a-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="7272a-111">[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([descarga)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7272a-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="7272a-112">En este tema, aprenda a usar identidad para registrar, inicie sesión y cerrar la sesión un usuario.</span><span class="sxs-lookup"><span data-stu-id="7272a-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="7272a-113">Para obtener instrucciones detalladas sobre cómo crear aplicaciones que usan la identidad, vea la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="7272a-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="7272a-114">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="7272a-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="7272a-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) se introdujo en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="7272a-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="7272a-116">Una llamada a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7272a-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="7272a-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="7272a-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="7272a-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="7272a-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="7272a-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="7272a-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="7272a-120">Consulte [AddDefaultIdentity origen](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7272a-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="7272a-121">Creación de una aplicación Web con autenticación</span><span class="sxs-lookup"><span data-stu-id="7272a-121">Create a Web app with authentication</span></span>

<span data-ttu-id="7272a-122">Cree un proyecto de aplicación Web ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="7272a-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7272a-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7272a-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="7272a-124">Seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7272a-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="7272a-125">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7272a-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="7272a-126">Denomine el proyecto **WebApp1** tener el mismo espacio de nombres como la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7272a-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="7272a-127">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7272a-127">Click **OK**.</span></span>
* <span data-ttu-id="7272a-128">Seleccione una de ASP.NET Core **aplicación Web**, a continuación, seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="7272a-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="7272a-129">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7272a-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7272a-130">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="7272a-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="7272a-131">Proporciona el proyecto generado [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="7272a-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="7272a-132">La biblioteca de clases de identidad Razor expone los puntos de conexión con el `Identity` área.</span><span class="sxs-lookup"><span data-stu-id="7272a-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="7272a-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7272a-133">For example:</span></span>

* <span data-ttu-id="7272a-134">/ Identidad o cuenta/inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="7272a-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="7272a-135">/ Identidad o cuenta/cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="7272a-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="7272a-136">/ Identidad o cuenta/administrar</span><span class="sxs-lookup"><span data-stu-id="7272a-136">/Identity/Account/Manage</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="7272a-137">Inicio de sesión y de registro de prueba</span><span class="sxs-lookup"><span data-stu-id="7272a-137">Test Register and Login</span></span>

<span data-ttu-id="7272a-138">Ejecute la aplicación y registrar un usuario.</span><span class="sxs-lookup"><span data-stu-id="7272a-138">Run the app and register a user.</span></span> <span data-ttu-id="7272a-139">Según el tamaño de pantalla, es posible que deba seleccionar el botón de alternancia de navegación para ver el **registrar** y **inicio de sesión** vínculos.</span><span class="sxs-lookup"><span data-stu-id="7272a-139">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="7272a-140">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="7272a-140">Configure Identity services</span></span>

<span data-ttu-id="7272a-141">Se agregan los servicios en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7272a-141">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="7272a-142">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="7272a-142">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="7272a-143">El código anterior configura la identidad con valores de la opción predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7272a-143">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="7272a-144">Los servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7272a-144">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="7272a-145">Se habilita la identidad mediante una llamada a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="7272a-145">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="7272a-146">`UseAuthentication` Agrega autenticación [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7272a-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="7272a-147">Los servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7272a-147">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="7272a-148">Se habilita la identidad de la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="7272a-148">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="7272a-149">`UseAuthentication` Agrega autenticación [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7272a-149">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="7272a-150">Estos servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7272a-150">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="7272a-151">Se habilita la identidad de la aplicación mediante una llamada a `UseIdentity` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="7272a-151">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="7272a-152">`UseIdentity` Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="7272a-152">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="7272a-153">Para obtener más información, consulte el [IdentityOptions clase](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y [inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="7272a-153">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="7272a-154">Aplicar la técnica scaffolding registrar, inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="7272a-154">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="7272a-155">Siga el [aplicar la técnica scaffolding identidad en un proyecto de Razor sin autorización](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones para generar el código mostrado en esta sección.</span><span class="sxs-lookup"><span data-stu-id="7272a-155">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7272a-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7272a-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7272a-157">Agregue los archivos de registro, inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="7272a-157">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7272a-158">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="7272a-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="7272a-159">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="7272a-159">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="7272a-160">En caso contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="7272a-160">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="7272a-161">PowerShell usa el punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="7272a-161">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="7272a-162">Cuando se usa PowerShell, el punto y coma en la lista de archivos de escape o coloque la lista de archivos en las comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="7272a-162">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="7272a-163">Examine el registro</span><span class="sxs-lookup"><span data-stu-id="7272a-163">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="7272a-164">Cuando un usuario hace clic en el **registrar** vínculo, el `RegisterModel.OnPostAsync` se invoca la acción.</span><span class="sxs-lookup"><span data-stu-id="7272a-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="7272a-165">El usuario se crea por [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el `_userManager` objeto.</span><span class="sxs-lookup"><span data-stu-id="7272a-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="7272a-166">`_userManager` se proporciona mediante la inserción de dependencias):</span><span class="sxs-lookup"><span data-stu-id="7272a-166">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="7272a-167">Cuando un usuario hace clic en el **registrar** vínculo, el `Register` acción se invoca en `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="7272a-167">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="7272a-168">El `Register` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a `AccountController` mediante la inserción de dependencias):</span><span class="sxs-lookup"><span data-stu-id="7272a-168">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="7272a-169">Si el usuario se creó correctamente, el usuario se registra en la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="7272a-169">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="7272a-170">**Nota:** Consulte [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar que el inicio de sesión de inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="7272a-170">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="7272a-171">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="7272a-171">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7272a-172">Se muestra el formulario de inicio de sesión cuando:</span><span class="sxs-lookup"><span data-stu-id="7272a-172">The Login form is displayed when:</span></span>

* <span data-ttu-id="7272a-173">El **iniciarla** vínculo está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="7272a-173">The **Log in** link is selected.</span></span>
* <span data-ttu-id="7272a-174">Un usuario intenta tener acceso a una página restringida que no están autorizados para tener acceso a **o** cuando aún no se ha autenticado por el sistema.</span><span class="sxs-lookup"><span data-stu-id="7272a-174">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="7272a-175">Cuando se envía el formulario de la página de inicio de sesión, el `OnPostAsync` se llama a la acción.</span><span class="sxs-lookup"><span data-stu-id="7272a-175">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="7272a-176">`PasswordSignInAsync` se llama en el `_signInManager` objeto (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="7272a-176">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="7272a-177">La base de `Controller` clase expone un `User` propiedad que se puede acceder desde los métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="7272a-177">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="7272a-178">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="7272a-178">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="7272a-179">Para obtener más información, consulta <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="7272a-179">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7272a-180">Se muestra el formulario de inicio de sesión cuando los usuarios seleccionan el **iniciarla** vincular o se le redirigirá al tener acceso a una página que requiere autenticación.</span><span class="sxs-lookup"><span data-stu-id="7272a-180">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="7272a-181">Cuando el usuario envía el formulario en la página de inicio de sesión, el `AccountController` `Login` se llama a la acción.</span><span class="sxs-lookup"><span data-stu-id="7272a-181">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="7272a-182">El `Login` acción llamadas `PasswordSignInAsync` en el `_signInManager` objeto (proporcionado a `AccountController` mediante la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="7272a-182">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="7272a-183">La base de (`Controller` o `PageModel`) clase expone un `User` propiedad.</span><span class="sxs-lookup"><span data-stu-id="7272a-183">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="7272a-184">Por ejemplo, `User.Claims` pueden enumerarse para tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="7272a-184">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="7272a-185">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="7272a-185">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7272a-186">El **cerrar sesión** vínculo invoca el `LogoutModel.OnPost` acción.</span><span class="sxs-lookup"><span data-stu-id="7272a-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="7272a-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones de usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="7272a-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="7272a-188">No redirigir después de llamar a `SignOutAsync` o que el usuario **no** cerrar la sesión.</span><span class="sxs-lookup"><span data-stu-id="7272a-188">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="7272a-189">POST se especifica en el *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7272a-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="7272a-190">Al hacer clic en el **cerrar sesión** llamadas de vincular el `LogOut` acción.</span><span class="sxs-lookup"><span data-stu-id="7272a-190">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="7272a-191">El código anterior llama el `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="7272a-191">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="7272a-192">El `SignOutAsync` método borra las notificaciones de usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="7272a-192">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="7272a-193">Comprobar la identidad</span><span class="sxs-lookup"><span data-stu-id="7272a-193">Test Identity</span></span>

<span data-ttu-id="7272a-194">Las plantillas de proyecto de web predeterminada permiten accesos anónimos a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="7272a-194">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="7272a-195">Para probar la identidad, agregue [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página de privacidad.</span><span class="sxs-lookup"><span data-stu-id="7272a-195">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="7272a-196">Si ha iniciado sesión, cerrar sesión. Ejecute la aplicación y seleccione el **privacidad** vínculo.</span><span class="sxs-lookup"><span data-stu-id="7272a-196">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="7272a-197">Se le redirigirá a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="7272a-197">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="7272a-198">Explore la identidad</span><span class="sxs-lookup"><span data-stu-id="7272a-198">Explore Identity</span></span>

<span data-ttu-id="7272a-199">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="7272a-199">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="7272a-200">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="7272a-200">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="7272a-201">Examine el origen de cada página y paso a través del depurador.</span><span class="sxs-lookup"><span data-stu-id="7272a-201">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="7272a-202">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="7272a-202">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7272a-203">Todas la identidad dependientes paquetes de NuGet se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7272a-203">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="7272a-204">El paquete principal de identidad está [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="7272a-204">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="7272a-205">Este paquete contiene el conjunto principal de interfaces de ASP.NET Core Identity y se incluye de forma `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="7272a-205">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="7272a-206">Migración a ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="7272a-206">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="7272a-207">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, consulte [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="7272a-207">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="7272a-208">Configuración de seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="7272a-208">Setting password strength</span></span>

<span data-ttu-id="7272a-209">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos de contraseña mínima.</span><span class="sxs-lookup"><span data-stu-id="7272a-209">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7272a-210">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7272a-210">Next Steps</span></span>

* [<span data-ttu-id="7272a-211">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="7272a-211">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
