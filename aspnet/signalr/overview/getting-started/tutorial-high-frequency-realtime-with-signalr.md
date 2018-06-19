---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: En tiempo real alta frecuencia con SignalR 2 | Documentos de Microsoft'
author: pfletcher
description: Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR para proporcionar funcionalidad de mensajería de alta frecuencia. Alta frecuencia de mensajería en...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036120"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Tutorial: En tiempo real alta frecuencia con SignalR 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de mensajería de alta frecuencia. Alta frecuencia de mensajería en este caso significa que las actualizaciones que se envían a una velocidad fija; en el caso de esta aplicación, hasta 10 mensajes por segundo.
> 
> La aplicación que se creará en este tutorial muestra una forma que los usuarios pueden arrastrar. La posición de la forma en todos los otros exploradores conectados, a continuación, se actualizará para que coincida con la posición de la forma arrastrada mediante actualizaciones cronometradas.
> 
> Conceptos presentados en este tutorial tienen las aplicaciones de juegos en tiempo real y otras aplicaciones de simulación.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
> 
> 
> Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:
> 
> - Actualización de su [el Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - El instalador de plataforma Web, buscar e instalar **2013.1 de herramientas Web para Visual Studio 2012 y ASP.NET**. Esto instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.
> - Algunas plantillas (como **clase de inicio de OWIN**) no estará disponible; para ello, utilice un archivo de clase en su lugar.
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este tutorial muestra cómo crear una aplicación que comparte el estado de un objeto con otros exploradores en tiempo real. La aplicación que crearemos se denomina MoveShape. Mostrará la página de MoveShape un elemento HTML Div que el usuario puede arrastrar; Cuando el usuario arrastra el elemento Div, se enviará al servidor, que, a continuación, se le indicará todos los demás clientes conectados para actualizar la posición de la forma para que coincida con la nueva posición.

![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

La aplicación creada en este tutorial se basa en una demostración de Damian Edwards. Se puede ver un vídeo que contiene esta demostración [aquí](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

El tutorial se iniciará mostrando cómo enviar mensajes de SignalR de cada evento que activa tal y como se arrastra la forma. Cada cliente conectado, a continuación, actualizará a la posición de la versión local de la forma cada vez que se recibe un mensaje.

Aunque la aplicación funcionará con este método, no es un modelo de programación recomendado, ya que no habrá ningún límite superior para el número de mensajes que se envían, por lo que los clientes y el servidor pueden obtener desbordados con mensajes y podría degradar el rendimiento . La animación mostrada en el cliente también sería separado, como la forma se moverán al instante por cada método, en lugar de mover sin problemas a cada nueva ubicación. En secciones posteriores del tutorial mostrará cómo crear una función de temporizador que restringe la velocidad máxima a la que los mensajes se envían por el cliente o el servidor y cómo mover la forma sin problemas entre las ubicaciones. La versión final de la aplicación creada en este tutorial puede descargarse desde [Galería de código](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Crear el proyecto y agregar el paquete JQuery.UI NuGet y de SignalR](#createtheproject2013)
- [Crear la aplicación básica](#baseapp)
- [A partir del concentrador al iniciar la aplicación](#startup2013)
- [Agregue el bucle de cliente](#clientloop)
- [Agregue el bucle de servidor](#serverloop)
- [Agregar animación suave en el cliente](#animation)
- [Más acciones](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Este tutorial requiere Visual Studio 2013.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Crear el proyecto y agregar el paquete JQuery.UI NuGet y de SignalR

En esta sección, vamos a crear el proyecto en Visual Studio 2013.

Los pasos siguientes utilizan Visual Studio 2013 para crear una aplicación Web ASP.NET vacía y agregar las bibliotecas SignalR y jQuery.UI:

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Crear sitio web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. En el **nuevo proyecto ASP.NET** ventana, deje **vacía** seleccionada y haga clic en **crear proyecto**.

    ![Crear sitio web vacío](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Clase de concentrador de SignalR (v2)**. Nombre de la clase **MoveShapeHub.cs** y agregarlo al proyecto. Este paso se crea el **MoveShapeHub** clase y se agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR.

    > [!NOTE]
    > También puede agregar SignalR a un proyecto, haga clic en **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecutar un comando:

    `install-package Microsoft.AspNet.SignalR`. 

    Si utiliza la consola para agregar SignalR, cree la clase de concentrador de SignalR como un paso independiente después de agregar SignalR.
4. Haga clic en **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**. En la ventana del Administrador de paquetes, ejecute el siguiente comando:

    `Install-Package jQuery.UI.Combined`

    Esto instala la biblioteca de interfaz de usuario jQuery, que se va a utilizar para animar la forma.
5. En **el Explorador de soluciones** expanda el nodo de secuencias de comandos. Bibliotecas de scripts de jQuery, jQueryUI y SignalR están visibles en el proyecto.

    ![Referencias de biblioteca de script](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Crear la aplicación básica

En esta sección, vamos a crear una aplicación de explorador que envía la ubicación de la forma en el servidor durante cada evento de movimiento del mouse. El servidor difunde a continuación, esta información para todos los demás clientes conectados tal y como se reciben. A partir de esta aplicación en secciones posteriores.

1. Si aún no ha creado la clase MoveShapeHub.cs, en **el Explorador de soluciones**, haga doble clic en el proyecto y seleccione **agregar**, **clase...** . Nombre de la clase **MoveShapeHub** y haga clic en **agregar**.
2. Reemplace el código en el nuevo **MoveShapeHub** clase con el código siguiente.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    La `MoveShapeHub` clase anterior es una implementación de un concentrador de SignalR. Como en el [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, el concentrador tiene un método que los clientes llamarán directamente. En este caso, el cliente enviará un objeto que contiene las nuevas coordenadas X e Y de la forma en el servidor, que, a continuación, obtiene difunden a todos los demás clientes conectados. SignalR serializará automáticamente este objeto mediante JSON.

    El objeto que se enviará al cliente (`ShapeModel`) contiene los miembros para almacenar la posición de la forma. La versión del objeto en el servidor también contiene un miembro para realizar un seguimiento de los datos del cliente que se va a almacenar, para que un cliente determinado no enviar sus propios datos. Este miembro se utiliza el `JsonIgnore` atributo para impedir que se serialicen y se envía al cliente.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>A partir del concentrador al iniciar la aplicación

1. A continuación, configuraremos la asignación al concentrador cuando se inicia la aplicación. En SignalR 2, esto se hace mediante la adición de una clase de inicio OWIN, que llamará `MapSignalR` cuando la clase de inicio `Configuration` método se ejecuta cuando se inicia de OWIN. La clase se agrega al inicio de OWIN proceso utilizando el `OwinStartup` atributo de ensamblado.

    En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Clase de inicio OWIN**. Nombre de la clase *inicio* y haga clic en **Aceptar**.
2. Cambiar el contenido de Startup.cs a lo siguiente:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>Agregar el cliente

1. A continuación, vamos a agregar al cliente. En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **página Html**. Nombre de la página **Default.html** y haga clic en **agregar**.
2. En **el Explorador de soluciones**, haga clic en la página que acaba de crear y haga clic en **establecer como página de inicio**.
3. Reemplace el código predeterminado en la página HTML con el siguiente fragmento de código.

    > [!NOTE]
    > Compruebe que la secuencia de comandos hace referencia a continuación de la coincidencia de los paquetes que se agrega al proyecto en la carpeta Scripts.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    El código HTML y JavaScript anterior crea un elemento Div rojo llamado de forma, habilita el comportamiento de arrastrar la forma mediante la biblioteca de jQuery y usa la forma `drag` eventos a la posición de la forma de envío al servidor.
4. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlos en una segunda ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>Agregue el bucle de cliente

Puesto que el envío de la ubicación de la forma en cada evento de movimiento del mouse, se creará una cantidad innecesarios del tráfico de red, los mensajes desde el cliente que limitarán. Vamos a usar el código javascript `setInterval` función para establecer un bucle que envía información de posición de nuevo al servidor con una tarifa fija. Este bucle es una representación muy básica de un "juego bucle", una función llamada repetidamente que controla toda la funcionalidad de juegos u otro simulación.

1. Actualice el código de cliente en la página HTML para que coincida con el siguiente fragmento de código.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    La actualización anterior agrega el `updateServerModel` función, que se invoca en una frecuencia fija. Esta función envía los datos de la posición en el servidor cada vez que la `moved` marca indica que hay nuevos datos de posición para enviar.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlos en una segunda ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. Puesto que se reducirán el número de mensajes que se envíen al servidor, la animación no aparecerán como smooth como se muestra en la sección anterior.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Agregue el bucle de servidor

En la aplicación actual, los mensajes enviados desde el servidor al cliente hacerlo con tanta frecuencia como se reciben. Esto presenta un problema similar, tal y como se ha visto en el cliente; los mensajes pueden enviarse con más frecuencia que son necesarias y la conexión podría se inunde como resultado. En esta sección se describe cómo actualizar el servidor para implementar un temporizador que limita la velocidad de los mensajes salientes.

1. Reemplace el contenido de `MoveShapeHub.cs` con el siguiente fragmento de código.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    El código anterior expande el cliente que desea agregar el `Broadcaster` (clase), que limita la salida mensajes mediante la `Timer` clase a partir de .NET framework.

    Puesto que el centro de sí misma es transitorio (se crea cada vez que sea necesario), el `Broadcaster` se creará como un singleton. La inicialización diferida (introducida en .NET 4) se usa para diferir su creación hasta que sea necesario, asegurándose de que la primera instancia de base de datos central se crea por completo antes de que se inicia el temporizador.

    La llamada a los clientes `UpdateShape` función, a continuación, se mueve fuera del centro `UpdateModel` método, por lo que TI ya no se llama inmediatamente siempre que se reciben los mensajes entrantes. En su lugar, se enviarán los mensajes a los clientes a una velocidad de 25 llamadas por segundo, administrados por el `_broadcastLoop` temporizador desde el `Broadcaster` clase.

    Por último, en lugar de llamar al método de cliente desde el concentrador directamente, la `Broadcaster` clase necesita obtener una referencia a la central está en ejecución actualmente (`_hubContext`) mediante el `GlobalHost`.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlos en una segunda ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. No habrá una diferencia visible en el Explorador de la sección anterior, pero se limitarán el número de mensajes que se envíen al cliente.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>Agregar animación suave en el cliente

La aplicación es casi ha finalizado, pero que pudiéramos hacer que una mayor mejora, el movimiento de la forma en el cliente medida que se mueve en respuesta a los mensajes de servidor. En lugar de establecer la posición de la forma a la nueva ubicación proporcionada por el servidor, vamos a usar la biblioteca de JQuery UI `animate` función para mover la forma sin problemas entre su posición actual y la nueva.

1. Actualizar el cliente `updateShape` método para buscar al igual que el código que aparece resaltado a continuación:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    El código anterior, mueve la forma de la ubicación anterior al nuevo proporcionado por el servidor a lo largo del intervalo de animación (en este caso, 100 milisegundos). Cualquier animación anterior que se ejecuta en la forma se borra antes de que comience la nueva animación.
2. Inicie la aplicación presionando F5. Copiar dirección URL de la página y pegarlos en una segunda ventana del explorador. Arrastre la forma de una de las ventanas del explorador; debe mover la forma en la ventana del explorador. El movimiento de la forma en la otra ventana debe aparecer menos irregular como su movimiento se interpola por hora, en lugar de que se va a establecer una vez por cada mensaje entrante.

    ![La ventana de la aplicación](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Más acciones

En este tutorial, ha aprendido cómo programar una aplicación de SignalR que envía mensajes de alta frecuencia entre clientes y servidores. Este paradigma de comunicación es útil para el desarrollo de juegos en línea y otros simulaciones, como [el juego ShootR creado con SignalR](http://shootr.signalr.net).

La aplicación completa creada en este tutorial puede descargarse desde [Galería de código](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Para obtener más información sobre los conceptos de desarrollo de SignalR, visite los siguientes sitios de código fuente de SignalR y recursos:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Para ver un tutorial sobre cómo implementar una aplicación de SignalR en Azure, consulte [utilizando SignalR con las aplicaciones Web en el servicio de aplicación de Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
