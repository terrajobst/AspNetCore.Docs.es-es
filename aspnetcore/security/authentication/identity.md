---
title: Introducción a la identidad en ASP.NET Core
author: rick-anderson
description: Usar identidad con una aplicación ASP.NET Core. Obtenga información sobre cómo establecer los requisitos de contraseña (RequireDigit, RequiredLength, RequiredUniqueChars etc.).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: af07adcc7f9513845bb91eb233f0a9840e1bd6f4
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749313"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introducción a la identidad en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Identity es un sistema de pertenencia que agrega la funcionalidad de inicio de sesión a las aplicaciones ASP.NET Core. Los usuarios pueden crear una cuenta con la información de inicio de sesión almacenada en la identidad o pueden utilizar un proveedor de inicio de sesión externo. Proveedores de inicio de sesión externos compatibles incluyen [Facebook, Google, Twitter y Microsoft Account](xref:security/authentication/social/index).

Identidad puede configurarse con una base de datos de SQL Server para almacenar los nombres de usuario, contraseñas y datos de perfil. Como alternativa, otro almacén persistente puede usarse, por ejemplo, Azure Table Storage.

[Ver o descargar el código de ejemplo.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Cómo descargar)](xref:tutorials/index#how-to-download-a-sample)

En este tema, aprenda a usar identidad para registrar, inicie sesión y cerrar la sesión un usuario. Para obtener instrucciones detalladas sobre cómo crear aplicaciones que usan la identidad, vea la sección pasos siguientes al final de este artículo.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity y AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) se introdujo en ASP. Core 2.1. Una llamada a `AddDefaultIdentity` es similar a llamar a lo siguiente:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Consulte [AddDefaultIdentity origen](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) para obtener más información.

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Creación de una aplicación Web con autenticación

Cree un proyecto de aplicación Web ASP.NET Core con cuentas de usuario individuales.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Seleccione **Archivo** > **Nuevo** > **Proyecto**. 
* Seleccione **Aplicación web de ASP.NET Core**. Denomine el proyecto **WebApp1** tener el mismo espacio de nombres como la descarga del proyecto. Haga clic en **Aceptar**.
* Seleccione una de ASP.NET Core **aplicación Web** para ASP.NET Core 2.1, a continuación, seleccione **Cambiar autenticación**.
* Seleccione **cuentas de usuario individuales** y haga clic en **Aceptar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Proporciona el proyecto generado [ASP.NET Core Identity](xref:security/authentication/identity) como un [biblioteca de clases de Razor](xref:razor-pages/ui-class).

### <a name="test-register-and-login"></a>Inicio de sesión y de registro de prueba

Ejecute la aplicación y registrar un usuario. Según el tamaño de pantalla, es posible que deba seleccionar el botón de alternancia de navegación para ver el **registrar** y **inicio de sesión** vínculos.

![botón de Alternar barra de navegación](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a>Configurar servicios de identidad

Se agregan los servicios en `ConfigureServices`. El patrón habitual consiste en llamar a todos los métodos `Add{Service}` y, luego, a todos los métodos `services.Configure{Service}`. El código siguiente no incluye la plantilla genera `CookiePolicyOptions`:

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

El código anterior configura la identidad con valores de la opción predeterminada. Los servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).

   Se habilita la identidad mediante una llamada a [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` Agrega autenticación [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Los servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).

   Se habilita la identidad de la aplicación mediante una llamada a `UseAuthentication` en el `Configure` método. `UseAuthentication` Agrega autenticación [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Estos servicios están disponibles para la aplicación a través de [inserción de dependencias](xref:fundamentals/dependency-injection).

   Se habilita la identidad de la aplicación mediante una llamada a `UseIdentity` en el `Configure` método. `UseIdentity` Agrega la autenticación basada en cookies [middleware](xref:fundamentals/middleware/index) a la canalización de solicitud.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Para obtener más información, consulte el [IdentityOptions clase](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) y [inicio de la aplicación](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Aplicar la técnica scaffolding registrar, inicio de sesión y cierre de sesión

Siga el [aplicar la técnica scaffolding en un proyecto de Razor sin autorización identidad](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrucciones.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Agregue los archivos de registro, inicio de sesión y cierre de sesión.


# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Si ha creado el proyecto con el nombre **WebApp1**, ejecute los siguientes comandos. En caso contrario, use el espacio de nombres correcto para el `ApplicationDbContext`:


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

PowerShell usa el punto y coma como separador de comandos. Cuando se usa PowerShell, el punto y coma en la lista de archivos de escape o coloque la lista de archivos en las comillas dobles, como se muestra en el ejemplo anterior.

---

### <a name="examine-register"></a>Examine el registro

::: moniker range=">= aspnetcore-2.1"

   Cuando un usuario hace clic en el **registrar** vínculo, el `RegisterModel.OnPostAsync` se invoca la acción. El usuario se crea por [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) en el `_userManager` objeto. `_userManager` se proporciona mediante la inserción de dependencias):

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   Cuando un usuario hace clic en el **registrar** vínculo, el `Register` acción se invoca en `AccountController`. El `Register` acción crea el usuario mediante una llamada a `CreateAsync` en el `_userManager` objeto (proporcionado a `AccountController` mediante la inserción de dependencias):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Si el usuario se creó correctamente, el usuario se registra en la llamada a `_signInManager.SignInAsync`.

   **Nota:** vea [cuenta confirmación](xref:security/authentication/accconfirm#prevent-login-at-registration) para conocer los pasos evitar que el inicio de sesión de inmediato en el registro.

### <a name="log-in"></a>Iniciar sesión

::: moniker range=">= aspnetcore-2.1"

Se muestra el formulario de inicio de sesión cuando:

* El **iniciarla** vínculo está seleccionado.
* Cuando un usuario accede a una página donde no se autentican **o** autorizado, se le redirigirá a la página de inicio de sesión. 

Cuando se envía el formulario de la página de inicio de sesión, el `OnPostAsync` se llama a la acción. `PasswordSignInAsync` se llama en el `_signInManager` objeto (proporcionado por la inserción de dependencias).

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   La base de `Controller` clase expone un `User` propiedad que se puede acceder desde los métodos de controlador. Por ejemplo, puede enumerar `User.Claims` y tomar decisiones de autorización. Para obtener más información, consulte [autorización](xref:security/authorization/index).

::: moniker-end
::: moniker range="= aspnetcore-2.0"

Se muestra el formulario de inicio de sesión cuando los usuarios seleccionan el **iniciarla** vincular o se le redirigirá al tener acceso a una página que requiere autenticación. Cuando el usuario envía el formulario en la página de inicio de sesión, el `AccountController` `Login` se llama a la acción.

El `Login` acción llamadas `PasswordSignInAsync` en el `_signInManager` objeto (proporcionado a `AccountController` mediante la inserción de dependencias).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

La base de (`Controller` o `PageModel`) clase expone un `User` propiedad. Por ejemplo, `User.Claims` pueden enumerarse para tomar decisiones de autorización.

::: moniker-end

### <a name="log-out"></a>Cerrar sesión

::: moniker range=">= aspnetcore-2.1"

El **cerrar sesión** vínculo invoca el `LogoutModel.OnPost` acción. 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) borra las notificaciones de usuario almacenadas en una cookie. No redirigir después de llamar a `SignOutAsync` o que el usuario **no** cerrar la sesión.

POST se especifica en el *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   Al hacer clic en el **cerrar sesión** llamadas de vincular el `LogOut` acción.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   El código anterior llama el `_signInManager.SignOutAsync` método. El `SignOutAsync` método borra las notificaciones de usuario almacenadas en una cookie.
::: moniker-end

## <a name="test-identity"></a>Comprobar la identidad

Las plantillas de proyecto de web predeterminada permiten accesos anónimos a las páginas principales. Para probar la identidad, agregue [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) a la página About.

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

Si ha iniciado sesión, cerrar sesión. Ejecute la aplicación y seleccione el **sobre** vínculo. Se le redirigirá a la página de inicio de sesión.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Explore la identidad

Para explorar la identidad con más detalle:

* [Crear origen de la interfaz de usuario de identidad completa](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Examine el origen de cada página y paso a través del depurador.

::: moniker-end

## <a name="identity-components"></a>Componentes de identidad

::: moniker range=">= aspnetcore-2.1"

Todas la identidad dependientes paquetes de NuGet se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).
::: moniker-end

El paquete principal de identidad está [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Este paquete contiene el conjunto principal de interfaces de ASP.NET Core Identity y se incluye de forma `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migración a ASP.NET Core Identity

Para obtener más información e instrucciones sobre cómo migrar el almacén de identidades existente, consulte [migrar autenticación e identidad](xref:migration/identity).

## <a name="setting-password-strength"></a>Configuración de seguridad de la contraseña

Consulte [configuración](#pw) para obtener un ejemplo que establece los requisitos de contraseña mínima.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* [Configurar el tipo de datos de las claves principales de identidad](xref:security/authentication/identity-primary-key-configuration).
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
