---
title: "Introducción a la identidad de un núcleo de ASP.NET"
author: rick-anderson
description: "Usar la identidad con una aplicación de ASP.NET Core"
keywords: "Autorización de ASP.NET Core, identidad, seguridad"
ms.author: riande
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 7daf0267a6dc659afbd188ce87e35ca40816a31d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="b380d-104">Introducción a la identidad de un núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b380d-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="b380d-105">Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b380d-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b380d-106">Identidad de ASP.NET Core es un sistema de pertenencia que le permite agregar funcionalidad de inicio de sesión a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b380d-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="b380d-107">Los usuarios pueden crear una cuenta y el inicio de sesión con un nombre de usuario y contraseña o se puede usar un proveedor de inicio de sesión externo como Facebook, Google, Microsoft Account, Twitter u otras personas.</span><span class="sxs-lookup"><span data-stu-id="b380d-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="b380d-108">Puede configurar ASP.NET Core Identity para utilizar una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil.</span><span class="sxs-lookup"><span data-stu-id="b380d-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="b380d-109">Como alternativa, puede usar su propio almacén persistente, por ejemplo, un almacenamiento de tablas de Azure.</span><span class="sxs-lookup"><span data-stu-id="b380d-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="b380d-110">Este documento contiene instrucciones para Visual Studio y para el uso de la CLI.</span><span class="sxs-lookup"><span data-stu-id="b380d-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="b380d-111">Información general de identidad</span><span class="sxs-lookup"><span data-stu-id="b380d-111">Overview of Identity</span></span>

<span data-ttu-id="b380d-112">En este tema, podrá aprender a usar ASP.NET Core Identity para agregar funcionalidad a registrar, inicie sesión y cierra la sesión un usuario.</span><span class="sxs-lookup"><span data-stu-id="b380d-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="b380d-113">Para obtener instrucciones detalladas acerca de cómo crear aplicaciones con ASP.NET Core Identity, vea la sección pasos siguientes al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="b380d-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="b380d-114">Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.</span><span class="sxs-lookup"><span data-stu-id="b380d-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b380d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b380d-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="b380d-116">En Visual Studio, seleccione **archivo** -> **New** -> **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="b380d-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="b380d-117">Seleccione el **aplicación Web ASP.NET** desde el **nuevo proyecto** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="b380d-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="b380d-118">Al seleccionar un ASP.NET Core **Web Application(Model-View-Controller)** para ASP.NET Core 2.x con **cuentas de usuario individuales** como método de autenticación.</span><span class="sxs-lookup"><span data-stu-id="b380d-118">Selecting an ASP.NET Core **Web Application(Model-View-Controller)** for ASP.NET Core 2.x with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="b380d-119">Nota: Debe seleccionar **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="b380d-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Cuadro de diálogo Nuevo proyecto](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b380d-121">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="b380d-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="b380d-122">Si usa la CLI de núcleo. NET, cree el nuevo proyecto utilizando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="b380d-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="b380d-123">Este comando crea un nuevo proyecto con el mismo código de plantilla de identidad que Visual Studio crea.</span><span class="sxs-lookup"><span data-stu-id="b380d-123">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="b380d-124">El proyecto creado contiene el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` paquete, que conserva los datos de identidad y el esquema a SQL Server mediante [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="b380d-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="b380d-125">Configurar servicios de identidad y agregar middleware en `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b380d-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="b380d-126">Los servicios de identidad se agregan a la aplicación en el `ConfigureServices` método en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="b380d-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b380d-127">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b380d-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="b380d-128">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b380d-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="b380d-129">Identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="b380d-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="b380d-130">`UseAuthentication`Agrega autenticación [middleware](xref:fundamentals/middleware) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b380d-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b380d-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b380d-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="b380d-132">Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b380d-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="b380d-133">Identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="b380d-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="b380d-134">`UseIdentity`Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware) a la canalización de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b380d-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="b380d-135">Para obtener más información sobre el proceso de inicio aplicación, consulte [inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b380d-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="b380d-136">Cree un usuario.</span><span class="sxs-lookup"><span data-stu-id="b380d-136">Create a user.</span></span>

    <span data-ttu-id="b380d-137">Inicie la aplicación y, a continuación, haga clic en el **registrar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="b380d-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="b380d-138">Si se trata de la primera vez que se va a realizar esta acción, puede ser necesario para ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b380d-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="b380d-139">La aplicación le pide que **migraciones aplicar**:</span><span class="sxs-lookup"><span data-stu-id="b380d-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Aplicar página Web de migraciones](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="b380d-141">Como alternativa, puede probar mediante ASP.NET Core Identity con la aplicación sin una base de datos persistente desde una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="b380d-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="b380d-142">Para usar una base de datos en memoria, agregue el ``Microsoft.EntityFrameworkCore.InMemory`` el paquete a la aplicación y modificar la llamada de la aplicación a ``AddDbContext`` en ``ConfigureServices`` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="b380d-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="b380d-143">Cuando el usuario hace clic en el **registrar** vínculo, el ``Register`` acción se invoca en ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="b380d-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="b380d-144">El ``Register`` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a ``AccountController`` por inyección de dependencia):</span><span class="sxs-lookup"><span data-stu-id="b380d-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="b380d-145">Si el usuario se creó correctamente, el usuario se registra en la llamada a ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="b380d-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="b380d-146">**Nota:** vea [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar el inicio de sesión de inmediato en el registro.</span><span class="sxs-lookup"><span data-stu-id="b380d-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="b380d-147">Inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="b380d-147">Log in.</span></span>
 
    <span data-ttu-id="b380d-148">Los usuarios pueden iniciar sesión, haga clic en el **sesión** vínculo en la parte superior del sitio, o puede navegar a la página de inicio de sesión si éstos intentan obtener acceso a una parte del sitio que requiera una autorización.</span><span class="sxs-lookup"><span data-stu-id="b380d-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="b380d-149">Cuando el usuario envía el formulario en la página de inicio de sesión, el ``AccountController`` ``Login`` acción se denomina.</span><span class="sxs-lookup"><span data-stu-id="b380d-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="b380d-150">El ``Login`` acción llamadas ``PasswordSignInAsync`` en el ``_signInManager`` objeto (proporcionado a ``AccountController`` por inyección de dependencia).</span><span class="sxs-lookup"><span data-stu-id="b380d-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="b380d-151">La base de ``Controller`` clase expone un ``User`` propiedad que se puede acceder desde los métodos de controlador.</span><span class="sxs-lookup"><span data-stu-id="b380d-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="b380d-152">Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización.</span><span class="sxs-lookup"><span data-stu-id="b380d-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="b380d-153">Para obtener más información, consulte [autorización](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="b380d-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="b380d-154">Cierre sesión.</span><span class="sxs-lookup"><span data-stu-id="b380d-154">Log out.</span></span>
 
    <span data-ttu-id="b380d-155">Al hacer clic en el **cerrar sesión** vincular llamadas el `LogOut` acción.</span><span class="sxs-lookup"><span data-stu-id="b380d-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="b380d-156">El código anterior por encima de las llamadas del `_signInManager.SignOutAsync` método.</span><span class="sxs-lookup"><span data-stu-id="b380d-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="b380d-157">El `SignOutAsync` método borra las solicitudes del usuario almacenadas en una cookie.</span><span class="sxs-lookup"><span data-stu-id="b380d-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="b380d-158">Configuración.</span><span class="sxs-lookup"><span data-stu-id="b380d-158">Configuration.</span></span>

    <span data-ttu-id="b380d-159">Identidad tiene algunos comportamientos predeterminados que se pueden reemplazar en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b380d-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="b380d-160">No es necesario configurar ``IdentityOptions`` si utilizas los comportamientos predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b380d-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b380d-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b380d-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b380d-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b380d-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="b380d-163">Para obtener más información acerca de cómo configurar la identidad, vea [configurar identidad](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="b380d-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="b380d-164">También puede configurar el tipo de datos de la clave principal, vea [tipo de datos de las claves principales de configurar la identidad](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="b380d-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="b380d-165">Ver la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b380d-165">View the database.</span></span>

    <span data-ttu-id="b380d-166">Si la aplicación usa una base de datos de SQL Server (el valor predeterminado en Windows y para usuarios de Visual Studio), puede ver la base de datos de la aplicación creada.</span><span class="sxs-lookup"><span data-stu-id="b380d-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="b380d-167">Puede usar **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="b380d-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="b380d-168">O bien, desde Visual Studio, seleccione **vista** -> **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b380d-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="b380d-169">Conectarse a **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="b380d-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="b380d-170">La base de datos con un nombre que coincida con  **aspnet - <*nombre del proyecto*>-<*cadena de fecha*> ** se muestra.</span><span class="sxs-lookup"><span data-stu-id="b380d-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menú contextual en la tabla de base de datos de AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="b380d-172">Expanda la base de datos y su **tablas**, a continuación, haga clic en el **dbo. AspNetUsers** de tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="b380d-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="b380d-173">Componentes de identidad</span><span class="sxs-lookup"><span data-stu-id="b380d-173">Identity Components</span></span>

<span data-ttu-id="b380d-174">El ensamblado de referencia principal para el sistema de identidades `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="b380d-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="b380d-175">Este paquete contiene el conjunto básico de interfaces para ASP.NET Core Identity y se incluye por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="b380d-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="b380d-176">Estas dependencias necesarios para usar el sistema de identidades en aplicaciones de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b380d-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="b380d-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contiene los tipos necesarios para usar la identidad con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b380d-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="b380d-178">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core es la tecnología de acceso a datos recomendado de Microsoft para las bases de datos relacional como SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b380d-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="b380d-179">Para las pruebas, puede usar `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="b380d-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="b380d-180">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que una aplicación utilizar la autenticación basada en cookies.</span><span class="sxs-lookup"><span data-stu-id="b380d-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="b380d-181">Migrar a la identidad de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b380d-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="b380d-182">Para obtener información adicional e instrucciones sobre cómo migrar su identidad existente store vea [migrar autenticación e identidad](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="b380d-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b380d-183">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b380d-183">Next Steps</span></span>

* [<span data-ttu-id="b380d-184">Migración de la autenticación y la identidad</span><span class="sxs-lookup"><span data-stu-id="b380d-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="b380d-185">Confirmación de cuentas y recuperación de contraseñas</span><span class="sxs-lookup"><span data-stu-id="b380d-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b380d-186">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="b380d-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="b380d-187">Habilitación de la autenticación con Facebook, Google y otros proveedores externos</span><span class="sxs-lookup"><span data-stu-id="b380d-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
