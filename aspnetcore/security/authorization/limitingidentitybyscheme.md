---
title: "Limitación de identidad por esquema"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d3d6ca1b-b4b5-4bf7-898e-dcd90ec1bf8c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 2483c441da317a5c29b611b3a4910eae3c01fd7a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="limiting-identity-by-scheme"></a>Limitación de identidad por esquema

<a name=security-authorization-limiting-by-scheme></a>

En algunos escenarios, como aplicaciones de una página solo es posible acabar con varios métodos de autenticación. Por ejemplo, la aplicación puede usar la autenticación basada en cookies para iniciar sesión y autenticación del portador para las solicitudes de JavaScript. En algunos casos puede tener varias instancias de un middleware de autenticación. Por ejemplo, dos cookie middlewares donde uno contiene una identidad básica y se crea uno cuando se desencadene una autenticación multifactor porque el usuario solicitó una operación que requiere seguridad adicional.

Se denominan esquemas de autenticación cuando middleware de autenticación se configura durante la autenticación, por ejemplo

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AuthenticationScheme = "Cookie",
    LoginPath = new PathString("/Account/Unauthorized/"),
    AccessDeniedPath = new PathString("/Account/Forbidden/"),
    AutomaticAuthenticate = false
});

app.UseBearerAuthentication(options =>
{
    options.AuthenticationScheme = "Bearer";
    options.AutomaticAuthenticate = false;
});
```

En esta configuración se han agregado dos middlewares de autenticación, uno para las cookies y otro para portador.

>[!NOTE]
>Al agregar varias middleware de autenticación, que debe asegurarse de que ningún middleware está configurado para ejecutarse automáticamente. Para ello, establezca el `AutomaticAuthenticate` opciones de propiedad en false. Si se realizan correctamente este filtrado por esquema no funcionará.

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>Seleccionar el esquema con el atributo Authorize

Ningún middleware de autenticación está configurado para ejecutar automáticamente y crear una identidad que debe, en el momento de la autorización elegir el middleware que se usará. Seleccione el middleware que desea autorizar con la forma más sencilla es usar el `ActiveAuthenticationSchemes` propiedad. Esta propiedad acepta una lista delimitada por comas de los esquemas de autenticación a utilizar. Por ejemplo,

```csharp
[Authorize(ActiveAuthenticationSchemes = "Cookie,Bearer")]
public class MixedController : Controller
```

En el ejemplo anterior, la cookie y el portador middlewares ejecutará y tiene una oportunidad para crear y agregar una identidad para el usuario actual. Mediante la especificación de un único esquema solo el middleware especificado se ejecutará;

```csharp
[Authorize(ActiveAuthenticationSchemes = "Bearer")]
```

En este caso se ejecutaría solo del middleware con el esquema de portador, y se ignorará cualquier identidad basado en cookies.

## <a name="selecting-the-scheme-with-policies"></a>Seleccionar el esquema con directivas

Si desea especificar los esquemas deseados en [directiva](policies.md#security-authorization-policies-based) puede establecer el `AuthenticationSchemes` colección al agregar la directiva.

```csharp
options.AddPolicy("Over18", policy =>
{
    policy.AuthenticationSchemes.Add("Bearer");
    policy.RequireAuthenticatedUser();
    policy.Requirements.Add(new Over18Requirement());
});
```

En este ejemplo el Over18 directiva solo se ejecutará con la identidad creada por el `Bearer` middleware.
