---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472341"
---
*  <span data-ttu-id="25891-101">Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="25891-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

    <span data-ttu-id="25891-102">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="25891-102">The preceding command displays the following dialog:</span></span>

    ![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

*    <span data-ttu-id="25891-104">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="25891-104">Select **Yes** if you agree to trust the development certificate.</span></span>

     <span data-ttu-id="25891-105">Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="25891-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>