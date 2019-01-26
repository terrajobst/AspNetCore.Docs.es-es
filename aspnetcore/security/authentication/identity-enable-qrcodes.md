---
title: Habilitar la generación de código QR para aplicaciones de authenticator TOTP en ASP.NET Core
author: rick-anderson
description: Descubra cómo habilitar la generación de código QR para las aplicaciones de autenticador TOTP que funcionan con la autenticación en dos fases principales de ASP.NET.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf99cc21a7a1bb4d01c7cc092106d23375a1a76f
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073132"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Habilitar la generación de código QR para aplicaciones de authenticator TOTP en ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Códigos QR requiere ASP.NET Core 2.0 o posterior.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core se distribuye con el soporte técnico para las aplicaciones de authenticator para la autenticación individual. Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector. 2FA uso TOTP es preferible a SMS 2FA. Una aplicación authenticator proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña. Normalmente, una aplicación de autenticador está instalada en un Smartphone.

Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan compatibilidad para la generación de CódigoQR. Los generadores de CódigoQR facilitan la instalación de 2FA. Este documento le guiará a través de agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración 2FA.

Autenticación en dos fases no se realiza mediante un proveedor de autenticación externo, como [Google](xref:security/authentication/google-logins) o [Facebook](xref:security/authentication/facebook-logins). Inicios de sesión externos están protegidos mediante cualquier mecanismo que proporciona el proveedor de inicio de sesión externo. Considere, por ejemplo, el [Microsoft](xref:security/authentication/microsoft-logins) proveedor de autenticación requiere una clave de hardware u otro enfoque de 2FA. Si las plantillas predeterminadas aplican 2FA "local", a continuación, los usuarios se necesitarían para satisfacer dos enfoques 2FA, que no es un escenario frecuente.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Agregar códigos QR en la página de configuración de 2FA

Estas instrucciones usan *qrcode.js* desde el https://davidshimjs.github.io/qrcodejs/ repositorio.

* Descargue el [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Siga las instrucciones de [Scaffold identidad](xref:security/authentication/scaffold-identity) para generar */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* En */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, busque el `Scripts` sección al final del archivo:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* En *Pages/Account/Manage/EnableAuthenticator.cshtml* (las páginas de Razor) o *Views/Manage/EnableAuthenticator.cshtml* (MVC), busque el `Scripts` sección al final del archivo:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR. Debe tener el aspecto siguiente:

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

Ejecute la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.

## <a name="change-the-site-name-in-the-qr-code"></a>Cambiar el nombre del sitio en el código QR

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

El nombre del sitio en el código QR se toma del nombre del proyecto que elige al crear inicialmente el proyecto. Puede cambiar mediante la búsqueda de la `GenerateQrCodeUri(string email, string unformattedKey)` método en el */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

El nombre del sitio en el código QR se toma del nombre del proyecto que elige al crear inicialmente el proyecto. Puede cambiar mediante la búsqueda de la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* archivo (las páginas de Razor) o el *Controllers/ManageController.cs* archivo (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

El código predeterminado de la plantilla tiene el aspecto siguiente:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

El segundo parámetro en la llamada a `string.Format` es el nombre del sitio procedente de su nombre de la solución. Se puede cambiar a cualquier valor, pero siempre debe estar codificado como URL.

## <a name="using-a-different-qr-code-library"></a>Uso de una biblioteca de código QR diferente

Puede reemplazar la biblioteca de código QR con la biblioteca preferida. El código HTML contiene un `qrCode` elemento donde se puede colocar un código QR mediante cualquier mecanismo de la biblioteca proporciona.

La dirección URL con el formato correcto para el código QR está disponible en el:

* `AuthenticatorUri` propiedad del modelo.
* `data-url` propiedad en el `qrCodeData` elemento.

## <a name="totp-client-and-server-time-skew"></a>TOTP cliente y servidor desfase horario

Autenticación TOTP (basado en tiempo de contraseña de un solo uso) depende del dispositivo con el servidor y el autenticador tiene una hora precisa. Los tokens solo duran 30 segundos. Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.

::: moniker-end
