---
ms.openlocfilehash: 2ec079606cb48670dbc3852482fd8d401e7db44b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737294"
---
* <span data-ttu-id="3103f-101">Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="3103f-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="3103f-102">El comando anterior muestra la siguiente salida:</span><span class="sxs-lookup"><span data-stu-id="3103f-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="3103f-103">Si se le solicita, escriba el nombre de usuario administrador y la contraseña.</span><span class="sxs-lookup"><span data-stu-id="3103f-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="3103f-104">El certificado se instalará ahora y será de confianza.</span><span class="sxs-lookup"><span data-stu-id="3103f-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="3103f-105">Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="3103f-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>