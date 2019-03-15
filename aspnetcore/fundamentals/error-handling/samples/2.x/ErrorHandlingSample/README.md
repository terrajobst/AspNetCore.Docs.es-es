---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665759"
---
# <a name="error-handling-sample-application"></a>Aplicación de ejemplo de control de errores

En esta aplicación de ejemplo se muestran los escenarios descritos en el tema [Control de errores en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).

## <a name="configure-a-custom-exception-handling-page"></a>Configurar una página personalizada de control de excepciones

Cuando la aplicación no se ejecuta en el entorno de desarrollo, el middleware de control de excepciones:

* Captura las excepciones.
* Registra las excepciones.
* Vuelve a ejecutar la solicitud en una canalización alternativa de la ruta de acceso proporcionada.

## <a name="configure-custom-exception-handling-code"></a>Configuración del código personalizado de control de excepciones

Una alternativa a proporcionar un punto de conexión para los errores con una página personalizada de control de excepciones es proporcionar una función lambda a `UseExceptionHandler`. Usar una función lambda con `UseExceptionHandler` permite acceder al error antes de devolver la respuesta.

La aplicación de ejemplo muestra el código personalizado de control de excepciones en `Startup.Configure`. Siga las instrucciones que aparecen en la parte superior del archivo *Startup.cs* (`LambdaErrorHandler`). Una vez que se inicie la aplicación, desencadene una excepción con el vínculo **Iniciar excepción** en la página de índice.
