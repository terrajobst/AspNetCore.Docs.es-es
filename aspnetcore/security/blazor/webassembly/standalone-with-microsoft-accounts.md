---
title: Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con cuentas De Microsoft
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977085"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con cuentas De Microsoft

Por [Javier Calvarro Nelson](https://github.com/javiercn) y Luke [Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Para crear Blazor una aplicación independiente de WebAssembly que use [cuentas de Microsoft con Azure Active Directory (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal) para la autenticación:

1. [Crear un inquilino de AAD y una aplicación web](/azure/active-directory/develop/v2-overview)

   Registre una aplicación de AAD en el área**registros** de aplicaciones de **Azure Active Directory** > de Azure Portal:

   1\. Proporcione un **nombre** para la aplicación (por ejemplo, ** Blazor Client AAD**).<br>
   2\. En **Tipos de cuenta scompatibles**, seleccione **Cuentas en cualquier directorio de organización**.<br>
   3\. Deje el descenso desplegable URI de **redirección** establecido `https://localhost:5001/authentication/login-callback`en **Web**y proporcione un URI de redireccionamiento de .<br>
   4\. Deshabilite la casilla de verificación **Permisos** > **de concesión de permisos para abrir y offline_access permisos.**<br>
   5\. Seleccione **Registrar**.

   En la**Web****de configuraciones** > de la plataforma de **autenticación:** > 

   1\. Confirme que el `https://localhost:5001/authentication/login-callback` URI de **redireccionamiento** está presente.<br>
   2\. En **Implicit grant**, active las casillas de los tokens de **acceso** y los tokens **de identificador.**<br>
   3\. Los valores predeterminados restantes de la aplicación son aceptables para esta experiencia.<br>
   4\. Seleccione el botón **Guardar**.

   Registre el ID de aplicación (ID `11111111-1111-1111-1111-111111111111`de cliente) (por ejemplo, ).

1. Reemplace los marcadores de posición del siguiente comando por la información registrada anteriormente y ejecute el comando en un shell de comandos:

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, `-o BlazorSample`incluya la opción de salida en el comando con una ruta de acceso (por ejemplo, ). El nombre de la carpeta también pasa a formar parte del nombre del proyecto.

Después de crear la aplicación, debería ser capaz de:

* Inicie sesión en la aplicación con una cuenta Microsoft.
* Solicite tokens de acceso para las API de Blazor Microsoft con el mismo enfoque que para las aplicaciones independientes siempre que haya configurado la aplicación correctamente. Para obtener más información, consulte [Inicio rápido: configurar una aplicación para exponer las API web.](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)

## <a name="authentication-package"></a>Paquete de autenticación

Cuando se crea una aplicación para`SingleOrg`usar cuentas de trabajo o educativas (`Microsoft.Authentication.WebAssembly.Msal`), la aplicación recibe automáticamente una referencia de paquete para la biblioteca de autenticación de [Microsoft](/azure/active-directory/develop/msal-overview) ( ). El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.

Si agrega autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Reemplace `{VERSION}` en la referencia del paquete `Microsoft.AspNetCore.Blazor.Templates` anterior por <xref:blazor/get-started> la versión del paquete que se muestra en el artículo.

El `Microsoft.Authentication.WebAssembly.Msal` paquete agrega transitivamente el `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paquete a la aplicación.

## <a name="authentication-service-support"></a>Soporte de servicio de autenticación

La compatibilidad con la autenticación de usuarios `AddMsalAuthentication` se registra `Microsoft.Authentication.WebAssembly.Msal` en el contenedor de servicios con el método de extensión proporcionado por el paquete. Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

El `AddMsalAuthentication` método acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación. Los valores necesarios para configurar la aplicación se pueden obtener de la configuración de cuentas de Microsoft al registrar la aplicación.

## <a name="access-token-scopes"></a>Acceso a ámbitos de token

La Blazor plantilla WebAssembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura. Para aprovisionar un token como parte del flujo de inicio de sesión, `MsalProviderOptions`agregue el ámbito a los ámbitos de token de acceso predeterminados de:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> Si Azure Portal proporciona un URI de ámbito y **la aplicación produce una excepción no controlada** cuando recibe una respuesta *401 Unauthorized* de la API, intente usar un URI de ámbito que no incluya el esquema y el host. Por ejemplo, Azure Portal puede proporcionar uno de los siguientes formatos de URI de ámbito:
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> Proporcione el URI de ámbito sin el esquema y el host:
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

Para obtener más información, vea <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.

## <a name="imports-file"></a>Archivo de importaciones

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>Página de índice

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>Componente de la aplicación

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Componente de autenticación

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Recursos adicionales

* [Solicitar tokens de acceso adicionales](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [Inicio rápido: registre una aplicación con la plataforma de identidad de Microsoft](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [Inicio rápido: configure una aplicación para exponer las API web](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
