---
title: Evitar los ataques de redirección abierta en ASP.NET Core
author: ardalis
description: Muestra cómo evitar los ataques de redirección abierta en una aplicación de ASP.NET Core
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: 9ac6b311170dbbc27dd388842c071bc64add6f08
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Evitar los ataques de redirección abierta en ASP.NET Core

Una aplicación web que redirija a una dirección URL que se especifica a través de la solicitud como la cadena de consulta o un formulario de datos potencialmente puede alterada para redirigir a los usuarios a una dirección URL externa, malintencionada. Esta modificación se llama a un ataque de redirección abierta.

Cada vez que la lógica de aplicación se redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado. ASP.NET Core tiene funcionalidad integrada para ayudar a proteger las aplicaciones frente a ataques de redirección abierta (también conocido como abrir redirección).

## <a name="what-is-an-open-redirect-attack"></a>¿Qué es un ataque de redirección abierta?

Las aplicaciones Web con frecuencia redirección a los usuarios a una página de inicio de sesión cuando accedan a los recursos que requieren autenticación. La redirección typlically incluye un `returnUrl` parámetro de cadena de consulta para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente. Después de que el usuario se autentica, se le redirige a la dirección URL que tenían originalmente solicitada.

Dado que la dirección URL de destino se especifica en la cadena de consulta de la solicitud, un usuario malintencionado podría manipular la cadena de consulta. Una cadena de consulta modificada podría permitir al sitio redirigir al usuario a un sitio externo, malintencionado. Esta técnica se denomina un ataque de redirección (o redirección) abierto.

### <a name="an-example-attack"></a>Un ataque de ejemplo

Un usuario malintencionado podría desarrollar un ataque diseñado para permitir el acceso de usuario malintencionado para las credenciales de un usuario o información confidencial en la aplicación. Para iniciar el ataque, convencer a los usuarios hacer clic en un vínculo a la página de inicio de sesión de su sitio, con un `returnUrl` valor cadena de consulta que se agrega a la dirección URL. Por ejemplo, el [NerdDinner.com](http://nerddinner.com) aplicación de ejemplo (escrito para ASP.NET MVC) incluye aquí tal una página de inicio de sesión: `http://nerddinner.com/Account/LogOn?returnUrl=/Home/About`. El ataque, a continuación, sigue estos pasos:

1. Usuario hace clic en un vínculo a `http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn` (tenga en cuenta, segunda dirección URL es nerddi**n**er, no nerddi**nn**er).
2. El usuario inicia sesión correctamente.
3. Se redirige al usuario (en el sitio) a `http://nerddiner.com/Account/LogOn` (sitio malintencionado que parece sitio real).
4. El usuario inicia sesión de nuevo (dando malintencionado sus credenciales de sitio) y se le redirige al sitio real.

El usuario es probable que cree su primer intento de iniciar sesión no se pudo y la otra se realizó correctamente. Probablemente permanecerá sin tener en cuenta sus credenciales se han visto comprometidas.

![Proceso de ataques de redirección abierta](preventing-open-redirects/_static/open-redirection-attack-process.png)

Además de las páginas de inicio de sesión, algunos sitios proporcionan páginas de redireccionamiento o puntos de conexión. Imagine que la aplicación tiene una página con una redirección abierta, `/Home/Redirect`. Un atacante podría crear, por ejemplo, un vínculo en un correo electrónico que se va a `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`. Un usuario típico en la dirección URL y saber que comienza con el nombre del sitio. Confiar en, hará clic en el vínculo. La redirección abierta enviaría a continuación, el usuario para el sitio de suplantación de identidad, cuya apariencia es idéntico a la suya, y es probable que lo haría el usuario inicie sesión en lo creen que es su sitio.

## <a name="protecting-against-open-redirect-attacks"></a>Protegerse contra los ataques de redirección abierta

Al desarrollar aplicaciones web, trate todos los datos proporcionados por el usuario que no es de confianza. Si la aplicación dispone de funcionalidad que redirige al usuario según el contenido de la dirección URL, asegúrese de que estas redirecciones solo se realizan localmente dentro de la aplicación (o a una dirección URL conocida, no cualquier dirección URL que puede especificarse en la cadena de consulta).

### <a name="localredirect"></a>LocalRedirect

Use la `LocalRedirect` método auxiliar de la base de `Controller` clase:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` se iniciará una excepción si se especifica una dirección URL no locales. En caso contrario, se comporta igual que el `Redirect` método.

### <a name="islocalurl"></a>IsLocalUrl

Use la [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método para probar las direcciones URL antes de redirigir:

En el ejemplo siguiente se muestra cómo comprobar si una dirección URL es local antes de redirigir.

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

El `IsLocalUrl` método protege a los usuarios sin darse cuenta sea redirigido a un sitio malintencionado. Puede registrar los detalles de la dirección URL que se proporcionó cuando se proporciona una dirección URL no es local en una situación donde se espera una dirección URL local. Registro de redirección de direcciones URL pueden ayudar a diagnosticar los ataques de redirección.
