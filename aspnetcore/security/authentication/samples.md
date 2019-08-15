---
title: Ejemplos de autenticación para ASP.NET Core
author: rick-anderson
description: Proporciona vínculos a los ejemplos de autenticación en el repositorio de ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: efa177245faceddad4eb80de9e6f6d38e1a4261c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022419"
---
# <a name="authentication-samples-for-aspnet-core"></a>Ejemplos de autenticación para ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

El [repositorio ASP.net Core](https://github.com/aspnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :

* [Transformación de notificaciones](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticación de cookies](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Proveedor de directivas personalizadas: IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opciones y esquemas de autenticación dinámica](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Notificaciones externas](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Selección entre cookies y otro esquema de autenticación en función de la solicitud](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restringe el acceso a los archivos estáticos](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Ejecutar los ejemplos

* Seleccione una [rama](https://github.com/aspnet/AspNetCore). Por ejemplo, `Tag:v3.0.0`
* Clone o descargue el [repositorio de ASP.net Core](https://github.com/aspnet/AspNetCore).
* Compruebe que ha instalado la versión [SDK de .net Core](https://www.microsoft.com/net/download/all) que coincide con el clon del repositorio de ASP.net Core.
* Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo `dotnet run`con.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

El [repositorio ASP.net Core](https://github.com/aspnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :

* [Transformación de notificaciones](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticación de cookies](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Proveedor de directivas personalizadas: IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opciones y esquemas de autenticación dinámica](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Notificaciones externas](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Selección entre cookies y otro esquema de autenticación en función de la solicitud](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restringe el acceso a los archivos estáticos](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Ejecutar los ejemplos

* Seleccione una [rama](https://github.com/aspnet/AspNetCore). Por ejemplo, `release/2.2`
* Clone o descargue el [repositorio de ASP.net Core](https://github.com/aspnet/AspNetCore).
* Compruebe que ha instalado la versión [SDK de .net Core](https://www.microsoft.com/net/download/all) que coincide con el clon del repositorio de ASP.net Core.
* Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo `dotnet run`con.

::: moniker-end
