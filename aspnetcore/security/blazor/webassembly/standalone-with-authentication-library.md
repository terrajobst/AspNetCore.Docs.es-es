---
title: Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con la biblioteca de autenticación
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977046"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Proteja una Blazor aplicación independiente de ASP.NET Core WebAssembly con la biblioteca de autenticación

Por [Javier Calvarro Nelson](https://github.com/javiercn) y Luke [Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

*Para Azure Active Directory (AAD) y Azure Active Directory B2C (AAD B2C), no siga las instrucciones de este tema. Consulte los temas AAD y AAD B2C en esta tabla de nodo de contenido.*

Para crear Blazor una aplicación independiente `Microsoft.AspNetCore.Components.WebAssembly.Authentication` de WebAssembly que utilice la biblioteca, ejecute el siguiente comando en un shell de comandos:

```dotnetcli
dotnet new blazorwasm -au Individual
```

Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, `-o BlazorSample`incluya la opción de salida en el comando con una ruta de acceso (por ejemplo, ). El nombre de la carpeta también pasa a formar parte del nombre del proyecto.

En Visual Studio, [cree una Blazor aplicación WebAssembly](xref:blazor/get-started). Establezca **Autenticación** en Cuentas de **usuario individuales** con la opción **Almacenar cuentas de usuario en la aplicación.**

## <a name="authentication-package"></a>Paquete de autenticación

Cuando se crea una aplicación para usar cuentas de usuario `Microsoft.AspNetCore.Components.WebAssembly.Authentication` individuales, la aplicación recibe automáticamente una referencia de paquete para el paquete en el archivo de proyecto de la aplicación. El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.

Si agrega autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Reemplace `{VERSION}` en la referencia del paquete `Microsoft.AspNetCore.Blazor.Templates` anterior por <xref:blazor/get-started> la versión del paquete que se muestra en el artículo.

## <a name="authentication-service-support"></a>Soporte de servicio de autenticación

La compatibilidad con la autenticación de usuarios `AddOidcAuthentication` se registra `Microsoft.AspNetCore.Components.WebAssembly.Authentication` en el contenedor de servicios con el método de extensión proporcionado por el paquete. Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

La compatibilidad de autenticación para aplicaciones independientes se ofrece mediante Open ID Connect (OIDC). El `AddOidcAuthentication` método acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación mediante OIDC. Los valores necesarios para configurar la aplicación se pueden obtener de la dirección IP compatible con OIDC. Obtenga los valores al registrar la aplicación, que normalmente se produce en su portal en línea.

## <a name="access-token-scopes"></a>Acceso a ámbitos de token

La Blazor plantilla WebAssembly no configura automáticamente la aplicación para solicitar un token de acceso para una API segura. Para aprovisionar un token como parte del flujo de inicio de sesión, agregue el ámbito a los ámbitos de token predeterminados `OidcProviderOptions`de:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
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
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

Para obtener más información, vea <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>.

## <a name="imports-file"></a>Archivo de importaciones

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>Página de índice

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

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
