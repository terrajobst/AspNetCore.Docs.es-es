---
title: Depuración ASP.NET Core extraordinaria
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones increíbles.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/22/2019
uid: blazor/debug
ms.openlocfilehash: e9477e504d32fd1dd5d6c87392386d1131f46e9f
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963998"
---
# <a name="debug-aspnet-core-blazor"></a>Depuración ASP.NET Core extraordinaria

[Daniel Roth](https://github.com/danroth27)

Existe compatibilidad *temprana* para depurar aplicaciones webassembly increíblemente que se ejecutan en webassembly en Chrome.

Las funcionalidades del depurador están limitadas. Entre los escenarios disponibles se incluyen:

* Establecer y quitar puntos de interrupción.
* Un solo paso (`F10`) a través de la ejecución de`F8`código de código o de reanudación ().
* En la pantalla *variables locales* , observe los valores de las variables locales de `int`tipo `string`, y `bool`.
* Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.

*No*se puede:

* Observe los valores de las variables locales que no `int`sean `string`, o `bool`.
* Observe los valores de cualquier propiedad o campo de clase.
* Mantenga el mouse sobre las variables para ver sus valores.
* Evaluar expresiones en la consola.
* Paso a través de las llamadas asincrónicas.
* Realice la mayoría de los demás escenarios de depuración normales.

El desarrollo de otros escenarios de depuración es un enfoque continuo del equipo de ingeniería.

## <a name="prerequisites"></a>Requisitos previos

La depuración requiere cualquiera de los siguientes exploradores:

* Google Chrome (versión 70 o posterior)
* Microsoft Edge Preview ([canal de desarrollo perimetral](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Procedimiento

1. Ejecutar una aplicación de `Debug` webassembly increíblemente bajo configuración. Pase la `--configuration Debug` opción al comando [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Acceda a la aplicación en el explorador.
1. Coloque el foco de teclado en la aplicación, no en el panel herramientas de desarrollo. Se puede cerrar el panel herramientas de desarrollo cuando se inicia la depuración.
1. Seleccione el siguiente método abreviado de teclado:
   * `Shift+Alt+D`en Windows/Linux
   * `Shift+Cmd+D`en macOS
1. Siga los pasos indicados en la pantalla para reiniciar el explorador con la depuración remota habilitada.
1. Para iniciar la sesión de depuración, seleccione de nuevo el siguiente método abreviado de teclado específico de increíbles:
   * `Shift+Alt+D`en Windows/Linux
   * `Shift+Cmd+D`en macOS

## <a name="enable-remote-debugging"></a>Habilitar depuración remota

Si la depuración remota está deshabilitada, Chrome genera una página de error **no se puede encontrar la pestaña del explorador depurable** . La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el proxy de depuración increíblemente rápido pueda conectarse a la aplicación. *Cierre todas las instancias de Chrome* y reinicie Chrome tal y como se indica.

## <a name="debug-the-app"></a>Depurar la aplicación

Una vez que Chrome se ejecuta con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Después de un momento, la pestaña **orígenes** muestra una lista de los ensamblados .net en la aplicación. Expanda cada ensamblado y busque los archivos de origen *. CS*/ *. Razor* disponibles para la depuración. Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código. Una vez que se visita un punto de interrupción,`F10`se ejecuta el código de un solo`F8`paso () a través del código o se reanuda () la ejecución del código con normalidad.

Increíbles proporciona un proxy de depuración que implementa el [Protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red. Cuando se presiona el método abreviado de teclado de depuración, el DevTools de cromo apunta al proxy. El proxy se conecta a la ventana del explorador que busca depurar (por lo tanto, la necesidad de habilitar la depuración remota).

## <a name="browser-source-maps"></a>Mapas de origen del explorador

Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente. Sin embargo, increíble no se asigna C# directamente a JavaScript/WASM. En su lugar, increíble es una interpretación de IL en el explorador, por lo que los mapas de origen no son relevantes.

## <a name="troubleshooting-tip"></a>Sugerencia para la solución de problemas

Si se encuentra con errores, la sugerencia siguiente puede ser útil:

En la pestaña **depurador** , abra las herramientas de desarrollo en el explorador. En la consola de, `localStorage.clear()` ejecute para quitar los puntos de interrupción.
