---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Chat en tiempo real con SignalR 2 y MVC 5. | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación de MVC 5.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098609"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Tutorial: Chat en tiempo real con SignalR 2 y MVC 5

Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación de MVC 5 y crear una vista de conversación para enviar y mostrar mensajes.

En este tutorial, puede:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examine el código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 y SignalR 2 para crear una aplicación ASP.NET MVC 5 vacía, agregue la biblioteca SignalR y crear la aplicación de chat.

1. En Visual Studio, cree una aplicación ASP.NET de C# que tiene como destino .NET Framework 4.5, asígnele el nombre SignalRChat y haga clic en Aceptar.

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. En **nueva aplicación Web de ASP.NET - SignalRMvcChat**, seleccione **MVC** y, a continuación, seleccione **Cambiar autenticación**.

1. En **Cambiar autenticación**, seleccione **sin autenticación** y haga clic en **Aceptar**.

    ![No seleccione autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. En **nueva aplicación Web de ASP.NET - SignalRMvcChat**, seleccione **Aceptar**.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - SignalRChat**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.

1. Nombre de la clase *ChatHub* y agréguelo al proyecto.

    Este paso se crea el *ChatHub.cs* archivo de clase y agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.

1. Reemplace el código en el nuevo *ChatHub.cs* archivo de clase con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.

1. Nombre de la nueva clase *inicio* y agréguelo al proyecto.

1. Reemplace el código en el *Startup.cs* archivo de clase con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. En **el Explorador de soluciones**, seleccione **controladores** > **HomeController.cs**.

1. Agregue este método para el *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Este método devuelve el **Chat** vista que se crea en un paso posterior.

1. En **el Explorador de soluciones**, haga clic en **vistas** > **inicio**y seleccione **agregar**  >    **Vista**.

1. En **agregar vista**, nombre la nueva vista **Chat** y seleccione **agregar**.

1. Reemplace el contenido de **Chat.cshtml** con este código:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. En **el Explorador de soluciones**, expanda **Scripts**.

    Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    > [!IMPORTANT]
    > El Administrador de paquetes que ha instalado una versión posterior de los scripts de SignalR.

1. Compruebe que las referencias de script en el bloque de código se corresponden con las versiones de los archivos de script en el proyecto.

    Referencias de script desde el bloque de código original:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Si no coinciden, actualice el *.cshtml* archivo.

1. En la barra de menús, seleccione **archivo** > **guardar todo**.

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar el ejemplo en modo de depuración.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Cuando se abre el explorador, escriba un nombre de la identidad de chat.

1. Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.

1. En cada explorador, escriba un nombre único.

1. Ahora, agregue un comentario y seleccione **enviar**. Se repiten en los otros exploradores. Los comentarios aparecen en tiempo real.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.

    Vea cómo se ejecuta la aplicación de chat en tres diferentes exploradores. Cuando Tom, Anand y Susan envían mensajes, actualizarán todos los exploradores en tiempo real:

    ![Todos los tres exploradores mostrar al mismo historial de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado *hubs* que genera la biblioteca SignalR en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor.

    ![script de concentradores generado automáticamente en el nodo documentos de Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR. Muestra cómo crear un concentrador. El servidor usa ese concentrador como el objeto principal de coordinación. El concentrador usa la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentradores de SignalR en el ChatHub.cs

En el ejemplo de código, el `ChatHub` clase se deriva de la `Microsoft.AspNet.SignalR.Hub` clase. Derivar de la `Hub` clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la `ChatHub.Send` método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a `Clients.All.addNewMessageToPage`.

El `Send` método muestra varios conceptos de hub:

* Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.

* Use la `Microsoft.AspNet.SignalR.Hub.Clients` propiedades dinámicas para comunicarse con todos los clientes conectados a este centro.

* Llamar a una función en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR y jQuery Chat.cshtml

El *Chat.cshtml* Ver archivo en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR.  El código lleva a cabo muchas tareas importantes. Crea una referencia al proxy generado automáticamente para el concentrador, declara una función que el servidor puede llamar para insertar contenido en clientes y se inicia una conexión para enviar mensajes al concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros se encuentra en camelCase. Las referencias de ejemplo de código la C# `ChatHub` clases en JavaScript como `chatHub`.

En este bloque de código, cree una función de devolución de llamada en la secuencia de comandos.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. La llamada opcional para el `htmlEncode` función se muestra un camino hacia HTML codifica el contenido del mensaje antes de mostrarla en la página. Es una manera de evitar la inyección de script.

Este código abre una conexión con el centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Este enfoque garantiza que establecer una conexión antes de que se ejecuta el controlador de eventos.

El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información acerca de SignalR, consulte los siguientes recursos:

* [Proyecto de SignalR](http://signalr.net)

* [SignalR GitHub y ejemplos](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, puede:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examinar el código

Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de mensajería de alta frecuencia.
> [!div class="nextstepaction"]
> [Aplicación Web con la mensajería de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md)