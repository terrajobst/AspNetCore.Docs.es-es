---
title: Esquemas de directivas en ASP.NET Core
author: rick-anderson
description: Esquemas de autenticación de directiva que sea más fácil tener un esquema de autenticación lógico único
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346781"
---
# <a name="policy-schemes-in-aspnet-core"></a>Esquemas de directivas en ASP.NET Core

Esquemas de la directiva de autenticación facilitan tiene un esquema de autenticación lógico único potencialmente usar varios enfoques. Por ejemplo, un esquema de la directiva podría usar autenticación de Google para enfrentar los desafíos y autenticación de cookies para todo lo demás. Esquemas de autenticación de directiva hacen que sea:

* Fácil reenviar cualquier acción de autenticación a otro esquema.
* Avance de manera dinámica basándose en la solicitud.

Todos los esquemas de autenticación que utilice derivados <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> y asociado [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Son automáticamente combinaciones de directivas en ASP.NET Core 2.1 y versiones posteriores.
* Puede habilitarse a través de configuración de las opciones del esquema.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Ejemplos

El ejemplo siguiente muestra un esquema de nivel superior que combina los esquemas de nivel inferior. Se utiliza la autenticación de Google para enfrentar los desafíos y se usa la autenticación de cookies para todo lo demás:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

El ejemplo siguiente habilita la selección dinámica de esquemas en función de la solicitud. Es decir, cómo se combinan las cookies y la autenticación de API.

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
