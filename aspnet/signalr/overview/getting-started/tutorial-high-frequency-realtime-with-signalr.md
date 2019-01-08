---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Creación de aplicaciones de alta frecuencia en tiempo real con SignalR 2 | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo crear una aplicación web que usa SignalR de ASP.NET para proporcionar funcionalidad de mensajería de alta frecuencia.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098595"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Tutorial: Creación de aplicaciones de alta frecuencia en tiempo real con SignalR 2

Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de mensajería de alta frecuencia. En este caso, "mensajería de alta frecuencia" significa que el servidor envía actualizaciones a una tarifa fija. Enviar hasta 10 mensajes por segundo.

La aplicación que crea muestra una forma que los usuarios pueden arrastrar. El servidor actualiza la posición de la forma en todos los exploradores conectados para que coincida con la posición de la forma arrastrada mediante actualizaciones periódicas.

Los conceptos presentados en este tutorial tienen aplicaciones de juegos en tiempo real y otras aplicaciones de la simulación.

En este tutorial, puede:

> [!div class="checklist"]
> * Configurar el proyecto
> * Crear la aplicación básica
> * Cuando se inicia la aplicación se asignan al concentrador
> * Agregar el cliente
> * Ejecutar la aplicación
> * Agregar el bucle de cliente
> * Agregar bucle de servidor
> * Agregar una animación fluida

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección, creará el proyecto en Visual Studio 2017.

En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía y agregar las bibliotecas de SignalR y jQuery.UI.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Creación de web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. En el **nueva aplicación Web de ASP.NET - MoveShapeDemo** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - MoveShapeDemo**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.

1. Nombre de la clase *MoveShapeHub* y agréguelo al proyecto.

    Este paso se crea el *MoveShapeHub.cs* archivo de clase. Al mismo tiempo, agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.

1. Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.

1. En **Package Manager Console**, ejecute este comando:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    El comando instala la biblioteca de interfaz de usuario de jQuery. Usa para animar la forma.

1. En **el Explorador de soluciones**, expanda el nodo secuencias de comandos.

    ![Referencias a bibliotecas de script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Bibliotecas de scripts de jQuery, jQueryUI y SignalR están visibles en el proyecto.

## <a name="create-the-base-application"></a>Crear la aplicación básica

En esta sección, creará una aplicación de explorador. La aplicación envía la ubicación de la forma en el servidor durante cada evento de mover el mouse. El servidor transmite esta información para todos los clientes conectados en tiempo real. Aprenderá más acerca de esta aplicación en secciones posteriores.

1. Abra el *MoveShapeHub.cs* archivo.

1. Reemplace el código en el *MoveShapeHub.cs* archivo con este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Guarde el archivo.

La `MoveShapeHub` clase es una implementación de un concentrador SignalR. Como en el [Introducción a SignalR](tutorial-getting-started-with-signalr.md) tutorial, el concentrador tiene un método que llaman directamente los clientes. En este caso, el cliente envía un objeto con la nueva X y las coordenadas de la forma en el servidor. Esas coordenadas obtengan difunden a todos los demás clientes conectados. SignalR serializa automáticamente este objeto mediante JSON.

Los envíos de la aplicación la `ShapeModel` objeto al cliente. Tiene miembros para almacenar la posición de la forma. La versión del objeto en el servidor también tiene un miembro para realizar un seguimiento de los datos del cliente que se va a almacenar. Este objeto impide que el servidor envíe datos de un cliente a sí mismo. Este miembro se usa el `JsonIgnore` atributo para mantener la aplicación de serializar los datos y se envían al cliente.

## <a name="map-to-the-hub-when-app-starts"></a>Cuando se inicia la aplicación se asignan al concentrador

A continuación, configurar la asignación al concentrador cuando se inicia la aplicación. En SignalR 2, la adición de una clase de inicio OWIN crea la asignación.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - MoveShapeDemo** seleccione **instalado** > **Visual C#**   >  **Web** y, a continuación, Seleccione **clase de inicio OWIN**.

1. Nombre de la clase *inicio* y seleccione **Aceptar**.

1. Reemplace el código predeterminado en el *Startup.cs* archivo con este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

La clase de inicio OWIN llama `MapSignalR` cuando se ejecuta la aplicación la `Configuration` método. La aplicación agrega la clase de inicio del OWIN proceso utilizando el `OwinStartup` atributo de ensamblado.

## <a name="add-the-client"></a>Agregar el cliente

Agregue la página HTML para el cliente.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.

1. Nombre de la página **predeterminado** y seleccione **Aceptar**.

1. En **el Explorador de soluciones**, haga clic en *Default.html* y seleccione **establecer como página de inicio**.

1. Reemplace el código predeterminado en el *Default.html* archivo con este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. En **el Explorador de soluciones**, expanda **Scripts**.

    Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    > [!IMPORTANT]
    > El Administrador de paquetes instala una versión posterior de los scripts de SignalR.

1. Actualizar las referencias de script en el bloque de código para que coincida con las versiones de los archivos de script en el proyecto.

Este código HTML y JavaScript crea una red `div` llamado `shape`. Se habilita el comportamiento de arrastrar la forma mediante la biblioteca de jQuery y se usa el `drag` eventos a la posición de la forma de envío al servidor.

## <a name="run-the-app"></a>Ejecutar la aplicación

Puede ejecutar la aplicación a se'e funciona. Cuando arrastre la forma en torno a una ventana del explorador, la forma se desplaza demasiado en los otros exploradores.

1. En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar la aplicación en modo de depuración.

    ![Captura de pantalla del usuario activar el modo de depuración y seleccione Reproducir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Se abre una ventana del explorador con la forma de color rojo en la esquina superior derecha.

1. Copie la dirección URL de la página.

1. Abra un explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma de una de las ventanas del explorador. Sigue la forma en la ventana del explorador.

Mientras que la aplicación de las funciones con este método, no es un modelo de programación recomendado. No hay ningún límite superior para el número de mensajes que se envían. Como resultado, los clientes y el servidor obtengan experimentando con mensajes y degrada el rendimiento. Además, la aplicación muestra una animación separada en el cliente. Esta animación irregular se produce debido a la forma se mueve al instante por cada método. Es mejor si la forma se moverá instantáneamente a cada nueva ubicación. A continuación, aprenderá a corregir esos problemas.

## <a name="add-the-client-loop"></a>Agregar el bucle de cliente

Envío de la ubicación de la forma en cada evento de mover el mouse, crea un volumen de tráfico de red innecesario. La aplicación debe limitar los mensajes desde el cliente.

Utilice el código javascript `setInterval` función para establecer un bucle que envía información de posición de nuevo al servidor con una tarifa fija. Este bucle es una representación básica de un "bucle de juego". Es una función varias veces a que controla toda la funcionalidad de un juego.

1. Reemplace el código de cliente en el *Default.html* archivo con este código:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Tiene que reemplazar las referencias de script nuevo. Deben coincidir con las versiones de las secuencias de comandos en el proyecto.

    Este nuevo código agrega el `updateServerModel` función. Se llama a una frecuencia fija. La función envía los datos de la posición en el servidor cada vez que el `moved` marca indica que hay nuevos datos de posición para enviar.

1. Seleccione el botón Reproducir para iniciar la aplicación

1. Copie la dirección URL de la página.

1. Abra un explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma de una de las ventanas del explorador. Sigue la forma en la ventana del explorador.

Puesto que la aplicación limita al número de mensajes que se envían al servidor, la animación no aparece nítida como hizo al principio.

## <a name="add-the-server-loop"></a>Agregar bucle de servidor

En la aplicación actual, los mensajes enviados desde el servidor al cliente salen tan a menudo cuando se reciben. Este tráfico de red presenta un problema similar, como veremos en el cliente.

La aplicación puede enviar mensajes con más frecuencia son necesarios. La conexión puede inundarse como resultado. En esta sección se describe cómo actualizar el servidor para agregar un temporizador que limita la velocidad de los mensajes salientes.

1. Reemplace el contenido de `MoveShapeHub.cs` con este código:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Seleccione el botón Reproducir para iniciar la aplicación.

1. Copie la dirección URL de la página.

1. Abra un explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma de una de las ventanas del explorador.

Este código expande el cliente para agregar la `Broadcaster` clase. La nueva clase limita los mensajes salientes con el `Timer` clase a partir de .NET framework.

Es bueno saber que el propio centro es transitorio. Se crea cada vez que sea necesario. Por lo que la aplicación crea el `Broadcaster` como un singleton. Usa la inicialización diferida para aplazar la `Broadcaster`de creación hasta que sea necesario. Que garantiza que la aplicación crea la primera instancia de concentrador por completo antes de iniciar el temporizador.

La llamada a los clientes `UpdateShape` función, a continuación, se mueve fuera del centro `UpdateModel` método. Ya no se llama inmediatamente siempre que la aplicación recibe los mensajes entrantes. En su lugar, la aplicación envía los mensajes a los clientes a una velocidad de 25 llamadas por segundo. El proceso es administrado por el `_broadcastLoop` temporizador desde el `Broadcaster` clase.

Por último, en lugar de llamar al método de cliente desde el centro directamente, la `Broadcaster` clase necesita para obtener una referencia a opera actualmente `_hubContext` concentrador. Obtiene la referencia con la `GlobalHost`.

## <a name="add-smooth-animation"></a>Agregar una animación fluida

La aplicación es casi hemos terminada, pero podemos hacer una sola mejora más. La aplicación mueve la forma en el cliente en respuesta a los mensajes del servidor. En lugar de establecer la posición de la forma a la nueva ubicación proporcionada por el servidor, utilice la biblioteca de JQuery UI `animate` función. La forma se puede mover sin problemas entre su posición actual y el nuevo.

1. Actualizar el cliente `updateShape` método en el *Default.html* archivo para examinar como el código resaltado:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Seleccione el botón Reproducir para iniciar la aplicación.

1. Copie la dirección URL de la página.

1. Abra un explorador y pegue la dirección URL en la barra de direcciones.

1. Arrastre la forma de una de las ventanas del explorador.

El movimiento de la forma en la ventana aparece menos irregular. La aplicación interpola su movimiento a través del tiempo, en lugar de que se va a establecer una vez por cada mensaje entrante.

Este código mueve la forma de la ubicación anterior a la nueva. El servidor proporciona la posición de la forma en el transcurso del intervalo de animación. En este caso, es 100 milisegundos. La aplicación borra cualquier animación anterior que se ejecutan en la forma antes de que comience la nueva animación.

## <a name="additional-resources"></a>Recursos adicionales

Es útil para el desarrollo de juegos en línea y otras simulaciones, al igual que el paradigma de comunicación que acaba de aprender sobre [el juego ShootR creado con SignalR](https://shootr.azurewebsites.net/).

Para obtener más información acerca de SignalR, consulte los siguientes recursos:

* [Proyecto de SignalR](http://signalr.net)

* [SignalR GitHub y ejemplos](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, puede:

> [!div class="checklist"]
> * Configurar el proyecto
> * Crea la aplicación básica
> * Asignado al concentrador cuando se inicia la aplicación
> * Agregó el cliente
> * Se ejecutó la aplicación
> * Agrega el bucle de cliente
> * Agrega el bucle de servidor
> * Se ha agregado una animación fluida

Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor.
> [!div class="nextstepaction"]
> [SignalR 2 y difusión de servidor](tutorial-server-broadcast-with-signalr.md)