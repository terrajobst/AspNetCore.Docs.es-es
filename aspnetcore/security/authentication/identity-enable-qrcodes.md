---
title: Habilitar la generación de código QR para aplicaciones de autenticador TOTP en ASP.NET Core
author: rick-anderson
description: Descubra cómo habilitar la generación de código QR para TOTP autenticador aplicaciones que funcionan con la autenticación en dos fases principales de ASP.NET.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/24/2017
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: b0d8f104119340b97bd65f1826bb921ca875acf8
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089976"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="61853-103">Habilitar la generación de código QR para aplicaciones de autenticador TOTP en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61853-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="61853-104">ASP.NET Core se suministra con compatibilidad para las aplicaciones de autenticador para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="61853-104">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="61853-105">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración única contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="61853-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="61853-106">2FA uso TOTP es preferible a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="61853-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="61853-107">Una aplicación de autenticador proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña.</span><span class="sxs-lookup"><span data-stu-id="61853-107">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="61853-108">Normalmente, una aplicación autenticadora está instalada en un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="61853-108">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="61853-109">Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan una compatibilidad para la generación de CódigoQR.</span><span class="sxs-lookup"><span data-stu-id="61853-109">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="61853-110">Generadores de CódigoQR facilitan la configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="61853-110">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="61853-111">Este documento le ayudará a agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="61853-111">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="61853-112">Agregar códigos QR a la página de configuración de 2FA</span><span class="sxs-lookup"><span data-stu-id="61853-112">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="61853-113">Estas instrucciones usan *qrcode.js* desde el https://davidshimjs.github.io/qrcodejs/ repositorio.</span><span class="sxs-lookup"><span data-stu-id="61853-113">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="61853-114">Descargue el [qrcode.js javascript biblioteca](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="61853-114">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="61853-115">En *Pages\Account\Manage\EnableAuthenticator.cshtml* (las páginas de Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), busque la `Scripts` sección al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="61853-115">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="61853-116">Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR.</span><span class="sxs-lookup"><span data-stu-id="61853-116">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="61853-117">Debería ser como sigue:</span><span class="sxs-lookup"><span data-stu-id="61853-117">It should look as follows:</span></span>

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

* <span data-ttu-id="61853-118">Eliminar el párrafo que proporciona vínculos a estas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="61853-118">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="61853-119">Ejecutar la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.</span><span class="sxs-lookup"><span data-stu-id="61853-119">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="61853-120">Cambiar el nombre del sitio en el código QR</span><span class="sxs-lookup"><span data-stu-id="61853-120">Change the site name in the QR Code</span></span>

<span data-ttu-id="61853-121">El nombre del sitio en el código QR se toma del nombre del proyecto que elegir al crear inicialmente el proyecto.</span><span class="sxs-lookup"><span data-stu-id="61853-121">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="61853-122">Puede cambiarla si se busca la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* archivo (las páginas de Razor) o la *Controllers\ManageController.cs* archivo (MVC).</span><span class="sxs-lookup"><span data-stu-id="61853-122">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span>

<span data-ttu-id="61853-123">El código predeterminado de la plantilla tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="61853-123">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="61853-124">El segundo parámetro en la llamada a `string.Format` es el nombre de sitio, tomado de su nombre de la solución.</span><span class="sxs-lookup"><span data-stu-id="61853-124">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="61853-125">Se puede cambiar a cualquier valor, pero debe ser siempre dirección URL codificada.</span><span class="sxs-lookup"><span data-stu-id="61853-125">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="61853-126">Utilizar una biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="61853-126">Using a different QR Code library</span></span>

<span data-ttu-id="61853-127">Puede reemplazar la biblioteca de código QR por la biblioteca preferida.</span><span class="sxs-lookup"><span data-stu-id="61853-127">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="61853-128">El código HTML contiene un `qrCode` proporciona la biblioteca de elemento que se puede colocar un código QR mediante cualquier mecanismo.</span><span class="sxs-lookup"><span data-stu-id="61853-128">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="61853-129">La dirección URL con el formato correcto para el código QR está disponible en el:</span><span class="sxs-lookup"><span data-stu-id="61853-129">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="61853-130">`AuthenticatorUri` propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="61853-130">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="61853-131">`data-url` propiedad en el `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="61853-131">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="61853-132">TOTP cliente y servidor sesgo horario</span><span class="sxs-lookup"><span data-stu-id="61853-132">TOTP client and server time skew</span></span>

<span data-ttu-id="61853-133">Autenticación de TOTP (basado en tiempo la contraseña de un solo uso) depende de dispositivo con el servidor y el autenticador tiene una hora precisa.</span><span class="sxs-lookup"><span data-stu-id="61853-133">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="61853-134">Símbolos (tokens) solo duran durante 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="61853-134">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="61853-135">Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="61853-135">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
