---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665759"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="e3c2f-101">Aplicación de ejemplo de control de errores</span><span class="sxs-lookup"><span data-stu-id="e3c2f-101">Error Handling Sample Application</span></span>

<span data-ttu-id="e3c2f-102">En esta aplicación de ejemplo se muestran los escenarios descritos en el tema [Control de errores en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="e3c2f-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="e3c2f-103">Configurar una página personalizada de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="e3c2f-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="e3c2f-104">Cuando la aplicación no se ejecuta en el entorno de desarrollo, el middleware de control de excepciones:</span><span class="sxs-lookup"><span data-stu-id="e3c2f-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="e3c2f-105">Captura las excepciones.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-105">Catches exceptions.</span></span>
* <span data-ttu-id="e3c2f-106">Registra las excepciones.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-106">Logs exceptions.</span></span>
* <span data-ttu-id="e3c2f-107">Vuelve a ejecutar la solicitud en una canalización alternativa de la ruta de acceso proporcionada.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="e3c2f-108">Configuración del código personalizado de control de excepciones</span><span class="sxs-lookup"><span data-stu-id="e3c2f-108">Configure custom exception handling code</span></span>

<span data-ttu-id="e3c2f-109">Una alternativa a proporcionar un punto de conexión para los errores con una página personalizada de control de excepciones es proporcionar una función lambda a `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="e3c2f-110">Usar una función lambda con `UseExceptionHandler` permite acceder al error antes de devolver la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="e3c2f-111">La aplicación de ejemplo muestra el código personalizado de control de excepciones en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="e3c2f-112">Siga las instrucciones que aparecen en la parte superior del archivo *Startup.cs* (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="e3c2f-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="e3c2f-113">Una vez que se inicie la aplicación, desencadene una excepción con el vínculo **Iniciar excepción** en la página de índice.</span><span class="sxs-lookup"><span data-stu-id="e3c2f-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
