---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introducción con SignalR 1.x y MVC 4 | Documentos de Microsoft'
author: pfletcher
description: Usar ASP.NET SignalR y 4 de ASP.NET MVC para compilar una aplicación de chat en tiempo real.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Introducción con SignalR 1.x y MVC 4
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Este tutorial muestra cómo usar ASP.NET SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación de MVC 4 y crear una vista de chat para enviar y mostrar mensajes.


## <a name="overview"></a>Información general

Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con ASP.NET SignalR y ASP.NET MVC 4. El tutorial utiliza el mismo código de aplicación de chat que la [tutorial de introducción SignalR](tutorial-getting-started-with-signalr.md), pero muestra cómo agregar a una aplicación de MVC 4 basada en la plantilla de Internet.

En este tema aprenderá las tareas de desarrollo de SignalR siguientes:

- Agregar la biblioteca de SignalR a una aplicación de MVC 4.
- Crear una clase de base de datos central para insertar contenido en los clientes.
- Usa la biblioteca de jQuery de SignalR en una página web para enviar mensajes y muestra las actualizaciones desde el concentrador.

La captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.

![Instancias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Requisitos previos:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express. Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2012 Express herramienta de desarrollo.
- Para Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Esta sección muestra cómo crear una aplicación de ASP.NET MVC 4, agregue la biblioteca de SignalR y crear la aplicación de chat.

1. 1. En Visual Studio crea una aplicación de ASP.NET MVC 4, asígnele el nombre SignalRChat y haga clic en Aceptar.

        > [!NOTE]
        > En VS 2010, seleccione **.NET Framework 4** en el control de lista desplegable de versión de Framework. Código de SignalR se ejecuta en las versiones de .NET Framework 4 y 4.5.

        ![Crear sitio web de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Seleccione la plantilla de aplicación de Internet, desactive la opción de **crear un proyecto de prueba unitaria**y haga clic en Aceptar.

         ![Crear el sitio de internet de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecute el siguiente comando. Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitar la funcionalidad de SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. En **el Explorador de soluciones** expanda la carpeta Scripts. Tenga en cuenta que las bibliotecas de scripts para SignalR se han agregado al proyecto.

         ![Referencias de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Nueva carpeta**, y agrega una carpeta nueva denominada **concentradores**.
      6. Haga clic en el **concentradores** carpeta, haga clic en **agregar | Clase**y cree una nueva clase de C# denominada **ChatHub.cs**. Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.

> [!NOTE]
> Si usa Visual Studio 2012 y ha instalado el [actualización ASP.NET y Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), puede usar la nueva plantilla de elemento de SignalR para crear la clase de base de datos central. Para ello, haga clic en el **concentradores** carpeta, haga clic en **agregar | Nuevo elemento**, seleccione **clase de concentrador de SignalR (v1)** y el nombre de la clase **ChatHub.cs**.


1. Reemplace el código de la **ChatHub** clase con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Abra la **Global.asax** de archivos para el proyecto y agregue una llamada al método `RouteTable.Routes.MapHubs();` como la primera línea de código en el `Application_Start` método. Este código registra la ruta predeterminada para los concentradores SignalR y se debe llamar antes de registrar las otras rutas. Completado `Application_Start` método es similar al ejemplo siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Editar la `HomeController` encontrar en la clase **Controllers/HomeController.cs** y agregue el método siguiente a la clase. Este método devuelve el **Chat** vista que va a crear en un paso posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Pulse el botón derecho en el `Chat` método que acaba de crea y haga clic en **agregar vista** para crear un nuevo archivo de vista.
5. En el **agregar vista** cuadro de diálogo, asegúrese de que está seleccionada la casilla de verificación para **usar una página de diseño o maestra** (desactive las demás casillas de verificación) y, a continuación, haga clic en **agregar**.

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edite el nuevo archivo de vista denominado **Chat.cshtml**. Después de la &lt;h2&gt; etiqueta, pegue el siguiente &lt;div&gt; sección y `@section scripts` bloque de código en la página. Esta secuencia de comandos permite a la página enviar mensajes de chat y mostrar mensajes desde el servidor. El código completo de la vista de chat aparece en el siguiente bloque de código.

    > [!IMPORTANT]
    > Cuando se agrega SignalR y otras bibliotecas de script a su proyecto de Visual Studio, el Administrador de paquetes puede instalar las versiones de las secuencias de comandos que son más recientes que las versiones que se muestra en este tema. Asegúrese de que las referencias de script en el código coinciden con las versiones de las bibliotecas de scripts instaladas en su proyecto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración.
2. En la línea de dirección del explorador, anexar **/principal/chat** a la dirección URL de la página predeterminada para el proyecto. La página de Chat se cargue en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Escriba un nombre de usuario.
4. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más. En cada instancia del explorador, escriba un nombre de usuario único.
5. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde los comentarios de todos los usuarios actuales. Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.
6. La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador. Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery. Si utiliza un explorador que no sea Internet Explorer, también puede tener acceso a los dinámicos **concentradores** desplazándose hasta ella directamente, por ejemplo el archivo http://mywebsite/signalr/hubs.

    ![Script de concentrador generado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde scripts de jQuery en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.

El **enviar** método muestra varios conceptos de base de datos central:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este concentrador.
- Llamar a una función de jQuery en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

El **Chat.cshtml** archivo de vista en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR. Las tareas esenciales en el código va a crear una referencia para el proxy generado automáticamente para el concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara a un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel. El ejemplo de código hace referencia a C# **ChatHub** clase jQuery como **chatHub**. Si desea hacer referencia a la `ChatHub` clase en jQuery con Pascal convencional mayúsculas y minúsculas como lo haría en C#, modifique el archivo de clase ChatHub.cs. Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres. A continuación, agregue el `HubName` atribuir a la `ChatHub` de la clase, por ejemplo `[HubName("ChatHub")]`. Por último, actualizar la referencia de jQuery para la `ChatHub` clase.


El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. La llamada opcional a la `htmlEncode` función muestra una manera de HTML codifica el contenido del mensaje antes de mostrarlo en la página, como una manera de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real. También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.

Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
