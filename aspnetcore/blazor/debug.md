---
title: Depuración de Blazor en ASP.NET Core
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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78648317"
---
# <a name="debug-aspnet-core-blazor"></a>Depuración de Blazor en ASP.NET Core

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Existe *compatibilidad temprana* con la depuración de WebAssembly de Blazor mediante las herramientas de desarrollo del explorador en exploradores basados en Chromium (Chrome o Edge). Hay trabajos en curso para:

* Habilitar la depuración de forma completa en Visual Studio.
* Habilitar la depuración en Visual Studio Code.

Las funcionalidades del depurador son limitadas. Entre los escenarios disponibles se incluyen los siguientes:

* Establecimiento y eliminación de puntos de interrupción.
* Examen (`F10`) del código una vez o reanudación (`F8`) de la ejecución del código.
* En la pantalla *Variables locales*, observe los valores de las variables locales de tipo `int`, `string` y `bool`.
* Vea la pila de llamadas, incluidas las cadenas de llamadas que van desde JavaScript hasta .NET y desde .NET a JavaScript.

*No* puede:

* Observar los valores de las variables locales que no sean de tipo `int`, `string` o `bool`.
* Observar los valores de ninguna propiedad de clase o campos.
* Mantener el puntero sobre las variables para ver sus valores.
* Evaluar expresiones en la consola.
* Ejecutar llamadas asincrónicas paso a paso.
* Realizar la mayoría del resto de escenarios de depuración convencionales.

El desarrollo de otros escenarios de depuración es un enfoque en curso del equipo de ingeniería.

## <a name="prerequisites"></a>Requisitos previos

La depuración requiere cualquiera de los exploradores siguientes:

* Google Chrome (versión 70 o posterior)
* Versión preliminar de Microsoft Edge ([canal de desarrollo de Edge](https://www.microsoftedgeinsider.com))

## <a name="procedure"></a>Procedimiento

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

> [!WARNING]
> La compatibilidad con la depuración en Visual Studio se encuentra en una fase temprana de desarrollo. Actualmente no se admite la depuración con **F5**.

1. Ejecute una aplicación WebAssembly de Blazor en la configuración `Debug` sin depurar (**Ctrl**+**F5** en lugar de **F5**).
1. Abra las propiedades de depuración de la aplicación (la última entrada del menú **Depurar**) y copie la **dirección URL de la aplicación** HTTP. Vaya a la dirección HTTP (no a la dirección HTTPS) de la aplicación mediante un explorador basado en Chromium (Edge Beta o Chrome).
1. Coloque el foco del teclado en la aplicación en la ventana del explorador, no en el panel de herramientas para desarrolladores. Para este procedimiento es recomendable mantener cerrado el panel de herramientas para desarrolladores. Una vez iniciada la depuración, puede volver a abrirlo.
1. Seleccione el siguiente método abreviado de teclado específico de Blazor:

   * `Shift+Alt+D` en Windows
   * `Shift+Cmd+D` en macOS

   Si recibe un mensaje en el que se indica que **no se puede encontrar la pestaña del explorador depurable**, vea [Habilitación de la depuración remota](#enable-remote-debugging).
   
   Después de habilitar la depuración remota:
   
   1\. Se abre una nueva ventana del explorador. Cierre la ventana anterior.

   2\. Coloque el foco del teclado en la aplicación en la ventana del explorador.

   3\. Seleccione el método abreviado de teclado específico de Blazor en la nueva ventana del explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.

   4\. Se abre la pestaña **DevTools** en el explorador. **Vuelva a seleccionar la pestaña de la aplicación en la ventana del explorador.**

   Para adjuntar la aplicación a Visual Studio, vea la sección [Asociación al proceso en Visual Studio](#attach-to-process-in-visual-studio).

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

1. Ejecute una aplicación WebAssembly de Blazor en la configuración `Debug` y pase la opción `--configuration Debug` al comando [dotnet run](/dotnet/core/tools/dotnet-run) `dotnet run --configuration Debug`.
1. Vaya a la aplicación en la dirección URL HTTP que se muestra en la ventana del shell.
1. Coloque el foco del teclado en la aplicación, no en el panel de herramientas para desarrolladores. Para este procedimiento es recomendable mantener cerrado el panel de herramientas para desarrolladores. Una vez iniciada la depuración, puede volver a abrirlo.
1. Seleccione el siguiente método abreviado de teclado específico de Blazor:

   * `Shift+Alt+D` en Windows
   * `Shift+Cmd+D` en macOS

   Si recibe un mensaje en el que se indica que **no se puede encontrar la pestaña del explorador depurable**, vea [Habilitación de la depuración remota](#enable-remote-debugging).
   
   Después de habilitar la depuración remota:
   
   1\. Se abre una nueva ventana del explorador. Cierre la ventana anterior.

   2\. Coloque el foco del teclado en la aplicación en la ventana del explorador, no en el panel de herramientas para desarrolladores.

   3\. Seleccione el método abreviado de teclado específico de Blazor en la nueva ventana del explorador: `Shift+Alt+D` en Windows o `Shift+Cmd+D` en macOS.

---

## <a name="enable-remote-debugging"></a>Habilitar la depuración remota

Si la depuración remota está deshabilitada, Chrome genera una página de error en la que se indica que **no se puede encontrar la pestaña del explorador depurable**. La página de error contiene instrucciones para ejecutar Chrome con el puerto de depuración abierto para que el proxy de depuración de Blazor se pueda conectar a la aplicación. *Cierre todas las instancias de Chrome* y reinicie dicho explorador como se ha indicado.

## <a name="debug-the-app"></a>Depurar la aplicación

Una vez que Chrome se ejecute con la depuración remota habilitada, el método abreviado de teclado de depuración abre una nueva pestaña del depurador. Tras unos instantes, en la pestaña **Orígenes** se mostrará una lista de los ensamblados .NET en la aplicación. Expanda cada ensamblado y busque los archivos de origen *.cs*/ *.razor* disponibles para la depuración. Establezca puntos de interrupción, vuelva a la pestaña de la aplicación y los puntos de interrupción se alcanzarán cuando se ejecute el código. Una vez que se haya alcanzado un punto de interrupción, examine el código una vez (`F10`) o reanude (`F8`) la ejecución del código con normalidad.

Blazor proporciona un proxy de depuración que implementa el [protocolo Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) y aumenta el protocolo con información específica de .NET. Cuando se presiona el método abreviado de teclado de depuración, Blazor apunta a Chrome DevTools en el proxy. El proxy se conecta a la ventana del explorador que se quiere depurar (de ahí la necesidad de habilitar la depuración remota).

## <a name="attach-to-process-in-visual-studio"></a>Asociación al proceso en Visual Studio

La asociación al proceso de la aplicación en Visual Studio es un escenario de depuración *temporal* para WebAssembly de Blazor mientras la depuración **F5** se encuentre en fase de desarrollo.

Para asociar el proceso de la aplicación en ejecución a Visual Studio:

1. En Visual Studio, seleccione **Depurar** > **Asociar al proceso**.
1. En **Tipo de conexión**, seleccione **WebSocket con protocolo Chrome DevTools (sin autenticación)** .
1. En **Destino de la conexión**, pegue la dirección HTTP (no la dirección HTTPS) de la aplicación.
1. Seleccione **Actualizar** para actualizar las entradas en **Procesos disponibles**.
1. Seleccione el proceso del explorador que se va a depurar y, después, **Adjuntar**.
1. En el cuadro de diálogo **Seleccionar tipo de código**, seleccione el tipo de código del explorador específico al que se va a asociar (Edge o Chrome) y, después, seleccione **Aceptar**.

## <a name="browser-source-maps"></a>Mapas de origen del explorador

Los mapas de origen del explorador permiten al explorador volver a asignar los archivos compilados a sus archivos de código fuente originales y se suelen usar para la depuración del lado cliente. Sin embargo, en la actualidad Blazor no asigna C# directamente a JavaScript o WASM. En su lugar, Blazor realiza la interpretación de IL en el explorador, por lo que los mapas de origen no son pertinentes.

## <a name="troubleshoot"></a>Solucionar problemas

Si se encuentra con errores, la sugerencia siguiente puede ser útil:

En la pestaña **Depurador**, abra las herramientas para desarrolladores en el explorador. En la consola, ejecute `localStorage.clear()` para quitar los puntos de interrupción.
