---
title: Evitar ataques de redireccionamiento abierto en ASP.NET Core
author: ardalis
description: Se muestra cómo evitar los ataques de redireccionamiento abierto en una aplicación ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828875"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Evitar ataques de redireccionamiento abierto en ASP.NET Core

Una aplicación web que se redirige a una dirección URL que se especifica a través de la solicitud, como los datos de cadena de consulta o formulario potencialmente ser alterada para redirigir usuarios a una dirección URL externa, malintencionada. Esta modificación se llama a un ataque de redireccionamiento abierto.

Cada vez que la lógica de aplicación redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado. ASP.NET Core tiene funcionalidad integrada para ayudar a proteger las aplicaciones frente a ataques de redireccionamiento abierto (también conocido como abierta redirección).

## <a name="what-is-an-open-redirect-attack"></a>¿Qué es un ataque de redireccionamiento abierto?

Las aplicaciones Web con frecuencia redirigen a los usuarios a una página de inicio de sesión cuando acceden a recursos que requieren autenticación. La redirección normalmente incluye un `returnUrl` parámetro de cadena de consulta para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente. Después de que el usuario se autentica, le redirigirá a la dirección URL que tenían originalmente solicitada.

Dado que la dirección URL de destino se especifica en el elemento querystring de la solicitud, un usuario malintencionado podría alterar la cadena de consulta. Una cadena de consulta alterado podría permitir que el sitio redirigir al usuario a un sitio externo, malintencionado. Esta técnica se denomina un ataque de redirección (o la redirección) abierto.

### <a name="an-example-attack"></a>Un ataque de ejemplo

Un usuario malintencionado puede desarrollar un ataque diseñado para permitir el acceso de usuarios malintencionados a las credenciales de un usuario o información confidencial. Para iniciar el ataque, el usuario malintencionado convence al usuario que haga clic en un vínculo a la página de inicio de sesión de su sitio con un `returnUrl` querystring agregado valor a la dirección URL. Por ejemplo, considere la posibilidad de una aplicación en `contoso.com` que incluye una página de inicio de sesión en `http://contoso.com/Account/LogOn?returnUrl=/Home/About`. El ataque sigue estos pasos:

1. El usuario hace clic en un vínculo malintencionado a `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (la segunda dirección URL es "contoso**1**.com", no "contoso.com").
2. El usuario inicia sesión correctamente.
3. Se redirige al usuario (por el sitio) a `http://contoso1.com/Account/LogOn` (un sitio malintencionado que es exactamente igual que un sitio real).
4. El usuario inicia sesión de nuevo (lo que ofrece malintencionado sus credenciales de sitio) y se redirige al sitio real.

El usuario es probable que se considera que no se pudo su primer intento de iniciar sesión y que su segundo intento se realiza correctamente. Más probable es que el usuario sigue siendo consciente de que están en peligro sus credenciales.

![Proceso de ataques de redireccionamiento abierto](preventing-open-redirects/_static/open-redirection-attack-process.png)

Además de las páginas de inicio de sesión, algunos sitios proporcionan páginas de redireccionamiento o los puntos de conexión. Imagine que su aplicación tiene una página con una redirección abierta, `/Home/Redirect`. Un atacante podría crear, por ejemplo, un vínculo en un correo electrónico que se dirige a `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Un usuario típico consultará la dirección URL y que empieza con el nombre del sitio. Confiar en, hará clic en el vínculo. El redireccionamiento abierto, a continuación, enviaría el usuario para el sitio de suplantación de identidad, que es idéntico a la suya, y probablemente lo haría el usuario inicie sesión en lo creemos que es su sitio.

## <a name="protecting-against-open-redirect-attacks"></a>Protección contra ataques de redireccionamiento abierto

Al desarrollar aplicaciones web, tratar todos los datos proporcionados por el usuario como no es de confianza. Si la aplicación tiene la funcionalidad que se redirige al usuario según el contenido de la dirección URL, asegúrese de que estas redirecciones solo se realizan localmente dentro de la aplicación (o a una dirección URL conocida, no en cualquier dirección URL que se puede proporcionar en la cadena de consulta).

### <a name="localredirect"></a>LocalRedirect

Use la `LocalRedirect` método auxiliar de la base de `Controller` clase:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` se iniciará una excepción si se especifica una URL no locales. En caso contrario, se comporta igual que el `Redirect` método.

### <a name="islocalurl"></a>IsLocalUrl

Use la [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método para probar las direcciones URL antes de redirigir:

El ejemplo siguiente muestra cómo comprobar si una dirección URL es local antes de redirigir.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

El `IsLocalUrl` método protege a los usuarios sin darse cuenta que se redirija a un sitio malintencionado. Puede registrar los detalles de la dirección URL que se proporcionó cuando se proporciona una dirección URL de no local en una situación donde se espera una dirección URL local. Registro redirigir direcciones URL pueden ayudar a diagnosticar los ataques de redireccionamiento.
