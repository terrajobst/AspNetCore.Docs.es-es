---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Introducción a SignalR 2 y MVC 5 | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 5 y crear una vista de chat...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: a58b95adfb5d0165887b95abd3247d3a829aa882
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912233"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Tutorial: Introducción a SignalR 2 y MVC 5
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 5 y crear una vista de conversación para enviar y mostrar mensajes.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - MVC 5
> - Versión 2 de SignalR
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
>
>
> Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:
>
> - Actualización de su [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - En el instalador de plataforma Web, buscar e instalar **ASP.NET y Web Tools 2013.1 para Visual Studio 2012**. Este modo instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.
> - Algunas plantillas (como **clase de inicio OWIN**) no estará disponible; para ello, use un archivo de clase en su lugar.
>
>
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
>
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con SignalR 2 de ASP.NET y ASP.NET MVC 5. El tutorial utiliza el mismo código de aplicación de chat como el [tutorial de introducción a SignalR](tutorial-getting-started-with-signalr.md), pero se muestra cómo agregar a una aplicación MVC 5.

En este tema obtendrá información sobre las siguientes tareas de desarrollo de SignalR:

- Agregar la biblioteca de SignalR a una aplicación de MVC 5.
- Crear clases de inicio OWIN para insertar contenido en los clientes y concentrador.
- Uso de la biblioteca de jQuery SignalR en una página web para enviar mensajes y mostrar las actualizaciones desde el centro.

Captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.

![Instancias del chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Requisitos previos:

- Visual Studio 2013. Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2013 Express herramienta de desarrollo gratuita.

En esta sección se muestra cómo crear una aplicación ASP.NET MVC 5, agregue la biblioteca SignalR y crear la aplicación de chat.

1. En Visual Studio, cree una aplicación ASP.NET de C# que tiene como destino .NET Framework 4.5, asígnele el nombre SignalRChat y haga clic en Aceptar.

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. En el `New ASP.NET Project` cuadro de diálogo y seleccione **MVC**y haga clic en **Cambiar autenticación**.

    ![Creación de web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Seleccione **sin autenticación** en el **Cambiar autenticación** cuadro de diálogo y haga clic en **Aceptar**.

    ![No seleccione autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Si selecciona un proveedor de autenticación diferente para la aplicación, un `Startup.cs` clase se crearán automáticamente; no lo necesitará crear sus propios `Startup.cs` clase en el paso 10 más abajo.
4. Haga clic en **Aceptar** en el **nuevo proyecto ASP.NET** cuadro de diálogo.
5. Abrir el **Herramientas > Administrador de paquetes NuGet > consola del Administrador de paquetes** y ejecute el siguiente comando. Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitan la funcionalidad de SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. En **el Explorador de soluciones**, expanda la carpeta Scripts. Tenga en cuenta que se han agregado al proyecto de bibliotecas de scripts para SignalR.

    ![Carpeta de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Nueva carpeta**, y agregue una nueva carpeta denominada **Hubs**.
8. Haga clic en el **Hubs** carpeta, haga clic en **Add | Nuevo elemento**, seleccione el **Visual C# | Web | SignalR** nodo en el **instalado** panel, seleccione **clase de concentrador de SignalR (v2)** desde el panel central y crear un nuevo centro denominado **ChatHub.cs**. Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.

    ![Crear nuevo centro](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Reemplace el código en el **ChatHub** con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Cree una nueva clase denominada Startup.cs. Cambiar el contenido del archivo a la siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Editar el `HomeController` clase se encuentra en **Controllers/HomeController.cs** y agregue el siguiente método a la clase. Este método devuelve el **Chat** vista que va a crear en un paso posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Haga clic en el **vistas/inicio** carpeta y seleccione **Add... | Vista**.
13. En el **agregar vista** cuadro de diálogo, nombre de la nueva vista **Chat**.

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Reemplace el contenido de **Chat.cshtml** con el código siguiente.

    > [!IMPORTANT]
    > Al agregar SignalR y otras bibliotecas de script al proyecto de Visual Studio, el Administrador de paquetes puede instalar una versión del archivo de script de SignalR que es más reciente que la versión que se muestra en este tema. Asegúrese de que la referencia de script en el código coincide con la versión de la biblioteca de scripts instalada en su proyecto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración.
2. En la línea de dirección del explorador, anexe **/home/chat** a la dirección URL de la página predeterminada para el proyecto. La página de Chat se carga en una instancia del explorador y solicitará un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Escriba un nombre de usuario.
4. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos. En cada instancia del explorador, escriba un nombre de usuario único.
5. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.
6. Captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador. Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor. Si usa un explorador que no sea Internet Explorer, también puede acceder a la dinámica **hubs** archivo yendo a él directamente, por ejemplo http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.

El **enviar** método muestra varios conceptos de hub:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este centro.
- Llamar a una función en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

El **Chat.cshtml** Ver archivo en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR. Las tareas esenciales en el código están creando una referencia para el proxy generado automáticamente para el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al centro.

El código siguiente declara una referencia a un proxy de concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas. El ejemplo de código hace referencia a C# **ChatHub** clases en JavaScript como **chatHub**. Si desea hacer referencia a la `ChatHub` clase en jQuery con grafía Pascal convencional las mayúsculas y minúsculas como lo haría en C#, edite el archivo de clase ChatHub.cs. Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres. A continuación, agregue el `HubName` atributo a la `ChatHub` clase, por ejemplo `[HubName("ChatHub")]`. Por último, actualice la referencia de jQuery a la `ChatHub` clase.


El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. La llamada opcional para el `htmlEncode` función se muestra un camino hacia HTML codifica el contenido del mensaje antes de mostrarla en la página, como una manera de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.

Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [usando SignalR con Web Apps en Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
