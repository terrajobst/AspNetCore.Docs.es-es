---
title: Ejemplos de autenticación para ASP.NET Core
author: rick-anderson
description: Proporciona vínculos a los ejemplos de autenticación en el repositorio de ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511618"
---
# <a name="authentication-samples-for-aspnet-core"></a>Ejemplos de autenticación para ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

El [repositorio ASP.net Core](https://github.com/dotnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :

* [Transformación de notificaciones](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Autenticación de cookies](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [Proveedor de directivas personalizadas: IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [Opciones y esquemas de autenticación dinámica](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [Notificaciones externas](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [Selección entre cookies y otro esquema de autenticación en función de la solicitud](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [Restringe el acceso a los archivos estáticos](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Ejecución de las muestras

* Seleccione una [rama](https://github.com/dotnet/AspNetCore). Por ejemplo: `Tag:v3.0.0`
* Clone o descargue el [repositorio de ASP.net Core](https://github.com/dotnet/AspNetCore).
* Compruebe que ha instalado la versión [SDK de .net Core](https://dotnet.microsoft.com/download/dotnet-core) que coincide con el clon del repositorio de ASP.net Core.
* Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo con `dotnet run`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

El [repositorio ASP.net Core](https://github.com/dotnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :

* [Transformación de notificaciones](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticación de cookies](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Proveedor de directivas personalizadas: IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opciones y esquemas de autenticación dinámica](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Notificaciones externas](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Selección entre cookies y otro esquema de autenticación en función de la solicitud](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restringe el acceso a los archivos estáticos](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Ejecución de las muestras

* Seleccione una [rama](https://github.com/dotnet/AspNetCore). Por ejemplo: `release/2.2`
* Clone o descargue el [repositorio de ASP.net Core](https://github.com/dotnet/AspNetCore).
* Compruebe que ha instalado la versión [SDK de .net Core](https://dotnet.microsoft.com/download/dotnet-core) que coincide con el clon del repositorio de ASP.net Core.
* Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo con `dotnet run`.

::: moniker-end
