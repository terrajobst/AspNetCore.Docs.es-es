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
# <a name="introduction-to-identity-on-aspnet-core"></a>Introducción a la identidad en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core identidad es un sistema de pertenencia que agrega funcionalidad de inicio de sesión a ASP.NET Core aplicaciones. Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo. Entre los proveedores de inicio de sesión externos admitidos se incluyen [Facebook, Google, cuenta de Microsoft y Twitter](xref:security/authentication/social/index).

La identidad puede configurarse mediante una base de datos de SQL Server para almacenar nombres de usuario, contraseñas y datos de perfil. Como alternativa, se puede usar otro almacén persistente, por ejemplo, Azure Table Storage.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Cómo descargar)](xref:index#how-to-download-a-sample)).

En este tema, aprenderá a usar la identidad para registrarse, iniciar sesión y cerrar la sesión de un usuario. Para obtener instrucciones más detalladas sobre la creación de aplicaciones que usan la identidad, consulte la sección pasos siguientes al final de este artículo.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity y AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) se presentó en ASP.net Core 2,1. Llamar a `AddDefaultIdentity` es similar a llamar a lo siguiente:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Vea [origen de AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) para obtener más información.

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Creación de una aplicación web con autenticación

Cree un proyecto de aplicación Web de ASP.NET Core con cuentas de usuario individuales.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Seleccione **Archivo** > **Nuevo** > **Proyecto**.
* Seleccione **Aplicación web de ASP.NET Core**. Asigne al proyecto el nombre **WebApp1** para que tenga el mismo espacio de nombres que la descarga del proyecto. Haga clic en **Aceptar**.
* Seleccione una **aplicación web**de ASP.net Core y, a continuación, seleccione **cambiar autenticación**.
* Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

El proyecto generado proporciona [ASP.net Core identidad](xref:security/authentication/identity) como una [biblioteca de clases de Razor](xref:razor-pages/ui-class). La biblioteca de clases de Razor de identidad expone puntos de conexión con el área @no__t 0. Por ejemplo:

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>Aplicación de migraciones

Aplique las migraciones para inicializar la base de datos.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ejecute el siguiente comando en la consola del administrador de paquetes (PMC):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Probar registro e inicio de sesión

Ejecute la aplicación y registre un usuario. En función del tamaño de la pantalla, es posible que tenga que seleccionar el botón de alternancia de navegación para ver los vínculos de **Inicio de sesión** y **registro** .

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Configurar servicios de identidad

Los servicios se agregan en `ConfigureServices`. El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

El código anterior configura la identidad con valores de opción predeterminados. Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).

   La identidad se habilita llamando a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Los servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).

   La identidad está habilitada para la aplicación mediante una llamada a `UseAuthentication` en el método `Configure`. `UseAuthentication` agrega [middleware](xref:fundamentals/middleware/index) de autenticación a la canalización de solicitudes.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Estos servicios se ponen a disposición de la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).

   La identidad está habilitada para la aplicación mediante una llamada a `UseIdentity` en el método `Configure`. `UseIdentity` agrega [middleware](xref:fundamentals/middleware/index) de autenticación basada en cookies a la canalización de solicitudes.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Para obtener más información, vea la [clase IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y el inicio de la [aplicación](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Registro de scaffolding, Inicio de sesión y cierre de sesión

Siga la [identidad scaffolding en un proyecto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones de autorización para generar el código que se muestra en esta sección.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Agregue los archivos de registro, Inicio de sesión y cierre de sesión.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos. De lo contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell usa punto y coma como separador de comandos. Al usar PowerShell, use el carácter de escape en la lista de archivos o ponga la lista de archivos entre comillas dobles, como se muestra en el ejemplo anterior.

---

### <a name="examine-register"></a>Examinar registro

::: moniker range=">= aspnetcore-2.1"

   Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción `RegisterModel.OnPostAsync`. El usuario lo crea [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el objeto `_userManager`. la inserción de dependencias proporciona `_userManager`):

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Cuando un usuario hace clic en el vínculo de **registro** , se invoca la acción `Register` en `AccountController`. La acción `Register` crea el usuario llamando a `CreateAsync` en el objeto `_userManager` (proporcionado a `AccountController` mediante la inserción de dependencias):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Si el usuario se creó correctamente, el usuario inicia sesión mediante la llamada a `_signInManager.SignInAsync`.

   **Nota:** Consulte [confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) de la cuenta para conocer los pasos para evitar el inicio de sesión inmediato en el registro.

### <a name="log-in"></a>Iniciar sesión

::: moniker range=">= aspnetcore-2.1"

El formulario de inicio de sesión se muestra cuando:

* Se selecciona el vínculo **iniciar sesión** .
* Un usuario intenta obtener acceso a una página restringida para la que no está autorizado para acceder **o** cuando el sistema no la ha autenticado.

Cuando se envía el formulario de la página de inicio de sesión, se llama a la acción `OnPostAsync`. se llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado por la inserción de dependencias).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   La clase base `Controller` expone una propiedad `User` a la que se puede tener acceso desde métodos de controlador. Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización. Para obtener más información, consulta <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

El formulario de inicio de sesión se muestra cuando los usuarios seleccionan el vínculo **iniciar sesión** o se redirigen al acceder a una página que requiere autenticación. Cuando el usuario envía el formulario en la página de inicio de sesión, se llama a la acción `AccountController` `Login`.

La acción `Login` llama a `PasswordSignInAsync` en el objeto `_signInManager` (proporcionado a `AccountController` mediante la inserción de dependencias).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

La clase base (`Controller` o `PageModel`) expone una propiedad `User`. Por ejemplo, se puede enumerar `User.Claims` para tomar decisiones de autorización.

::: moniker-end

### <a name="log-out"></a>Cerrar sesión

::: moniker range=">= aspnetcore-2.1"

El vínculo de **cierre de sesión** invoca la acción `LogoutModel.OnPost`. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones del usuario almacenadas en una cookie.

Post se especifica en *pages/Shared/_LoginPartial. cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Al hacer clic en el vínculo **Cerrar sesión** , se llama a la acción `LogOut`.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   El código anterior llama al método `_signInManager.SignOutAsync`. El método `SignOutAsync` borra las notificaciones del usuario almacenadas en una cookie.

::: moniker-end

## <a name="test-identity"></a>Identidad de prueba

Las plantillas predeterminadas del proyecto web permiten el acceso anónimo a las páginas principales. Para probar la identidad, agregue [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página de privacidad.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Si ha iniciado sesión, cierre la sesión. Ejecute la aplicación y seleccione el vínculo de **privacidad** . Se le redirigirá a la página de inicio de sesión.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Explorar identidad

Para explorar la identidad con más detalle:

* [Crear origen de la interfaz de usuario de identidad completa](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Examine el origen de cada página y recorra el depurador.

::: moniker-end

## <a name="identity-components"></a>Componentes de identidad

::: moniker range=">= aspnetcore-2.1"

Todos los paquetes de NuGet que dependen de la identidad se incluyen en el [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

El paquete principal de identidad es [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Este paquete contiene el conjunto principal de interfaces para ASP.NET Core identidad y se incluye en `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migrar a ASP.NET Core identidad

Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, vea [migrar autenticación e identidad](xref:migration/identity).

## <a name="setting-password-strength"></a>Establecer la seguridad de la contraseña

Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos mínimos de contraseña.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
