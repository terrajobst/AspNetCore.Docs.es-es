---
title: Ejemplos de autenticación para ASP.NET Core
author: rick-anderson
description: Proporciona vínculos a los ejemplos de autenticación en el repositorio de ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236683"
---
# <a name="authentication-samples-for-aspnet-core"></a>Ejemplos de autenticación para ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El [repositorio de ASP.NET Core](https://github.com/aspnet/AspNetCore) contiene los siguientes ejemplos de autenticación en el *AspNetCore/src/seguridad/samples* carpeta:

* [Transformación de notificaciones](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Autenticación con cookies](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Proveedor de directivas personalizadas - IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Opciones y los esquemas de autenticación dinámico](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Notificaciones externas](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Seleccionar entre la cookie y otro esquema de autenticación basada en la solicitud](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restringe el acceso a los archivos estáticos](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Ejecutar los ejemplos

* Seleccione un [rama](https://github.com/aspnet/AspNetCore). Por ejemplo, `release/2.2`.
* Clone o descargue el [repositorio de ASP.NET Core](https://github.com/aspnet/AspNetCore).
* Compruebe que tiene instalada la [SDK de .NET Core](https://www.microsoft.com/net/download/all) versión que coincida con la clonación del repositorio de ASP.NET Core.
* Navegue a un ejemplo en *AspNetCore/src/seguridad/samples* y ejecutar el ejemplo con `dotnet run`.
