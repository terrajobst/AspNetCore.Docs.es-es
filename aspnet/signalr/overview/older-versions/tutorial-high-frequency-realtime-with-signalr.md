---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: Alta frecuencia en tiempo real con SignalR 1.x | Microsoft Docs
author: pfletcher
description: Este tutorial muestra cómo crear una aplicación web que usa SignalR de ASP.NET para proporcionar funcionalidad de mensajería de alta frecuencia. Alta frecuencia de mensajería en...
ms.author: riande
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: c677bcbc78eac6056c035c2b34fe659caac9c6fa
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912285"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Alta frecuencia en tiempo real con SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este tutorial muestra cómo crear una aplicación web que usa SignalR de ASP.NET para proporcionar funcionalidad de mensajería de alta frecuencia. En este caso la mensajería de alta frecuencia significa que las actualizaciones que se envían a una tarifa fija; en el caso de esta aplicación, hasta 10 mensajes por segundo.
> 
> La aplicación que creará en este tutorial muestra una forma que los usuarios pueden arrastrar. La posición de la forma en todos los demás exploradores conectados, a continuación, se actualizará para que coincida con la posición de la forma arrastrada mediante actualizaciones periódicas.
> 
> Los conceptos presentados en este tutorial tienen aplicaciones de juegos en tiempo real y otras aplicaciones de la simulación.
> 
> Comentarios en el tutorial son bienvenidos. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Información general

Este tutorial muestra cómo crear una aplicación que comparte el estado de un objeto con otros exploradores en tiempo real. La aplicación que vamos a crear se denomina MoveShape. Mostrará la página de MoveShape un elemento HTML Div que el usuario puede arrastrar; Cuando el usuario arrastra el elemento Div, la nueva posición se enviarán al servidor, que indicará que todos los clientes conectados para actualizar la posición de la forma para que coincida con.

![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

La aplicación creada en este tutorial se basa en una demostración de Damian Edwards. Se puede ver un vídeo que contiene esta demostración [aquí](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Para empezar el tutorial, que demuestra cómo enviar mensajes de SignalR en cada evento que se desencadena a medida que se arrastra la forma. Cada cliente conectado, a continuación, actualizará la posición de la versión local de la forma cada vez que se recibe un mensaje.

Aunque la aplicación funcionará con este método, no es un modelo de programación recomendado, ya que no habría ningún límite superior para el número de mensajes enviados al obtener, por lo que los clientes y el servidor podrían obtener experimentando con mensajes y podría degradar el rendimiento . La animación mostrada en el cliente también sería separado, como la forma se pasaría al instante por cada método, en lugar de mover sin problemas a cada nueva ubicación. En secciones posteriores de este tutorial se muestran cómo crear una función de temporizador que restringe la velocidad máxima a la que los mensajes se envían por el cliente o el servidor y cómo mover la forma sin problemas entre las ubicaciones. La versión final de la aplicación creada en este tutorial se puede descargar desde [Galería de código](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Crear el proyecto](#createtheproject)
- [Agregue los paquetes de JQuery.UI NuGet y ASP.NET SignalR](#nugetpackages)
- [Crear la aplicación básica](#baseapp)
- [Agregar el bucle de cliente](#clientloop)
- [Agregar bucle de servidor](#serverloop)
- [Agregar una animación fluida en el cliente](#animation)
- [Pasos adicionales](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Este tutorial requiere Visual Studio 2012 o Visual Studio 2010. Si se usa Visual Studio 2010, se usará el proyecto de .NET Framework 4 en lugar de .NET Framework 4.5.

Si utiliza Visual Studio 2012, se recomienda que instale el [actualización ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Esta actualización contiene nuevas características, como mejoras en la publicación, la nueva funcionalidad y las nuevas plantillas.

Si tiene Visual Studio 2010, asegúrese de que [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) está instalado.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Crear el proyecto

En esta sección, vamos a crear el proyecto en Visual Studio.

1. Desde el **archivo** menú haga clic en **nuevo proyecto**.
2. En el **nuevo proyecto** cuadro de diálogo, expanda **C#** en **plantillas** y seleccione **Web**.
3. Seleccione el **aplicación Web ASP.NET vacía** plantilla, el nombre del proyecto *MoveShapeDemo*y haga clic en **Aceptar**.

    ![Creación del nuevo proyecto](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>Agregue los paquetes de JQuery.UI NuGet y SignalR

Puede agregar funcionalidad de SignalR a un proyecto mediante la instalación de un paquete de NuGet. Este tutorial también utilizará el paquete JQuery.UI para permitir la forma poder arrastrar y anima.

1. Haga clic en **herramientas | Administrador de paquetes de NuGet | Consola de administrador de paquetes**.
2. Escriba el siguiente comando en el Administrador de paquetes.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    El paquete de SignalR instala a una serie de otros paquetes de NuGet como dependencias. Cuando finalice la instalación tienen todos los componentes de servidor y cliente necesarios para usar SignalR en una aplicación ASP.NET.
3. Escriba el siguiente comando en la consola del Administrador de paquetes para instalar los paquetes de JQuery y JQuery.UI.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Crear la aplicación básica

En esta sección, vamos a crear una aplicación de explorador que envía la ubicación de la forma en el servidor durante cada evento de mover el mouse. El servidor transmite, a continuación, esta información para todos los demás clientes conectados cuando se reciben. Se expandirán en esta aplicación en secciones posteriores.

1. En **el Explorador de soluciones**, haga doble clic en el proyecto y seleccione **agregar**, **clase...** . Nombre de la clase **MoveShapeHub** y haga clic en **agregar**.
2. Reemplace el código en el nuevo **MoveShapeHub** con el código siguiente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    La `MoveShapeHub` clase anterior es una implementación de un concentrador SignalR. Como en el [Introducción a SignalR](index.md) tutorial, el concentrador tiene un método que los clientes llamen directamente. En este caso, el cliente enviará un objeto que contiene las nuevas coordenadas X e Y de la forma en el servidor, que, a continuación, obtiene difunden a todos los demás clientes conectados. SignalR serializará automáticamente este objeto mediante JSON.

    El objeto que se enviará al cliente (`ShapeModel`) contiene miembros para almacenar la posición de la forma. La versión del objeto en el servidor también contiene un miembro para realizar un seguimiento de los datos del cliente que se va a almacenar, para que un cliente determinado no se envíe a sus propios datos. Este miembro se usa el `JsonIgnore` atributo para impedir que se va a serializar y enviar al cliente.
3. A continuación, configuraremos el concentrador cuando se inicia la aplicación. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Clase de aplicación global**. Acepte el nombre predeterminado de *Global* y haga clic en **Aceptar**.

    ![Agregar clase de aplicación Global](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Agregue el siguiente `using` instrucción después proporcionado **mediante** las instrucciones de la clase de Global.asax.cs.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Agregue la siguiente línea de código en el `Application_Start` método de la clase Global para registrar la ruta predeterminada para SignalR.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    El archivo global.asax debe tener el siguiente aspecto:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. A continuación, vamos a agregar al cliente. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **página Html**. Asigne un nombre adecuado a la página (como **Default.html**) y haga clic en **agregar**.
7. En **el Explorador de soluciones**, haga clic en la página que acaba de crear y haga clic en **establecer como página de inicio**.
8. Reemplace el código predeterminado en la página HTML con el siguiente fragmento de código.

    > [!NOTE]
    > Comprobar que el script hace referencia a continuación de la coincidencia de los paquetes que se agrega al proyecto en la carpeta Scripts. En Visual Studio 2010, la versión de JQuery y SignalR que se agrega al proyecto no puede coincidir con los números de versión siguientes.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    El código HTML y JavaScript anterior crea un elemento Div rojo llamado de forma, habilita el comportamiento de arrastrar la forma mediante la biblioteca de jQuery y utiliza la forma `drag` eventos a la posición de la forma de envío al servidor.
9. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlo en otra ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Agregar el bucle de cliente

Dado que el envío de la ubicación de la forma en cada evento de mover el mouse, se creará una cantidad innecesarios del tráfico de red, los mensajes desde el cliente deben estar limitado. Vamos a usar el código javascript `setInterval` función para establecer un bucle que envía información de posición de nuevo al servidor con una tarifa fija. Este bucle es una representación muy básica de un "bucle de juego", una función varias veces a que controla toda la funcionalidad de un juego o una simulación de otro.

1. Actualice el código de cliente en la página HTML para que coincida con el siguiente fragmento de código.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    La actualización anterior agrega el `updateServerModel` función, que se invoca con una frecuencia fija. Esta función envía los datos de la posición en el servidor cada vez que el `moved` marca indica que hay nuevos datos de posición para enviar.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlo en otra ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. Puesto que se limitará el número de mensajes que se envían al servidor, la animación no aparecerán como smooth, como se muestra en la sección anterior.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Agregar bucle de servidor

En la aplicación actual, los mensajes enviados desde el servidor al cliente salen con tanta frecuencia como se reciben. Esto presenta un problema similar como se vio en el cliente; los mensajes pueden enviarse con más frecuencia que son necesarios y la conexión podría inundarse como resultado. En esta sección se describe cómo actualizar el servidor para implementar un temporizador que limita la velocidad de los mensajes salientes.

1. Reemplace el contenido de `MoveShapeHub.cs` con el siguiente fragmento de código.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    El código anterior expande el cliente para agregar la `Broadcaster` (clase), que limita la salida de mensajes mediante el `Timer` clase a partir de .NET framework.

    Puesto que el propio centro es transitorio (se crea cada vez que sea necesario), el `Broadcaster` se creará como un singleton. La inicialización diferida (introducida en .NET 4) se usa para diferir su creación hasta que sea necesario, lo que garantiza que la primera instancia de concentrador se crea por completo antes de que se inicia el temporizador.

    La llamada a los clientes `UpdateShape` función, a continuación, se mueve fuera del centro `UpdateModel` método, por lo que TI ya no se denomina inmediatamente cada vez que se reciben los mensajes entrantes. En su lugar, se enviará a una velocidad de 25 llamadas por segundo, los mensajes a los clientes administrados por el `_broadcastLoop` temporizador desde el `Broadcaster` clase.

    Por último, en lugar de llamar al método de cliente desde el centro directamente, la `Broadcaster` clase necesita obtener una referencia al centro de funcionamiento actualmente (`_hubContext`) mediante el `GlobalHost`.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlo en otra ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. No habrá una diferencia visible en el Explorador de la sección anterior, pero se limitará el número de mensajes que se envían al cliente.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Agregar una animación fluida en el cliente

La aplicación está casi completada, pero podríamos realizar una mayor mejora, el movimiento de la forma en el cliente cuando se desplace en respuesta a los mensajes del servidor. En lugar de establecer la posición de la forma a la nueva ubicación proporcionada por el servidor, vamos a usar la biblioteca de JQuery UI `animate` función para mover la forma sin problemas entre su posición actual y nuevo.

1. Actualizar el cliente `updateShape` , como método para buscar el código resaltado siguiente:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    El código anterior, mueve la forma de la ubicación anterior a la nueva proporcionado por el servidor en el transcurso del intervalo de animación (en este caso 100 milisegundos). Cualquier animación anterior que se ejecutan en la forma se borra antes de que comience la nueva animación.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlo en otra ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. El movimiento de la forma en la ventana debe aparecer menos irregular como su movimiento se interpola a través del tiempo, en lugar de que se va a establecer una vez por cada mensaje entrante.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Pasos adicionales

En este tutorial, ha aprendido cómo programar una aplicación de SignalR que envía mensajes de alta frecuencia entre clientes y servidores. Este paradigma de comunicación es útil para el desarrollo de juegos en línea y otras simulaciones, tales como [el juego ShootR creado con SignalR](http://shootr.signalr.net).

La aplicación completa creada en este tutorial se puede descargar desde [Galería de código](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Para obtener más información sobre los conceptos de desarrollo de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
