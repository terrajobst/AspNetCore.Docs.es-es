---
title: "Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core"
author: rick-anderson
description: "Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 87a6d3f17216625e0f7ce206dddd72cb2f371e9a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="20695-103">Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20695-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="20695-104">Nota: En este tema se aplica a ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20695-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="20695-105">ASP.NET Core se suministra con compatibilidad para las aplicaciones de autenticador para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="20695-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="20695-106">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración única contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="20695-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="20695-107">2FA uso TOTP es preferible a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="20695-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="20695-108">Una aplicación de autenticador proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña.</span><span class="sxs-lookup"><span data-stu-id="20695-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="20695-109">Normalmente, una aplicación autenticadora está instalada en un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="20695-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="20695-110">Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan compatibilidad para la generación de CódigoQR.</span><span class="sxs-lookup"><span data-stu-id="20695-110">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="20695-111">Generadores de CódigoQR facilitan la configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="20695-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="20695-112">Este documento le ayudará a agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="20695-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="20695-113">Agregar códigos QR a la página de configuración de 2FA</span><span class="sxs-lookup"><span data-stu-id="20695-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="20695-114">Estas instrucciones usan *qrcode.js* desde el repositorio de https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="20695-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="20695-115">Descargue el [qrcode.js javascript biblioteca](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="20695-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="20695-116">En *Pages\Account\Manage\EnableAuthenticator.cshtml* (las páginas de Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), busque la `Scripts` sección al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="20695-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="20695-117">Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR.</span><span class="sxs-lookup"><span data-stu-id="20695-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="20695-118">Debería ser como sigue:</span><span class="sxs-lookup"><span data-stu-id="20695-118">It should look as follows:</span></span>

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

* <span data-ttu-id="20695-119">Eliminar el párrafo que proporciona vínculos a estas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="20695-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="20695-120">Ejecutar la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.</span><span class="sxs-lookup"><span data-stu-id="20695-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="20695-121">Cambiar el nombre del sitio en el código QR</span><span class="sxs-lookup"><span data-stu-id="20695-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="20695-122">El nombre del sitio en el código QR se toma del nombre del proyecto que elegir al crear inicialmente el proyecto.</span><span class="sxs-lookup"><span data-stu-id="20695-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="20695-123">Puede cambiarla si se busca la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* archivo (las páginas de Razor) o la *Controllers\ManageController.cs* archivo (MVC).</span><span class="sxs-lookup"><span data-stu-id="20695-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="20695-124">El código predeterminado de la plantilla tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="20695-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="20695-125">El segundo parámetro en la llamada a `string.Format` es el nombre de sitio, tomado de su nombre de la solución.</span><span class="sxs-lookup"><span data-stu-id="20695-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="20695-126">Se puede cambiar a cualquier valor, pero debe ser siempre dirección URL codificada.</span><span class="sxs-lookup"><span data-stu-id="20695-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="20695-127">Utilizar una biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="20695-127">Using a different QR Code library</span></span>

<span data-ttu-id="20695-128">Puede reemplazar la biblioteca de código QR por la biblioteca preferida.</span><span class="sxs-lookup"><span data-stu-id="20695-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="20695-129">El código HTML contiene un `qrCode` proporciona la biblioteca de elemento que se puede colocar un código QR mediante cualquier mecanismo.</span><span class="sxs-lookup"><span data-stu-id="20695-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="20695-130">La dirección URL con el formato correcto para el código QR está disponible en el:</span><span class="sxs-lookup"><span data-stu-id="20695-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="20695-131">`AuthenticatorUri`propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="20695-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="20695-132">`data-url`propiedad en el `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="20695-132">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="20695-133">Use `@Html.Raw` para tener acceso a la propiedad de modelo en una vista (en caso contrario, los signos de y comercial en la dirección url se va a codificar double y se pasará por alto el parámetro de la etiqueta del código QR).</span><span class="sxs-lookup"><span data-stu-id="20695-133">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="20695-134">TOTP cliente y servidor sesgo horario</span><span class="sxs-lookup"><span data-stu-id="20695-134">TOTP client and server time skew</span></span>

<span data-ttu-id="20695-135">Autenticación de TOTP depende de dispositivo con el servidor y el autenticador tiene una hora precisa.</span><span class="sxs-lookup"><span data-stu-id="20695-135">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="20695-136">Símbolos (tokens) solo duran durante 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="20695-136">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="20695-137">Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="20695-137">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
