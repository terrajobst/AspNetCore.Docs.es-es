---
title: Esquemas de directiva en ASP.NET Core
author: rick-anderson
description: Los esquemas de directivas de autenticación facilitan el uso de un único esquema de autenticación lógica
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652355"
---
# <a name="policy-schemes-in-aspnet-core"></a>Esquemas de directiva en ASP.NET Core

Los esquemas de directivas de autenticación facilitan que un único esquema de autenticación lógica use varios enfoques. Por ejemplo, un esquema de directiva podría usar la autenticación de Google para desafíos y la autenticación de cookies para todo lo demás. Los esquemas de la Directiva de autenticación lo hacen:

* Es fácil reenviar cualquier acción de autenticación a otro esquema.
* Reenviar dinámicamente según la solicitud.

Todos los esquemas de autenticación que usan <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> derivadas y las [\<ciones de AuthenticationHandler asociadas >](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Son esquemas de directivas automáticamente en ASP.NET Core 2,1 y versiones posteriores.
* Se puede habilitar mediante la configuración de las opciones del esquema.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra un esquema de nivel superior que combina esquemas de nivel inferior. La autenticación de Google se usa para los desafíos y la autenticación de cookies se usa para todo lo demás:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

En el ejemplo siguiente se habilita la selección dinámica de esquemas por solicitud. Es decir, cómo mezclar cookies y la autenticación de API:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
