---
title: Habilitar la generación de código QR para aplicaciones de authenticator TOTP en ASP.NET Core
author: rick-anderson
description: Descubra cómo habilitar la generación de código QR para las aplicaciones de autenticador TOTP que funcionan con la autenticación en dos fases principales de ASP.NET.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 437f354f71128a98bae9abdced291e04efc9f48e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225387"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="e0c7d-103">Habilitar la generación de código QR para aplicaciones de authenticator TOTP en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e0c7d-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e0c7d-104">Códigos QR requiere ASP.NET Core 2.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e0c7d-105">ASP.NET Core se distribuye con el soporte técnico para las aplicaciones de authenticator para la autenticación individual.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="e0c7d-106">Dos aplicaciones de autenticador de autenticación (2FA) de factor, con una duración definida por única vez contraseña algoritmo (TOTP), son el enfoque para 2FA recomendado en el sector.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="e0c7d-107">2FA uso TOTP es preferible a SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="e0c7d-108">Una aplicación authenticator proporciona un código de 6 a 8 dígitos que los usuarios deben escribir después de confirmar su nombre de usuario y contraseña.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="e0c7d-109">Normalmente, una aplicación de autenticador está instalada en un Smartphone.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="e0c7d-110">Las plantillas de aplicación web de ASP.NET Core admiten autenticadores, pero no proporcionan compatibilidad para la generación de CódigoQR.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="e0c7d-111">Los generadores de CódigoQR facilitan la instalación de 2FA.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="e0c7d-112">Este documento le guiará a través de agregar [código QR](https://wikipedia.org/wiki/QR_code) generación a la página de configuración 2FA.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="e0c7d-113">Agregar códigos QR en la página de configuración de 2FA</span><span class="sxs-lookup"><span data-stu-id="e0c7d-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="e0c7d-114">Estas instrucciones usan *qrcode.js* desde el https://davidshimjs.github.io/qrcodejs/ repositorio.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="e0c7d-115">Descargue el [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) a la `wwwroot\lib` carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="e0c7d-116">Siga las instrucciones de [Scaffold identidad](xref:security/authentication/scaffold-identity) para generar */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-116">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="e0c7d-117">En */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, busque el `Scripts` sección al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="e0c7d-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="e0c7d-118">En *Pages/Account/Manage/EnableAuthenticator.cshtml* (las páginas de Razor) o *Views/Manage/EnableAuthenticator.cshtml* (MVC), busque el `Scripts` sección al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="e0c7d-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="e0c7d-119">Actualización de la `Scripts` sección para agregar una referencia a la `qrcodejs` biblioteca que agregó y una llamada a generar el código QR.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-119">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="e0c7d-120">Debe tener el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="e0c7d-120">It should look as follows:</span></span>

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

* <span data-ttu-id="e0c7d-121">Eliminar el párrafo que proporciona vínculos a estas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-121">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="e0c7d-122">Ejecute la aplicación y asegúrese de que puede examinar el código QR y validar el código que demuestra el autenticador.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-122">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="e0c7d-123">Cambiar el nombre del sitio en el código QR</span><span class="sxs-lookup"><span data-stu-id="e0c7d-123">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e0c7d-124">El nombre del sitio en el código QR se toma del nombre del proyecto que elige al crear inicialmente el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-124">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="e0c7d-125">Puede cambiar mediante la búsqueda de la `GenerateQrCodeUri(string email, string unformattedKey)` método en el */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-125">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e0c7d-126">El nombre del sitio en el código QR se toma del nombre del proyecto que elige al crear inicialmente el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-126">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="e0c7d-127">Puede cambiar mediante la búsqueda de la `GenerateQrCodeUri(string email, string unformattedKey)` método en el *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* archivo (las páginas de Razor) o el *Controllers/ManageController.cs* archivo (MVC).</span><span class="sxs-lookup"><span data-stu-id="e0c7d-127">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="e0c7d-128">El código predeterminado de la plantilla tiene el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="e0c7d-128">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="e0c7d-129">El segundo parámetro en la llamada a `string.Format` es el nombre del sitio procedente de su nombre de la solución.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-129">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="e0c7d-130">Se puede cambiar a cualquier valor, pero siempre debe estar codificado como URL.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-130">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="e0c7d-131">Uso de una biblioteca de código QR diferente</span><span class="sxs-lookup"><span data-stu-id="e0c7d-131">Using a different QR Code library</span></span>

<span data-ttu-id="e0c7d-132">Puede reemplazar la biblioteca de código QR con la biblioteca preferida.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-132">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="e0c7d-133">El código HTML contiene un `qrCode` elemento donde se puede colocar un código QR mediante cualquier mecanismo de la biblioteca proporciona.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-133">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="e0c7d-134">La dirección URL con el formato correcto para el código QR está disponible en el:</span><span class="sxs-lookup"><span data-stu-id="e0c7d-134">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="e0c7d-135">`AuthenticatorUri` propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-135">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="e0c7d-136">`data-url` propiedad en el `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-136">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="e0c7d-137">TOTP cliente y servidor desfase horario</span><span class="sxs-lookup"><span data-stu-id="e0c7d-137">TOTP client and server time skew</span></span>

<span data-ttu-id="e0c7d-138">Autenticación TOTP (basado en tiempo de contraseña de un solo uso) depende del dispositivo con el servidor y el autenticador tiene una hora precisa.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-138">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="e0c7d-139">Los tokens solo duran 30 segundos.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-139">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="e0c7d-140">Si se producen errores en los inicios de sesión TOTP 2FA, compruebe que la hora del servidor es precisa y preferiblemente sincronizada para un servicio NTP preciso.</span><span class="sxs-lookup"><span data-stu-id="e0c7d-140">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
