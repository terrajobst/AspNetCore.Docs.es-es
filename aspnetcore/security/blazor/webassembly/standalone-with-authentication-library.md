---
title: Protección de un ASP.NET Core Blazor aplicación independiente webassembly con la biblioteca de autenticación
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083594"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>Protección de un ASP.NET Core Blazor aplicación independiente webassembly con la biblioteca de autenticación

Por [Javier Calvarro Nelson](https://github.com/javiercn) y [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Para crear una Blazor aplicación independiente webassembly que use `Microsoft.AspNetCore.Components.WebAssembly.Authentication` Library, ejecute el siguiente comando en un shell de comandos:

```dotnetcli
dotnet new blazorwasm -au Individual
```

Para especificar la ubicación de salida, que crea una carpeta de proyecto si no existe, incluya la opción output en el comando con una ruta de acceso (por ejemplo, `-o BlazorSample`). El nombre de la carpeta también se convierte en parte del nombre del proyecto.

En Visual Studio, [cree una aplicación Blazor Webassembly](xref:blazor/get-started). Establezca la **autenticación** en **cuentas de usuario individuales** con la opción **almacenar cuentas de usuario en la aplicación** .

## <a name="authentication-package"></a>Paquete de autenticación

Cuando se crea una aplicación para usar cuentas de usuario individuales, la aplicación recibe automáticamente una referencia de paquete para el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication` en el archivo de proyecto de la aplicación. El paquete proporciona un conjunto de primitivas que ayudan a la aplicación a autenticar a los usuarios y obtener tokens para llamar a las API protegidas.

Si agrega la autenticación a una aplicación, agregue manualmente el paquete al archivo de proyecto de la aplicación:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Reemplace `{VERSION}` en la referencia de paquete anterior por la versión del paquete de `Microsoft.AspNetCore.Blazor.Templates` que se muestra en el artículo <xref:blazor/get-started>.

## <a name="authentication-service-support"></a>Compatibilidad con el servicio de autenticación

La compatibilidad con la autenticación de usuarios se registra en el contenedor de servicios con el método de extensión `AddOidcAuthentication` proporcionado por el paquete de `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Este método configura todos los servicios necesarios para que la aplicación interactúe con el proveedor de identidades (IP).

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

La compatibilidad con la autenticación para aplicaciones independientes se ofrece mediante Open ID Connect (OIDC). El método `AddOidcAuthentication` acepta una devolución de llamada para configurar los parámetros necesarios para autenticar una aplicación mediante OIDC. Los valores necesarios para configurar la aplicación pueden obtenerse a partir de la dirección IP, como Google, Microsoft u otro proveedor compatible con OIDC. Obtenga los valores al registrar la aplicación, que suele producirse en su portal en línea.

## <a name="index-page"></a>Página de índice

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a>Componente de aplicación

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>Componente de autenticación

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
