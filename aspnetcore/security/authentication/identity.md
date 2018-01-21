---
title: "Introducción a la identidad de un núcleo de ASP.NET"
author: rick-anderson
description: "Usar la identidad con una aplicación de ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 436a5ecfd126c9660591cd55efc1cc52b9493136
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="76842-103">Introducción a la identidad de un núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="76842-103">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="76842-104">Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="76842-104">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="76842-105">Identidad de ASP.NET Core es un sistema de pertenencia que le permite agregar funcionalidad de inicio de sesión a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76842-105">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="76842-106">Los usuarios pueden crear una cuenta y el inicio de sesión con un nombre de usuario y contraseña o se puede usar un proveedor de inicio de sesión externo como Facebook, Google, Microsoft Account, Twitter u otras personas.</span><span class="sxs-lookup"><span data-stu-id="76842-106">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="76842-107">Puede configurar ASP.NET Core Identity para utilizar una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="76842-107">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="76842-108">Como alternativa, puede usar su propio almacén persistente, por ejemplo, un almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="76842-108">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="76842-109">Este documento contiene instrucciones para Visual Studio y para el uso de la CLI.</span><span class="sxs-lookup"><span data-stu-id="76842-109">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="76842-110">Ver o descargar el código de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="76842-110">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="76842-111">(Cómo descargar)</span><span class="sxs-lookup"><span data-stu-id="76842-111">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="76842-112">Información general de identidad</span><span class="sxs-lookup"><span data-stu-id="76842-112">Overview of Identity</span></span>

<span data-ttu-id="76842-113">En este tema, podrá aprender a usar ASP.NET Core Identity para agregar funcionalidad a registrar, inicie sesión y cierra la sesión un usuario.</span><span class="sxs-lookup"><span data-stu-id="76842-113">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="76842-114">Para obtener instrucciones detalladas acerca de cómo crear aplicaciones con ASP.NET Core Identity, vea la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="76842-114">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="76842-115">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="76842-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76842-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76842-116">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="76842-117">En Visual Studio, seleccione **archivo** > **New** > **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="76842-117">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="76842-118">Seleccione **aplicación Web de ASP.NET Core** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="76842-118">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Cuadro de diálogo Nuevo proyecto](identity/_static/01-new-project.png)

    <span data-ttu-id="76842-120">Seleccione un ASP.NET Core **aplicación Web (Model-View-Controller)** para ASP.NET Core 2.x, a continuación, seleccione **Cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="76842-120">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Cuadro de diálogo Nuevo proyecto](identity/_static/02-new-project.png)

    <span data-ttu-id="76842-122">Un cuadro de diálogo aparece oferta las opciones de autenticación.</span><span class="sxs-lookup"><span data-stu-id="76842-122">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="76842-123">Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar** para volver al cuadro de diálogo anterior.</span><span class="sxs-lookup"><span data-stu-id="76842-123">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Cuadro de diálogo Nuevo proyecto](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="76842-125">Seleccionar **cuentas de usuario individuales** indica a Visual Studio para crear modelos, ViewModels, vistas, controladores y otros recursos necesarios para la autenticación como parte de la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="76842-125">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="76842-126">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="76842-126">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="76842-127">Si usa la CLI de núcleo. NET, cree el nuevo proyecto utilizando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="76842-127">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="76842-128">Este comando crea un nuevo proyecto con el mismo código de plantilla de identidad que Visual Studio crea.</span><span class="sxs-lookup"><span data-stu-id="76842-128">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="76842-129">El proyecto creado contiene el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` paquete, que conserva los datos de identidad y el esquema a SQL Server mediante [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="76842-129">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="76842-130">Configurar servicios de identidad y agregar middleware en `Startup`.</span><span class="sxs-lookup"><span data-stu-id="76842-130">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="76842-131">Los servicios de identidad se agregan a la aplicación en el `ConfigureServices` método en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="76842-131">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="76842-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="76842-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="76842-133">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="76842-133">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="76842-134">Identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="76842-134">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="76842-135">`UseAuthentication`Agrega autenticación [middleware](xref:fundamentals/middleware) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="76842-135">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="76842-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="76842-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="76842-137">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="76842-137">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="76842-138">Identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="76842-138">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="76842-139">`UseIdentity`Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="76842-139">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="76842-140">Para obtener más información sobre el proceso de inicio aplicación, consulte [inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="76842-140">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="76842-141">Cree un usuario.</span><span class="sxs-lookup"><span data-stu-id="76842-141">Create a user.</span></span>

    <span data-ttu-id="76842-142">Inicie la aplicación y, a continuación, haga clic en el **registrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="76842-142">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="76842-143">Si se trata de la primera vez que se va a realizar esta acción, puede ser necesario para ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="76842-143">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="76842-144">La aplicación le pide que **migraciones aplicar**.</span><span class="sxs-lookup"><span data-stu-id="76842-144">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="76842-145">Si es necesario, actualice la página.</span><span class="sxs-lookup"><span data-stu-id="76842-145">Refresh the page if needed.</span></span>
    
    ![Aplicar página Web de migraciones](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="76842-147">Como alternativa, puede probar mediante ASP.NET Core Identity con la aplicación sin una base de datos persistente desde una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="76842-147">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="76842-148">Para usar una base de datos en memoria, agregue el ``Microsoft.EntityFrameworkCore.InMemory`` el paquete a la aplicación y modificar la llamada de la aplicación a ``AddDbContext`` en ``ConfigureServices`` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="76842-148">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="76842-149">Cuando el usuario hace clic en el **registrar** vínculo, el ``Register`` acción se invoca en ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="76842-149">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="76842-150">El ``Register`` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a ``AccountController`` por inyección de dependencia):</span><span class="sxs-lookup"><span data-stu-id="76842-150">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="76842-151">Si el usuario se creó correctamente, el usuario se registra en la llamada a ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="76842-151">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="76842-152">**Nota:** vea [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar el inicio de sesión de inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="76842-152">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="76842-153">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="76842-153">Log in.</span></span>
 
    <span data-ttu-id="76842-154">Los usuarios pueden iniciar sesión, haga clic en el **sesión** vínculo en la parte superior del sitio, o puede navegar a la página de inicio de sesión si éstos intentan obtener acceso a una parte del sitio que requiera una autorización.</span><span class="sxs-lookup"><span data-stu-id="76842-154">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="76842-155">Cuando el usuario envía el formulario en la página de inicio de sesión, el ``AccountController`` ``Login`` acción se denomina.</span><span class="sxs-lookup"><span data-stu-id="76842-155">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="76842-156">El ``Login`` acción llamadas ``PasswordSignInAsync`` en el ``_signInManager`` objeto (proporcionado a ``AccountController`` por inyección de dependencia).</span><span class="sxs-lookup"><span data-stu-id="76842-156">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="76842-157">La base de ``Controller`` clase expone un ``User`` propiedad que se puede acceder desde los métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="76842-157">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="76842-158">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="76842-158">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="76842-159">Para obtener más información, consulte [autorización](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="76842-159">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="76842-160">Cierre sesión.</span><span class="sxs-lookup"><span data-stu-id="76842-160">Log out.</span></span>
 
    <span data-ttu-id="76842-161">Al hacer clic en el **cerrar sesión** vincular llamadas el `LogOut` acción.</span><span class="sxs-lookup"><span data-stu-id="76842-161">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="76842-162">El código anterior por encima de las llamadas del `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="76842-162">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="76842-163">El `SignOutAsync` método borra las solicitudes del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="76842-163">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="76842-164">Configuración.</span><span class="sxs-lookup"><span data-stu-id="76842-164">Configuration.</span></span>

    <span data-ttu-id="76842-165">Identidad tiene algunos comportamientos predeterminados que se pueden reemplazar en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76842-165">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="76842-166">No es necesario configurar ``IdentityOptions`` si utilizas los comportamientos predeterminados.</span><span class="sxs-lookup"><span data-stu-id="76842-166">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="76842-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="76842-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="76842-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="76842-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="76842-169">Para obtener más información acerca de cómo configurar la identidad, vea [configurar identidad](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="76842-169">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="76842-170">También puede configurar el tipo de datos de la clave principal, vea [tipo de datos de las claves principales de configurar la identidad](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="76842-170">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="76842-171">Ver la base de datos.</span><span class="sxs-lookup"><span data-stu-id="76842-171">View the database.</span></span>

    <span data-ttu-id="76842-172">Si la aplicación usa una base de datos de SQL Server (el valor predeterminado en Windows y para usuarios de Visual Studio), puede ver la base de datos de la aplicación creada.</span><span class="sxs-lookup"><span data-stu-id="76842-172">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="76842-173">Puede usar **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="76842-173">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="76842-174">O bien, desde Visual Studio, seleccione **vista** > **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="76842-174">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="76842-175">Conectarse a **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="76842-175">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="76842-176">La base de datos con un nombre que coincida con **aspnet - <*nombre del proyecto*>-<*cadena de fecha* >**  se muestra.</span><span class="sxs-lookup"><span data-stu-id="76842-176">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menú contextual en la tabla de base de datos de AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="76842-178">Expanda la base de datos y su **tablas**, a continuación, haga clic en el **dbo. AspNetUsers** de tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="76842-178">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="76842-179">Compruebe que funciona de identidad</span><span class="sxs-lookup"><span data-stu-id="76842-179">Verify Identity works</span></span>

    <span data-ttu-id="76842-180">El valor predeterminado *aplicación Web de ASP.NET Core* plantilla de proyecto permite a los usuarios tener acceso a cualquier acción en la aplicación sin necesidad de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="76842-180">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="76842-181">Para comprobar que funciona ASP.NET Identity, agregue un`[Authorize]` atribuir a la `About` acción de la `Home` controlador.</span><span class="sxs-lookup"><span data-stu-id="76842-181">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76842-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76842-182">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="76842-183">Ejecutar el proyecto mediante **Ctrl** + **F5** y navegue hasta la **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="76842-183">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="76842-184">Solo los usuarios autenticados pueden tener acceso a la **sobre** página ahora, por lo que ASP.NET le redirige a la página de inicio de sesión para iniciar sesión o regístrese.</span><span class="sxs-lookup"><span data-stu-id="76842-184">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="76842-185">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="76842-185">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="76842-186">Abra una ventana de comandos y desplácese hasta la raíz del proyecto que contiene el directorio el `.csproj` archivo.</span><span class="sxs-lookup"><span data-stu-id="76842-186">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="76842-187">Ejecute el `dotnet run` comando para ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="76842-187">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="76842-188">Busque la dirección URL especificada en los resultados de la `dotnet run` comando.</span><span class="sxs-lookup"><span data-stu-id="76842-188">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="76842-189">La dirección URL debe apuntar a `localhost` con un número de puerto generado.</span><span class="sxs-lookup"><span data-stu-id="76842-189">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="76842-190">Navegue hasta la **sobre** página.</span><span class="sxs-lookup"><span data-stu-id="76842-190">Navigate to the **About** page.</span></span> <span data-ttu-id="76842-191">Solo los usuarios autenticados pueden tener acceso a la **sobre** página ahora, por lo que ASP.NET le redirige a la página de inicio de sesión para iniciar sesión o regístrese.</span><span class="sxs-lookup"><span data-stu-id="76842-191">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="76842-192">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="76842-192">Identity Components</span></span>

<span data-ttu-id="76842-193">El ensamblado de referencia principal para el sistema de identidades `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="76842-193">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="76842-194">Este paquete contiene el conjunto básico de interfaces para ASP.NET Core Identity y se incluye por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="76842-194">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="76842-195">Estas dependencias necesarios para usar el sistema de identidades en aplicaciones de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="76842-195">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="76842-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contiene los tipos necesarios para usar la identidad con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="76842-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="76842-197">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core es la tecnología de acceso a datos recomendado de Microsoft para las bases de datos relacional como SQL Server.</span><span class="sxs-lookup"><span data-stu-id="76842-197">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="76842-198">Para las pruebas, puede usar `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="76842-198">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="76842-199">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que una aplicación utilizar la autenticación basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="76842-199">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="76842-200">Migrar a la identidad de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76842-200">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="76842-201">Para obtener información adicional e instrucciones sobre cómo migrar su identidad existente store vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="76842-201">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="76842-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="76842-202">Next Steps</span></span>

* [<span data-ttu-id="76842-203">Migración de la autenticación y la identidad</span><span class="sxs-lookup"><span data-stu-id="76842-203">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="76842-204">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="76842-204">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="76842-205">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="76842-205">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="76842-206">Habilitación de la autenticación con Facebook, Google y otros proveedores externos</span><span class="sxs-lookup"><span data-stu-id="76842-206">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
