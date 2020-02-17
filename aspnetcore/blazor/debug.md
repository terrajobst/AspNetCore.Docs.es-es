---
title: Depurar ASP.NET Core Blazor
author: guardrex
description: Obtenga información sobre cómo depurar aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/debug
ms.openlocfilehash: 1b0035af48b82807a6ae14835a41a1ecbef06bb6
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76159994"
---
# <a name="debug-aspnet-core-opno-locblazor"></a>Depurar ASP.NET Core [!OP.NO-LOC(Blazor)]

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Existe compatibilidad *temprana* para depurar [!OP.NO-LOC(Blazor)] webassembly con las herramientas de desarrollo del explorador en exploradores basados en cromo (Chrome/ Microsoft Edge). El trabajo está en curso para:

* Habilite totalmente la depuración en Visual Studio.
* Habilite la depuración en Visual Studio Code.

Las funcionalidades del depurador están limitadas. Entre los escenarios disponibles se incluyen:

* Establecer y quitar puntos de interrupción.
* Un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`).
* En la pantalla *variables locales* , observe los valores de las variables locales de tipo `int`, `string`y `bool`.
* Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.

*No*se puede:

* Observe los valores de las variables locales que no son `int`, `string`o `bool`.
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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> La compatibilidad con la depuración en Visual Studio se encuentra en una fase temprana del desarrollo. La depuración **F5** no se admite actualmente.

1. Ejecute una aplicación [!OP.NO-LOC(Blazor)] webassembly en `Debug` configuración sin depurar (**Ctrl**+**F5** en lugar de **F5**).
1. Abra las propiedades de depuración de la aplicación (última entrada en el menú **depurar** ) y copie la **dirección URL**de la aplicación http. Vaya a la dirección HTTP (no a la dirección HTTPS) de la aplicación mediante un explorador basado en cromo (versión beta o Chrome de Microsoft Edge).
1. Coloque el foco de teclado en la aplicación en la ventana del explorador, no en el panel herramientas de desarrollo. Es mejor mantener el panel herramientas de desarrollo cerrado para este procedimiento. Una vez iniciada la depuración, puede volver a abrir el panel herramientas de desarrollo.
1. Seleccione el siguiente método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]:

   * `Shift+Alt+D` en Windows
   * `Shift+Cmd+D` en macOS

   Si recibe la **pestaña no se puede encontrar el explorador depurable**, vea [Habilitar depuración remota](#enable-remote-debugging).
   
   Después de habilitar la depuración remota:
   
   1 \. Se abre una nueva ventana del explorador. Cierre la ventana anterior.

   2 \. Coloque el foco de teclado en la aplicación en la ventana del explorador.

   3 \. Seleccione el método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]en la ventana nuevo explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.

   4 \. Se abre la pestaña **DevTools** en el explorador. **Reseleccione la pestaña de la aplicación en la ventana del explorador.**

   Para adjuntar la aplicación a Visual Studio, consulte la sección [asociar al proceso en Visual Studio](#attach-to-process-in-visual-studio) .

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

1. Ejecute una aplicación [!OP.NO-LOC(Blazor)] webassembly en `Debug` configuración; para ello, pase la opción `--configuration Debug` al comando [dotnet Run](/dotnet/core/tools/dotnet-run) : `dotnet run --configuration Debug`.
1. Vaya a la aplicación en la dirección URL HTTP que se muestra en la ventana del shell.
1. Coloque el foco de teclado en la aplicación, no en el panel herramientas de desarrollo. Es mejor mantener el panel herramientas de desarrollo cerrado para este procedimiento. Una vez iniciada la depuración, puede volver a abrir el panel herramientas de desarrollo.
1. Seleccione el siguiente método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]:

   * `Shift+Alt+D` en Windows
   * `Shift+Cmd+D` en macOS

   Si recibe la **pestaña no se puede encontrar el explorador depurable**, vea [Habilitar depuración remota](#enable-remote-debugging).
   
   Después de habilitar la depuración remota:
   
   1 \. Se abre una nueva ventana del explorador. Cierre la ventana anterior.

   2 \. Coloque el foco de teclado en la aplicación en la ventana del explorador, no en el panel herramientas de desarrollo.

   3 \. Seleccione el método abreviado de teclado específico de [!OP.NO-LOC(Blazor)]en la ventana nuevo explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.

---

## <a name="enable-remote-debugging"></a>Habilitar la depuración remota

Si la depuración remota está deshabilitada, Chrome genera una página de error **no se puede encontrar la pestaña del explorador depurable** . La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el Blazor proxy de depuración pueda conectarse a la aplicación. *Cierre todas las instancias de Chrome* y reinicie Chrome tal y como se indica.

## <a name="debug-the-app"></a>Depurar la aplicación

Una vez que Chrome se ejecuta con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Después de un momento, la pestaña **orígenes** muestra una lista de los ensamblados .net en la aplicación. Expanda cada ensamblado y busque los archivos de origen *. cs*/ *. Razor* disponibles para la depuración. Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código. Una vez que se visita un punto de interrupción, se realiza un solo paso (`F10`) a través de la ejecución de código de código o de reanudación (`F8`) con normalidad.

Blazor proporciona un proxy de depuración que implementa el [Protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con. Información específica de la red. Cuando se presiona el método abreviado de teclado de depuración, Blazor señala el DevTools de cromo en el proxy. El proxy se conecta a la ventana del explorador que busca depurar (por lo tanto, la necesidad de habilitar la depuración remota).

## <a name="attach-to-process-in-visual-studio"></a>Asociar al proceso en Visual Studio

Adjuntar al proceso de la aplicación en Visual Studio es un escenario de depuración *temporal* para Blazor webassembly mientras la depuración **F5** está en desarrollo.

Para asociar el proceso de la aplicación en ejecución a Visual Studio:

1. En Visual Studio, seleccione **Depurar** > **adjuntar al proceso**.
1. Para el **tipo de conexión**, seleccione **Protocolo de WebSocket DevTools de Chrome (sin autenticación)** .
1. En destino de la **conexión**, pegue la dirección http (no la dirección https) de la aplicación.
1. Seleccione **Actualizar** para actualizar las entradas en **procesos disponibles**.
1. Seleccione el proceso del explorador para depurar y seleccione **adjuntar**.
1. En el cuadro de diálogo **Seleccionar tipo de código** , seleccione el tipo de código para el explorador específico al que se va a asociar (Microsoft Edge o Chrome) y, después, seleccione **Aceptar**.

## <a name="browser-source-maps"></a>Mapas de origen del explorador

Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente. Sin embargo, Blazor actualmente no C# se asigna directamente a JavaScript/WASM. En su lugar, Blazor realiza la interpretación de IL en el explorador, por lo que los mapas de origen no son relevantes.

## <a name="troubleshoot"></a>Solucionar problemas

Si se encuentra con errores, la sugerencia siguiente puede ser útil:

En la pestaña **depurador** , abra las herramientas de desarrollo en el explorador. En la consola de, ejecute `localStorage.clear()` para quitar los puntos de interrupción.
