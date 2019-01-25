---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Chat en tiempo real con SignalR 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación web ASP.NET vacía.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 90f2c03fbda522e3a46200bc0132cc74100ce70f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836797"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Tutorial: Chat en tiempo real con SignalR 2

Este tutorial muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregar SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.

En este tutorial ha:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examine el código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 y SignalR 2 para crear una aplicación web ASP.NET vacía, agregar SignalR y crear la aplicación de chat.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Creación de web](tutorial-getting-started-with-signalr/_static/image2.png)

1. En el **nuevo proyecto ASP.NET - SignalRChat** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - SignalRChat**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.

1. Nombre de la clase *ChatHub* y agréguelo al proyecto.

    Este paso se crea el *ChatHub.cs* archivo de clase y agrega un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR al proyecto.

1. Reemplace el código en el nuevo *ChatHub.cs* archivo de clase con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - SignalRChat** seleccione **instalado** > **Visual C#**   >  **Web** y, a continuación, Seleccione **clase de inicio OWIN**.

1. Nombre de la clase *inicio* y agréguelo al proyecto.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.

1. Nombre de la nueva página *índice* y seleccione **Aceptar**.

1. En **el Explorador de soluciones**, haga clic en la página HTML que creó y seleccione **establecer como página de inicio**.

1. Reemplace el código predeterminado en la página HTML con este código:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. En **el Explorador de soluciones**, expanda **Scripts**.

    Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    > [!IMPORTANT]
    > El Administrador de paquetes que ha instalado una versión posterior de los scripts de SignalR.

1. Compruebe que las referencias de script en el bloque de código se corresponden con las versiones de los archivos de script en el proyecto.

    Referencias de script desde el bloque de código original:
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Si no coinciden, actualice el *.html* archivo.

1. En la barra de menús, seleccione **archivo** > **guardar todo**.

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar el ejemplo en modo de depuración.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image3.png)

1. Cuando se abre el explorador, escriba un nombre de la identidad de chat.

1. Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.

1. En cada explorador, escriba un nombre único.

1. Ahora, agregue un comentario y seleccione **enviar**. Se repiten en los otros exploradores. Los comentarios aparecen en tiempo real.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.

    Vea cómo se ejecuta la aplicación de chat en tres diferentes exploradores. Cuando Tom, Anand y Susan envían mensajes, actualizarán todos los exploradores en tiempo real:

    ![Todos los tres exploradores mostrar al mismo historial de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado *hubs* que genera la biblioteca SignalR en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor.

    ![script de concentradores generado automáticamente en el nodo documentos de Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Examine el código

La aplicación SignalRChat muestra dos tareas básicas de desarrollo de SignalR. Muestra cómo crear un concentrador. El servidor usa ese concentrador como el objeto principal de coordinación. El concentrador usa la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentradores de SignalR en el ChatHub.cs

En el ejemplo de código anterior, el `ChatHub` clase se deriva de la `Microsoft.AspNet.SignalR.Hub` clase. Derivar de la `Hub` clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, usar estos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la `ChatHub.Send` método para enviar un mensaje nuevo. El concentrador, a continuación, envía el mensaje a todos los clientes mediante una llamada a `Clients.All.broadcastMessage`.

El `Send` método muestra varios conceptos de hub:

* Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.

* Use la `Microsoft.AspNet.SignalR.Hub.Clients` propiedades dinámicas para comunicarse con todos los clientes conectados a este centro.

* Llamar a una función en el cliente (como el `broadcastMessage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR y jQuery en el archivo index.html

El *index.html* página en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR. El código lleva a cabo muchas tareas importantes. Declara un proxy para hacer referencia al concentrador, declara una función que el servidor puede llamar para insertar contenido a los clientes y se inicia una conexión para enviar mensajes al concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> En JavaScript la referencia a la clase de servidor y sus miembros tiene que ser camelCase. Las referencias de ejemplo de código la C# *ChatHub* clases en JavaScript como `chatHub`.

En este bloque de código, cree una función de devolución de llamada en la secuencia de comandos.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. Las dos líneas que codificar en HTML el contenido antes de mostrarla son opcionales y mostrar una buena forma de evitar la inyección de script.

Este código abre una conexión con el centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Este enfoque garantiza que el código establece una conexión antes de que se ejecuta el controlador de eventos.

El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.

## <a name="get-the-code"></a>Obtención del código

[Descargue el proyecto completado](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información acerca de SignalR, consulte los siguientes recursos:

* [SignalR Project](http://signalr.net)

* [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial le:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examinar el código

Avance al siguiente artículo para obtener información sobre cómo usar SignalR y MVC 5.
> [!div class="nextstepaction"]
> [SignalR 2 y MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)