---
title: "Introducción a la identidad de un núcleo de ASP.NET"
author: rick-anderson
description: "Uso de la identidad con una aplicación de ASP.NET Core"
keywords: "Autorización de ASP.NET Core, identidad, seguridad"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0c17daa96bc69dc0b8393811a4dfe0e5dc4a1884
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introducción a la identidad de un núcleo de ASP.NET

Por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), y [Steve Smith](https://ardalis.com/)

Identidad de ASP.NET Core es un sistema de pertenencia que le permite agregar funcionalidad de inicio de sesión a la aplicación. Los usuarios pueden crear una cuenta y el inicio de sesión con un nombre de usuario y contraseña o se puede usar un proveedor de inicio de sesión externo como Facebook, Google, Microsoft Account, Twitter u otras personas.

Puede configurar ASP.NET Core Identity para utilizar una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil. Como alternativa, puede usar su propio almacén persistente, por ejemplo el almacenamiento de tablas de Azure. Este documento contiene instrucciones para Visual Studio y para el uso de la CLI.

## <a name="overview-of-identity"></a>Información general de identidad

En este tema, podrá aprender a usar ASP.NET Core Identity para agregar funcionalidad a registrar, inicie sesión y cierra la sesión un usuario. Para obtener instrucciones detalladas acerca de cómo crear aplicaciones con ASP.NET Core Identity, vea la sección pasos siguientes al final de este artículo.

1.  Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    En Visual Studio, seleccione **archivo** -> **New** -> **proyecto**. Seleccione el **aplicación Web ASP.NET** desde el **nuevo proyecto** cuadro de diálogo. Al seleccionar un ASP.NET Core **aplicación Web** con **cuentas de usuario individuales** como método de autenticación.

    Nota: Debe seleccionar **cuentas de usuario individuales**.
 
    ![Cuadro de diálogo Nuevo proyecto](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)
    Si usa la CLI de núcleo. NET, cree el nuevo proyecto utilizando ``dotnet new mvc --auth Individual``. Esto creará un nuevo proyecto con el mismo código de plantilla de identidad que Visual Studio crea.
 
    El proyecto creado contiene el `Microsoft.AspNetCore.Identity.EntityFrameworkCore` paquete, que se conservará los datos de identidad y el esquema a SQL Server mediante [Entity Framework Core](https://docs.microsoft.com/ef/).
    
    ---
 
2.  Configurar servicios de identidad y agregar middleware en `Startup`.

    Los servicios de identidad se agregan a la aplicación en el `ConfigureServices` método en la `Startup` clase:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).
    
    Identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método. `UseAuthentication`Agrega autenticación [middleware](xref:fundamentals/middleware) a la canalización de solicitud.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Estos servicios se ponen a disposición a la aplicación a través de [inyección de dependencia](xref:fundamentals/dependency-injection).
    
    Identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el `Configure` método. `UseIdentity`Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware) a la canalización de solicitud.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Para obtener más información sobre el proceso de inicio aplicación, consulte [inicio de la aplicación](xref:fundamentals/startup).

3.  Cree un usuario.

    Inicie la aplicación y, a continuación, haga clic en el **registrar** vínculo.

    Si se trata de la primera vez que se va a realizar esta acción, puede ser necesario para ejecutar las migraciones. La aplicación le pide que **migraciones aplicar**:
    
    ![Aplicar página Web de migraciones](identity/_static/apply-migrations.png)
    
    Como alternativa, puede probar mediante ASP.NET Core Identity con la aplicación sin una base de datos persistente desde una base de datos en memoria. Para usar una base de datos en memoria, agregue el ``Microsoft.EntityFrameworkCore.InMemory`` el paquete a la aplicación y modificar la llamada de la aplicación a ``AddDbContext`` en ``ConfigureServices`` como se indica a continuación:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Cuando el usuario hace clic en el **registrar** vínculo, el ``Register`` acción se invoca en ``AccountController``. El ``Register`` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a ``AccountController`` por inyección de dependencia):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Si el usuario se creó correctamente, el usuario se registra en la llamada a ``_signInManager.SignInAsync``.

    **Nota:** vea [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar el inicio de sesión de inmediato en el registro.
 
4.  Inicia sesión.
 
    Los usuarios pueden iniciar sesión, haga clic en el **sesión** vínculo en la parte superior del sitio, o puede navegar a la página de inicio de sesión si éstos intentan obtener acceso a una parte del sitio que requiera una autorización. Cuando el usuario envía el formulario en la página de inicio de sesión, el ``AccountController`` ``Login`` acción se denomina.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    El ``Login`` acción llamadas ``PasswordSignInAsync`` en el ``_signInManager`` objeto (proporcionado a ``AccountController`` por inyección de dependencia).
 
    La base de ``Controller`` clase expone un ``User`` propiedad que se puede acceder desde los métodos de controlador. Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización. Para obtener más información, consulte [autorización](xref:security/authorization/index).
 
5.  Cierre sesión.
 
    Al hacer clic en el **cerrar sesión** vincular llamadas el `LogOut` acción.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    El código anterior por encima de las llamadas del `_signInManager.SignOutAsync` método. El `SignOutAsync` método borra las solicitudes del usuario almacenadas en una cookie.
 
6.  Configuración.

    Identidad tiene algunos comportamientos predeterminados que se pueden reemplazar en la clase de inicio de la aplicación. No es necesario configurar ``IdentityOptions`` si utilizas los comportamientos predeterminados.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Para obtener más información acerca de cómo configurar la identidad, vea [configurar identidad](xref:security/authentication/identity-configuration).
    
    También puede configurar el tipo de datos de la clave principal, vea [tipo de datos de las claves principales de configurar la identidad](xref:security/authentication/identity-primary-key-configuration).
 
7.  Ver la base de datos.

    Si la aplicación usa una base de datos de SQL Server (el valor predeterminado en Windows y para usuarios de Visual Studio), puede ver la base de datos de la aplicación creada. Puede usar **SQL Server Management Studio**. O bien, desde Visual Studio, seleccione **vista** -> **Explorador de objetos de SQL Server**. Conectarse a **(localdb) \MSSQLLocalDB**. La base de datos con un nombre que coincida con * *aspnet - <*nombre del proyecto*>-<*cadena de fecha*> ** se muestra.

    ![Menú contextual en la tabla de base de datos de AspNetUsers](identity/_static/04-db.png)
    
    Expanda la base de datos y su **tablas**, a continuación, haga clic en el **dbo. AspNetUsers** de tabla y seleccione **ver datos**.

## <a name="identity-components"></a>Componentes de identidad

El ensamblado de referencia principal para el sistema de identidades `Microsoft.AspNetCore.Identity`. Este paquete contiene el conjunto básico de interfaces para ASP.NET Core Identity y se incluye por `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Estas dependencias necesarios para usar el sistema de identidades en aplicaciones de ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contiene los tipos necesarios para usar la identidad con Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core es la tecnología de acceso a datos recomendado de Microsoft para las bases de datos relacional como SQL Server. Para las pruebas, puede usar `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Middleware que permite que una aplicación utilizar la autenticación basada en cookies.

## <a name="migrating-to-aspnet-core-identity"></a>Migrar a la identidad de ASP.NET Core

Para obtener información adicional e instrucciones sobre cómo migrar su identidad existente store vea [migrar autenticación e identidad](xref:migration/identity).

## <a name="next-steps"></a>Pasos siguientes

* [Migración de la autenticación y la identidad](xref:migration/identity)
* [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
* [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
* [Habilitación de la autenticación con Facebook, Google y otros proveedores externos](xref:security/authentication/social/index)
