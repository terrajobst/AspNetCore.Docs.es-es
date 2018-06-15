---
title: Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core
author: rick-anderson
description: Descubra cómo habilitar la generación de código QR para aplicaciones de autenticador que funcionan con la autenticación de dos factores principales de ASP.NET.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 463c1c7b3aef624622e34943f1a7a518e658a037
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613039"
---
# <a name="enable-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core

Nota: En este tema se aplica a ASP.NET Core 2.x

ASP.NET Core se suministra con compatibilidad para las aplicaciones de autenticador para la autenticación individual. Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración única contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector. 2FA uso TOTP es preferible a 2FA SMS. Una aplicación de autenticador proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña. Normalmente, una aplicación autenticadora está instalada en un Smartphone.

Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan una compatibilidad para la generación de CódigoQR. Generadores de CódigoQR facilitan la configuración de 2FA. Este documento le ayudará a agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración de 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Agregar códigos QR a la página de configuración de 2FA

Utilizan estas instrucciones *qrcode.js* de la https://davidshimjs.github.io/qrcodejs/ repo.

* Descargue el [qrcode.js javascript biblioteca](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.

* En *Pages\Account\Manage\EnableAuthenticator.cshtml* (las páginas de Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), busque la `Scripts` sección al final del archivo:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR. Debería ser como sigue:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Eliminar el párrafo que proporciona vínculos a estas instrucciones.

Ejecutar la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.

## <a name="change-the-site-name-in-the-qr-code"></a>Cambiar el nombre del sitio en el código QR

El nombre del sitio en el código QR se toma del nombre del proyecto que elegir al crear inicialmente el proyecto. Puede cambiarla si se busca la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* archivo (las páginas de Razor) o la *Controllers\ManageController.cs* archivo (MVC). 

El código predeterminado de la plantilla tiene el siguiente aspecto:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

El segundo parámetro en la llamada a `string.Format` es el nombre de sitio, tomado de su nombre de la solución. Se puede cambiar a cualquier valor, pero debe ser siempre dirección URL codificada.

## <a name="using-a-different-qr-code-library"></a>Utilizar una biblioteca de código QR diferente

Puede reemplazar la biblioteca de código QR por la biblioteca preferida. El código HTML contiene un `qrCode` proporciona la biblioteca de elemento que se puede colocar un código QR mediante cualquier mecanismo.

La dirección URL con el formato correcto para el código QR está disponible en el:

* `AuthenticatorUri` propiedad del modelo.
* `data-url` propiedad en el `qrCodeData` elemento. 

## <a name="totp-client-and-server-time-skew"></a>TOTP cliente y servidor sesgo horario

Autenticación de TOTP (basado en tiempo la contraseña de un solo uso) depende de dispositivo con el servidor y el autenticador tiene una hora precisa. Símbolos (tokens) solo duran durante 30 segundos. Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.
