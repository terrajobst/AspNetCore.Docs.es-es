---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472341"
---
*  Para confiar en el certificado de desarrollo de HTTPS, ejecute el comando siguiente:

    ```console
    dotnet dev-certs https --trust
    ```

    El comando anterior muestra el siguiente cuadro de diálogo:

    ![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

*    Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.

     Para obtener más información, vea [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).