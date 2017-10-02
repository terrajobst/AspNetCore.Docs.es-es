---
title: "Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core"
author: rick-anderson
description: "Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core"
keywords: "Núcleo de ASP.NET, MVC, la generación de código QR, autenticador, 2FA"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 36a3dc542f3321c5e6ebaa078efd8bde3f50948f
ms.sourcegitcommit: e4a1df2a5a85f299322548809e547a79b380bb92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="64e27-104">Habilitar la generación de código QR para las aplicaciones de autenticador de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64e27-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="64e27-105">Nota: En este tema se aplica a ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="64e27-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="64e27-106">ASP.NET Core se suministra con compatibilidad para las aplicaciones de autenticador para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="64e27-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="64e27-107">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración única contraseña algoritmo (TOTP), son el sector recomendado approch para 2FA.</span><span class="sxs-lookup"><span data-stu-id="64e27-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approch for 2FA.</span></span> <span data-ttu-id="64e27-108">2FA uso TOTP es preferible a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="64e27-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="64e27-109">Una aplicación de autenticador proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña.</span><span class="sxs-lookup"><span data-stu-id="64e27-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="64e27-110">Normalmente, una aplicación autenticadora está instalada en un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="64e27-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="64e27-111">Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan compatibilidad para la generación de CódigoQR.</span><span class="sxs-lookup"><span data-stu-id="64e27-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="64e27-112">Generadores de CódigoQR facilitan la configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="64e27-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="64e27-113">Este documento le ayudará a agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración de 2FA.</span><span class="sxs-lookup"><span data-stu-id="64e27-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="64e27-114">Agregar códigos QR a la página de configuración de 2FA</span><span class="sxs-lookup"><span data-stu-id="64e27-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="64e27-115">Estas instrucciones usan *qrcode.js* desde el repositorio de https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="64e27-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="64e27-116">Descargue el [qrcode.js javascript biblioteca](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="64e27-116">Download the  [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="64e27-117">En *Pages\Account\Manage\EnableAuthenticator.cshtml* (las páginas de Razor) o *Views\Account\Manage\EnableAuthenticator.cshtml* (MVC), busque la `Scripts` sección al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="64e27-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Account\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="64e27-118">Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR.</span><span class="sxs-lookup"><span data-stu-id="64e27-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="64e27-119">Debería ser como sigue:</span><span class="sxs-lookup"><span data-stu-id="64e27-119">It should look as follows:</span></span>

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

* <span data-ttu-id="64e27-120">Eliminar el párrafo que proporciona vínculos a estas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="64e27-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="64e27-121">Ejecutar la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.</span><span class="sxs-lookup"><span data-stu-id="64e27-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="64e27-122">Cambiar el nombre del sitio en el código QR</span><span class="sxs-lookup"><span data-stu-id="64e27-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="64e27-123">El nombre del sitio en el código QR se toma del nombre del proyecto que elegir al crear inicialmente el proyecto.</span><span class="sxs-lookup"><span data-stu-id="64e27-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="64e27-124">Puede cambiarla si se busca la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *EnableAuthenticator.cshtml.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="64e27-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in  the *EnableAuthenticator.cshtml.cs* file.</span></span> 

<span data-ttu-id="64e27-125">El código predeterminado de la plantilla tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="64e27-125">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="64e27-126">El segundo parámetro en la llamada a `string.Format` es el nombre de sitio, tomado de su nombre de la solución.</span><span class="sxs-lookup"><span data-stu-id="64e27-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="64e27-127">Se puede cambiar a cualquier valor, pero debe ser siempre dirección URL codificada.</span><span class="sxs-lookup"><span data-stu-id="64e27-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="64e27-128">Utilizar una biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="64e27-128">Using a different QR Code library</span></span>

<span data-ttu-id="64e27-129">Puede reemplazar la biblioteca de código QR por la biblioteca preferida.</span><span class="sxs-lookup"><span data-stu-id="64e27-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="64e27-130">El código HTML contiene un `qrCode` proporciona la biblioteca de elemento que se puede colocar un código QR mediante cualquier mecanismo.</span><span class="sxs-lookup"><span data-stu-id="64e27-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="64e27-131">La dirección URL con el formato correcto para el código QR está disponible en el:</span><span class="sxs-lookup"><span data-stu-id="64e27-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="64e27-132">`AuthenticatorUri`propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="64e27-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="64e27-133">`data-url`propiedad en el `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="64e27-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="64e27-134">Use `@Html.Raw` para tener acceso a la propiedad de modelo en una vista (en caso contrario, los signos de y comercial en la dirección url se va a codificar double y se pasará por alto el parámetro de la etiqueta del código QR).</span><span class="sxs-lookup"><span data-stu-id="64e27-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="64e27-135">TOTP cliente y servidor sesgo horario</span><span class="sxs-lookup"><span data-stu-id="64e27-135">TOTP client and server time skew</span></span>

<span data-ttu-id="64e27-136">Autenticación de TOTP depende de dispositivo con el servidor y el autenticador tiene una hora precisa.</span><span class="sxs-lookup"><span data-stu-id="64e27-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="64e27-137">Símbolos (tokens) solo duran durante 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="64e27-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="64e27-138">Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="64e27-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
