---
title: Introducción a la identidad de un núcleo de ASP.NET
author: rick-anderson
description: Usar identidad con una aplicación de ASP.NET Core. Incluye requisitos de configuración de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars etc.).
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: b3bfae665403162db1fb012fac227275b1dfd6c9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="f7046-104">Introducción a la identidad de un núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7046-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="f7046-105">Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f7046-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f7046-106">Identidad de ASP.NET Core es un sistema de pertenencia que le permite agregar funcionalidad de inicio de sesión a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7046-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="f7046-107">Los usuarios pueden crear una cuenta y el inicio de sesión con un nombre de usuario y contraseña o se puede usar un proveedor de inicio de sesión externo como Facebook, Google, Microsoft Account, Twitter u otras personas.</span><span class="sxs-lookup"><span data-stu-id="f7046-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="f7046-108">Puede configurar ASP.NET Core Identity para utilizar una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="f7046-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="f7046-109">Como alternativa, puede usar su propio almacén persistente, por ejemplo, un almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="f7046-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="f7046-110">Este documento contiene instrucciones para Visual Studio y para el uso de la CLI.</span><span class="sxs-lookup"><span data-stu-id="f7046-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="f7046-111">Ver o descargar el código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f7046-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="f7046-112">(Cómo descargar)</span><span class="sxs-lookup"><span data-stu-id="f7046-112">(How to download)</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="f7046-113">Información general de identidad</span><span class="sxs-lookup"><span data-stu-id="f7046-113">Overview of Identity</span></span>

<span data-ttu-id="f7046-114">En este tema, podrá aprender a usar ASP.NET Core Identity para agregar funcionalidad a registrar, inicie sesión y cierra la sesión un usuario.</span><span class="sxs-lookup"><span data-stu-id="f7046-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="f7046-115">Para obtener instrucciones detalladas acerca de cómo crear aplicaciones con ASP.NET Core Identity, vea la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="f7046-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="f7046-116">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="f7046-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7046-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7046-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="f7046-118">En Visual Studio, seleccione **archivo** > **New** > **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f7046-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="f7046-119">Seleccione **aplicación Web de ASP.NET Core** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f7046-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](identity/_static/01-new-project.png)

   <span data-ttu-id="f7046-121">Seleccione un ASP.NET Core **aplicación Web (Model-View-Controller)** para ASP.NET Core 2.x, a continuación, seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="f7046-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](identity/_static/02-new-project.png)

   <span data-ttu-id="f7046-123">Un cuadro de diálogo aparece oferta las opciones de autenticación.</span><span class="sxs-lookup"><span data-stu-id="f7046-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="f7046-124">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar** para volver al cuadro de diálogo anterior.</span><span class="sxs-lookup"><span data-stu-id="f7046-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Cuadro de diálogo Nuevo proyecto](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="f7046-126">Seleccionar **cuentas de usuario individuales** indica a Visual Studio para crear modelos, ViewModels, vistas, controladores y otros recursos necesarios para la autenticación como parte de la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="f7046-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7046-127">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7046-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="f7046-128">Si usa la CLI de núcleo. NET, cree el nuevo proyecto utilizando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="f7046-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="f7046-129">Este comando crea un nuevo proyecto con el mismo código de plantilla de identidad que Visual Studio crea.</span><span class="sxs-lookup"><span data-stu-id="f7046-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="f7046-130">El proyecto creado contiene el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` paquete, que conserva los datos de identidad y el esquema a SQL Server mediante [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="f7046-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="f7046-131">Configurar servicios de identidad y agregar middleware en `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f7046-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="f7046-132">Los servicios de identidad se agregan a la aplicación en el `ConfigureServices` método en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="f7046-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7046-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7046-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="f7046-134">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7046-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="f7046-135">Identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="f7046-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="f7046-136">`UseAuthentication` Agrega autenticación [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="f7046-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7046-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7046-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="f7046-138">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7046-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="f7046-139">Identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="f7046-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="f7046-140">`UseIdentity` Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="f7046-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   * * *
   <span data-ttu-id="f7046-141">Para obtener más información sobre el proceso de inicio aplicación, consulte [inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f7046-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="f7046-142">Cree un usuario.</span><span class="sxs-lookup"><span data-stu-id="f7046-142">Create a user.</span></span>

   <span data-ttu-id="f7046-143">Inicie la aplicación y, a continuación, haga clic en el **registrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="f7046-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="f7046-144">Si se trata de la primera vez que se va a realizar esta acción, puede ser necesario para ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f7046-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="f7046-145">La aplicación le pide que **migraciones aplicar**.</span><span class="sxs-lookup"><span data-stu-id="f7046-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="f7046-146">Si es necesario, actualice la página.</span><span class="sxs-lookup"><span data-stu-id="f7046-146">Refresh the page if needed.</span></span>

   ![Aplicar página Web de migraciones](identity/_static/apply-migrations.png)

   <span data-ttu-id="f7046-148">Como alternativa, puede probar mediante ASP.NET Core Identity con la aplicación sin una base de datos persistente desde una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="f7046-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="f7046-149">Para usar una base de datos en memoria, agregue el ``Microsoft.EntityFrameworkCore.InMemory`` el paquete a la aplicación y modificar la llamada de la aplicación a ``AddDbContext`` en ``ConfigureServices`` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="f7046-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="f7046-150">Cuando el usuario hace clic en el **registrar** vínculo, el ``Register`` acción se invoca en ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="f7046-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="f7046-151">El ``Register`` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a ``AccountController`` por inyección de dependencia):</span><span class="sxs-lookup"><span data-stu-id="f7046-151">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="f7046-152">Si el usuario se creó correctamente, el usuario se registra en la llamada a ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="f7046-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

   <span data-ttu-id="f7046-153">**Nota:** vea [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar el inicio de sesión de inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="f7046-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="f7046-154">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="f7046-154">Log in.</span></span>

   <span data-ttu-id="f7046-155">Los usuarios pueden iniciar sesión, haga clic en el **sesión** vínculo en la parte superior del sitio, o puede navegar a la página de inicio de sesión si éstos intentan obtener acceso a una parte del sitio que requiera una autorización.</span><span class="sxs-lookup"><span data-stu-id="f7046-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="f7046-156">Cuando el usuario envía el formulario en la página de inicio de sesión, el ``AccountController`` ``Login`` acción se denomina.</span><span class="sxs-lookup"><span data-stu-id="f7046-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

   <span data-ttu-id="f7046-157">El ``Login`` acción llamadas ``PasswordSignInAsync`` en el ``_signInManager`` objeto (proporcionado a ``AccountController`` por inyección de dependencia).</span><span class="sxs-lookup"><span data-stu-id="f7046-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="f7046-158">La base de ``Controller`` clase expone un ``User`` propiedad que se puede acceder desde los métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="f7046-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="f7046-159">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="f7046-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="f7046-160">Para obtener más información, consulte [autorización](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="f7046-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="f7046-161">Cierre sesión.</span><span class="sxs-lookup"><span data-stu-id="f7046-161">Log out.</span></span>

   <span data-ttu-id="f7046-162">Al hacer clic en el **cerrar sesión** vincular llamadas el `LogOut` acción.</span><span class="sxs-lookup"><span data-stu-id="f7046-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="f7046-163">El código anterior por encima de las llamadas del `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="f7046-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="f7046-164">El `SignOutAsync` método borra las solicitudes del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="f7046-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="f7046-165">Configuración.</span><span class="sxs-lookup"><span data-stu-id="f7046-165">Configuration.</span></span>

   <span data-ttu-id="f7046-166">Identidad tiene algunos comportamientos predeterminados que se pueden invalidar en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7046-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="f7046-167">`IdentityOptions` no es necesario configurar al utilizar los comportamientos predeterminados.</span><span class="sxs-lookup"><span data-stu-id="f7046-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="f7046-168">El código siguiente establece varias opciones de seguridad de contraseña:</span><span class="sxs-lookup"><span data-stu-id="f7046-168">The following code sets several password strength options:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7046-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7046-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7046-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7046-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   * * *
   <span data-ttu-id="f7046-171">Para obtener más información acerca de cómo configurar la identidad, vea [configurar identidad](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="f7046-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="f7046-172">También puede configurar el tipo de datos de la clave principal, vea [tipo de datos de las claves principales de configurar la identidad](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="f7046-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="f7046-173">Ver la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7046-173">View the database.</span></span>

   <span data-ttu-id="f7046-174">Si la aplicación usa una base de datos de SQL Server (el valor predeterminado en Windows y para usuarios de Visual Studio), puede ver la base de datos de la aplicación creada.</span><span class="sxs-lookup"><span data-stu-id="f7046-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="f7046-175">Puede usar **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="f7046-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="f7046-176">O bien, desde Visual Studio, seleccione **vista** > **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f7046-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="f7046-177">Conectarse a **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="f7046-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="f7046-178">La base de datos con un nombre que coincida con **aspnet - <*nombre del proyecto*>-<*cadena de fecha* >**  se muestra.</span><span class="sxs-lookup"><span data-stu-id="f7046-178">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

   ![Menú contextual en la tabla de base de datos de AspNetUsers](identity/_static/04-db.png)

   <span data-ttu-id="f7046-180">Expanda la base de datos y su **tablas**, a continuación, haga clic en el **dbo. AspNetUsers** de tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="f7046-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="f7046-181">Compruebe que funciona de identidad</span><span class="sxs-lookup"><span data-stu-id="f7046-181">Verify Identity works</span></span>

    <span data-ttu-id="f7046-182">El valor predeterminado *aplicación Web de ASP.NET Core* plantilla de proyecto permite a los usuarios tener acceso a cualquier acción en la aplicación sin necesidad de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="f7046-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="f7046-183">Para comprobar que funciona ASP.NET Identity, agregue un`[Authorize]` atribuir a la `About` acción de la `Home` controlador.</span><span class="sxs-lookup"><span data-stu-id="f7046-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f7046-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7046-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="f7046-185">Ejecutar el proyecto mediante **Ctrl** + **F5** y navegue hasta la **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="f7046-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="f7046-186">Solo los usuarios autenticados pueden tener acceso a la **sobre** página ahora, por lo que ASP.NET le redirige a la página de inicio de sesión para iniciar sesión o regístrese.</span><span class="sxs-lookup"><span data-stu-id="f7046-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f7046-187">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7046-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="f7046-188">Abra una ventana de comandos y desplácese hasta la raíz del proyecto que contiene el directorio el `.csproj` archivo.</span><span class="sxs-lookup"><span data-stu-id="f7046-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="f7046-189">Ejecute el [dotnet ejecutar](/dotnet/core/tools/dotnet-run) comando para ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f7046-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="f7046-190">Busque la dirección URL especificada en los resultados de la [dotnet ejecutar](/dotnet/core/tools/dotnet-run) comando.</span><span class="sxs-lookup"><span data-stu-id="f7046-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="f7046-191">La dirección URL debe apuntar a `localhost` con un número de puerto generado.</span><span class="sxs-lookup"><span data-stu-id="f7046-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="f7046-192">Navegue hasta la **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="f7046-192">Navigate to the **About** page.</span></span> <span data-ttu-id="f7046-193">Solo los usuarios autenticados pueden tener acceso a la **sobre** página ahora, por lo que ASP.NET le redirige a la página de inicio de sesión para iniciar sesión o regístrese.</span><span class="sxs-lookup"><span data-stu-id="f7046-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="f7046-194">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="f7046-194">Identity Components</span></span>

<span data-ttu-id="f7046-195">El ensamblado de referencia principal para el sistema de identidades `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="f7046-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="f7046-196">Este paquete contiene el conjunto básico de interfaces para ASP.NET Core Identity y se incluye por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="f7046-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="f7046-197">Estas dependencias necesarios para usar el sistema de identidades en aplicaciones de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f7046-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="f7046-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contiene los tipos necesarios para usar la identidad con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f7046-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="f7046-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core es la tecnología de acceso a datos recomendado de Microsoft para las bases de datos relacional como SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f7046-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="f7046-200">Para las pruebas, puede usar `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="f7046-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="f7046-201">`Microsoft.AspNetCore.Authentication.Cookies` -Middleware que permite que una aplicación utilizar la autenticación basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="f7046-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="f7046-202">Migrar a la identidad de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7046-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="f7046-203">Para obtener información adicional e instrucciones sobre cómo migrar su identidad existente store vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="f7046-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="f7046-204">Configuración de seguridad de la contraseña</span><span class="sxs-lookup"><span data-stu-id="f7046-204">Setting password strength</span></span>

<span data-ttu-id="f7046-205">Vea [configuración](#pw) para obtener un ejemplo que establece los requisitos de contraseña mínima.</span><span class="sxs-lookup"><span data-stu-id="f7046-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7046-206">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f7046-206">Next Steps</span></span>

* [<span data-ttu-id="f7046-207">Migrar de autenticación e identidad</span><span class="sxs-lookup"><span data-stu-id="f7046-207">Migrate Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="f7046-208">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="f7046-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="f7046-209">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="f7046-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="f7046-210">Facebook, Google y la autenticación de proveedor externo.</span><span class="sxs-lookup"><span data-stu-id="f7046-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
