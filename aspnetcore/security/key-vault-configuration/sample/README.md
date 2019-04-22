---
ms.openlocfilehash: 209b5c41e17897693962954b1e795bdbb41f9384
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012739"
---
# <a name="key-vault-configuration-provider-sample-app"></a>Aplicación de ejemplo de proveedor de configuración de almacén de claves

En este ejemplo se muestra el uso del proveedor de configuración de Azure Key Vault.

El ejemplo se ejecuta en uno de los dos modos, determinados por la `#define` instrucción en la parte superior de la *Program.cs* archivo. Para obtener instrucciones, consulte [las directivas de preprocesador en el código de ejemplo](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Certificate` &ndash; Muestra el uso de un certificado X.509 y de identificador de cliente de Azure Key Vault a secretos de acceso almacenada en Azure Key Vault. Esta versión de la muestra se puede ejecutar desde cualquier ubicación, implementada en Azure App Service o en cualquier host capaz de ofrecer servicio a una aplicación ASP.NET Core.
* `Managed` &ndash; Muestra cómo usar Azure [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) para autenticar la aplicación a Azure Key Vault con la autenticación de Azure AD sin credenciales en código o configuración de la aplicación. Un identificador de cliente de Azure AD y el secreto no son necesarios para la aplicación para autenticarse con Azure Key Vault. Este ejemplo debe implementarse en Azure App Service para explorar el scearnio identidad administrada.

Para obtener más información, consulte [proveedor de configuración de Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
