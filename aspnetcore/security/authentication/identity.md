---
title: Introducción a la identidad en ASP.NET Core
author: rick-anderson
description: Usar Identity con una aplicación ASP.NET Core. Obtenga información sobre cómo establecer los requisitos de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars, etc.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 6701eb0ac1b1abb8699a5a529bcc29f295e5c7c9
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981908"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8f12f-104">Introducción a la identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f12f-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="8f12f-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f12f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8f12f-106">ASP.NET Core identidad es un sistema de pertenencia que agrega funcionalidad de inicio de sesión a ASP.NET Core aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="8f12f-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="8f12f-107">Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo.</span><span class="sxs-lookup"><span data-stu-id="8f12f-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="8f12f-108">Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8f12f-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="8f12f-109">La identidad puede configurarse mediante una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="8f12f-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8f12f-110">Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="8f12f-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="8f12f-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Cómo descargar)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8f12f-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="8f12f-112">En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.</span><span class="sxs-lookup"><span data-stu-id="8f12f-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="8f12f-113">Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="8f12f-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="8f12f-114">AddDefaultIdentity y AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8f12f-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="8f12f-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) se presentó en ASP.net Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8f12f-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8f12f-116">Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f12f-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="8f12f-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8f12f-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="8f12f-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="8f12f-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="8f12f-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="8f12f-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="8f12f-120">Vea [origen de AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8f12f-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="8f12f-121">Creación de una aplicación web con autenticación</span><span class="sxs-lookup"><span data-stu-id="8f12f-121">Create a Web app with authentication</span></span>

<span data-ttu-id="8f12f-122">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="8f12f-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8f12f-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f12f-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8f12f-124">Seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8f12f-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="8f12f-125">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8f12f-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8f12f-126">Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8f12f-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="8f12f-127">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8f12f-127">Click **OK**.</span></span>
* <span data-ttu-id="8f12f-128">Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="8f12f-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="8f12f-129">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8f12f-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8f12f-130">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8f12f-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="8f12f-131">El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8f12f-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8f12f-132">La biblioteca de clases de Razor de identidad expone puntos de conexión con el área @no__t 0.</span><span class="sxs-lookup"><span data-stu-id="8f12f-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="8f12f-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8f12f-133">For example:</span></span>

* <span data-ttu-id="8f12f-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="8f12f-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="8f12f-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="8f12f-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="8f12f-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="8f12f-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="8f12f-137">Aplicación de migraciones</span><span class="sxs-lookup"><span data-stu-id="8f12f-137">Apply migrations</span></span>

<span data-ttu-id="8f12f-138">Aplique las migraciones para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8f12f-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8f12f-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f12f-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8f12f-140">Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):</span><span class="sxs-lookup"><span data-stu-id="8f12f-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8f12f-141">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8f12f-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="8f12f-142">Probar registro e inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="8f12f-142">Test Register and Login</span></span>

<span data-ttu-id="8f12f-143">Ejecute la aplicación y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="8f12f-143">Run the app and register a user.</span></span> <span data-ttu-id="8f12f-144">En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .</span><span class="sxs-lookup"><span data-stu-id="8f12f-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="8f12f-145">Configurar servicios de identidad</span><span class="sxs-lookup"><span data-stu-id="8f12f-145">Configure Identity services</span></span>

<span data-ttu-id="8f12f-146">Los servicios se agregan en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="8f12f-147">El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="8f12f-148">El código anterior configura la identidad con valores de opción predeterminados.</span><span class="sxs-lookup"><span data-stu-id="8f12f-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="8f12f-149">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8f12f-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8f12f-150">La identidad se habilita llamando a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8f12f-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="8f12f-151">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="8f12f-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="8f12f-152">Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8f12f-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8f12f-153">La identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="8f12f-154">`UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="8f12f-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="8f12f-155">Estos servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8f12f-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8f12f-156">La identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="8f12f-157">`UseIdentity` agrega [middleware](xref:fundamentals/middleware/index) de autenticación basada en cookies a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="8f12f-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="8f12f-158">Para obtener más información, vea la [clase IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y el inicio de la [aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="8f12f-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="8f12f-159">Registro de scaffolding, Inicio de sesión y cierre de sesión</span><span class="sxs-lookup"><span data-stu-id="8f12f-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="8f12f-160">Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.</span><span class="sxs-lookup"><span data-stu-id="8f12f-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8f12f-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f12f-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8f12f-162">Agregue los archivos de registro, Inicio de sesión y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="8f12f-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8f12f-163">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="8f12f-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8f12f-164">Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="8f12f-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="8f12f-165">De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="8f12f-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="8f12f-166">PowerShell usa punto y coma como separador de comandos.</span><span class="sxs-lookup"><span data-stu-id="8f12f-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="8f12f-167">Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="8f12f-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="8f12f-168">Examinar registro</span><span class="sxs-lookup"><span data-stu-id="8f12f-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="8f12f-169">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="8f12f-170">El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="8f12f-171">la inserción de dependencias proporciona `_userManager`):</span><span class="sxs-lookup"><span data-stu-id="8f12f-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="8f12f-172">Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción `Register` en `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="8f12f-173">La acción `Register` crea el usuario llamando a `CreateAsync` en el objeto `_userManager` (proporcionado a `AccountController` mediante la inserción de dependencias):</span><span class="sxs-lookup"><span data-stu-id="8f12f-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="8f12f-174">Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="8f12f-175">**Nota:** Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="8f12f-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="8f12f-176">Iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="8f12f-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8f12f-177">El formulario de inicio de sesión se muestra cuando:</span><span class="sxs-lookup"><span data-stu-id="8f12f-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="8f12f-178">Se selecciona el vínculo **iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="8f12f-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="8f12f-179">Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.</span><span class="sxs-lookup"><span data-stu-id="8f12f-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="8f12f-180">Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="8f12f-181">se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="8f12f-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="8f12f-182">La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="8f12f-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="8f12f-183">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="8f12f-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="8f12f-184">Para obtener más información, consulta <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="8f12f-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8f12f-185">El formulario de inicio de sesión se muestra cuando los usuarios seleccionan el vínculo **iniciar sesión** o se redirigen al acceder a una página que requiere autenticación.</span><span class="sxs-lookup"><span data-stu-id="8f12f-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="8f12f-186">Cuando el usuario envía el formulario en la página de inicio de sesión, se llama a la acción `AccountController` `Login`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="8f12f-187">La acción `Login` llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado a `AccountController` mediante la inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="8f12f-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="8f12f-188">La clase base (`Controller` o `PageModel`) expone una propiedad `User`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="8f12f-189">Por ejemplo, se puede enumerar `User.Claims` para tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="8f12f-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="8f12f-190">Cerrar sesión</span><span class="sxs-lookup"><span data-stu-id="8f12f-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8f12f-191">El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="8f12f-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="8f12f-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="8f12f-193">Post se especifica en *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8f12f-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="8f12f-194">Al hacer clic en el vínculo **Cerrar sesión** , se llama a la acción `LogOut`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="8f12f-195">El código anterior llama al método `_signInManager.SignOutAsync`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="8f12f-196">El método `SignOutAsync` borra las notificaciones del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="8f12f-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="8f12f-197">Identidad de prueba</span><span class="sxs-lookup"><span data-stu-id="8f12f-197">Test Identity</span></span>

<span data-ttu-id="8f12f-198">Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales.</span><span class="sxs-lookup"><span data-stu-id="8f12f-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="8f12f-199">Para probar la identidad, agregue [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página de privacidad.</span><span class="sxs-lookup"><span data-stu-id="8f12f-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="8f12f-200">Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** .</span><span class="sxs-lookup"><span data-stu-id="8f12f-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="8f12f-201">Se le redirigirá a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="8f12f-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="8f12f-202">Explorar identidad</span><span class="sxs-lookup"><span data-stu-id="8f12f-202">Explore Identity</span></span>

<span data-ttu-id="8f12f-203">Para explorar la identidad con más detalle:</span><span class="sxs-lookup"><span data-stu-id="8f12f-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="8f12f-204">Crear origen de la interfaz de usuario de identidad completa</span><span class="sxs-lookup"><span data-stu-id="8f12f-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="8f12f-205">Examine el origen de cada página y recorra el depurador.</span><span class="sxs-lookup"><span data-stu-id="8f12f-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="8f12f-206">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="8f12f-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8f12f-207">Todos los paquetes de NuGet que dependen de la identidad se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8f12f-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="8f12f-208">El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="8f12f-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="8f12f-209">Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8f12f-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8f12f-210">Migrar a ASP.NET Core identidad</span><span class="sxs-lookup"><span data-stu-id="8f12f-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8f12f-211">Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="8f12f-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="8f12f-212">Establecer la seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="8f12f-212">Setting password strength</span></span>

<span data-ttu-id="8f12f-213">Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.</span><span class="sxs-lookup"><span data-stu-id="8f12f-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f12f-214">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="8f12f-214">Next Steps</span></span>

* [<span data-ttu-id="8f12f-215">Configuración de Identity</span><span class="sxs-lookup"><span data-stu-id="8f12f-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
