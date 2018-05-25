---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: Introducción a SignalR 2 y MVC 5 | Documentos de Microsoft'
author: pfletcher
description: Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 5 y crear una vista de chat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Tutorial: Introducción a SignalR 2 y MVC 5
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Este tutorial muestra cómo usar ASP.NET SignalR 2 para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 5 y crear una vista de chat para enviar y mostrar mensajes. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con ASP.NET SignalR 2 y 5 de MVC de ASP.NET. El tutorial utiliza el mismo código de aplicación de chat que la [tutorial de introducción SignalR](tutorial-getting-started-with-signalr.md), pero muestra cómo agregar a una aplicación de MVC 5.

En este tema aprenderá las tareas de desarrollo de SignalR siguientes:

- Agregar la biblioteca de SignalR a una aplicación de MVC 5.
- Crear clases de inicio OWIN para insertar contenido en los clientes y concentrador.
- Usa la biblioteca de jQuery de SignalR en una página web para enviar mensajes y muestra las actualizaciones desde el concentrador.

La captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.

![Instancias de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Requisitos previos:

- Visual Studio 2013. Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2013 Express herramienta de desarrollo.

En esta sección se muestra cómo crear una aplicación ASP.NET MVC 5, agregue la biblioteca de SignalR y crear la aplicación de chat.

1. En Visual Studio, cree una aplicación C# ASP.NET que tiene como destino .NET Framework 4.5, asígnele el nombre SignalRChat y haga clic en Aceptar.

    ![Crear sitio web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. En el `New ASP.NET Project` cuadro de diálogo y seleccione **MVC**y haga clic en **Cambiar autenticación**.

    ![Crear sitio web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Seleccione **sin autenticación** en el **Cambiar autenticación** cuadro de diálogo y haga clic en **Aceptar**.

    ![No seleccione autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Si selecciona un proveedor de autenticación diferentes para la aplicación, un `Startup.cs` clase se creará automáticamente; no lo necesitará crear sus propios `Startup.cs` clase en el paso 10 más abajo.
4. Haga clic en **Aceptar** en el **nuevo proyecto ASP.NET** cuadro de diálogo.
5. Abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecute el siguiente comando. Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitar la funcionalidad de SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. En **el Explorador de soluciones**, expanda la carpeta Scripts. Tenga en cuenta que las bibliotecas de scripts para SignalR se han agregado al proyecto.

    ![Carpeta de scripts](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Nueva carpeta**, y agrega una carpeta nueva denominada **concentradores**.
8. Haga clic en el **concentradores** carpeta, haga clic en **agregar | Nuevo elemento**, seleccione la **Visual C# | Web | SignalR** nodo en el **instalado** panel, seleccione **clase de concentrador de SignalR (v2)** desde el panel central y crear un nuevo concentrador denominado **ChatHub.cs**. Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.

    ![Crear nuevo centro](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Reemplace el código de la **ChatHub** clase con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Crear una nueva clase denominada Startup.cs. Cambiar el contenido del archivo a la siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Editar la `HomeController` encontrar en la clase **Controllers/HomeController.cs** y agregue el método siguiente a la clase. Este método devuelve el **Chat** vista que va a crear en un paso posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Haga clic en el **vistas/inicio** carpeta y seleccione **agregar... | Vista**.
13. En el **agregar vista** cuadro de diálogo, nombre de la nueva vista **Chat**.

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Reemplace el contenido de **Chat.cshtml** con el código siguiente.

    > [!IMPORTANT]
    > Cuando se agrega SignalR y otras bibliotecas de script a su proyecto de Visual Studio, el Administrador de paquetes puede instalar una versión del archivo de script de SignalR es más reciente que la versión que se muestra en este tema. Asegúrese de que la referencia de script en el código coincide con la versión de la biblioteca de secuencias de comandos instalada en su proyecto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración.
2. En la línea de dirección del explorador, anexar **/principal/chat** a la dirección URL de la página predeterminada para el proyecto. La página de Chat se cargue en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Escriba un nombre de usuario.
4. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más. En cada instancia del explorador, escriba un nombre de usuario único.
5. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde los comentarios de todos los usuarios actuales. Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.
6. La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador. Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery. Si utiliza un explorador que no sea Internet Explorer, también puede tener acceso a los dinámicos **concentradores** archivo desplazándose a él directamente, por ejemplo http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.

El **enviar** método muestra varios conceptos de base de datos central:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este concentrador.
- Llamar a una función en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

El **Chat.cshtml** archivo de vista en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR. Las tareas esenciales en el código va a crear una referencia para el proxy generado automáticamente para el concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara una referencia a un proxy de concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel. El ejemplo de código hace referencia a C# **ChatHub** clase en JavaScript como **chatHub**. Si desea hacer referencia a la `ChatHub` clase en jQuery con Pascal convencional mayúsculas y minúsculas como lo haría en C#, modifique el archivo de clase ChatHub.cs. Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres. A continuación, agregue el `HubName` atribuir a la `ChatHub` de la clase, por ejemplo `[HubName("ChatHub")]`. Por último, actualizar la referencia de jQuery para la `ChatHub` clase.


El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. La llamada opcional a la `htmlEncode` función muestra una manera de HTML codifica el contenido del mensaje antes de mostrarlo en la página, como una manera de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real. También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.

Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [utilizando SignalR con las aplicaciones Web en el servicio de aplicación de Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
