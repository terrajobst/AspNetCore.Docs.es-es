---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introducción a SignalR 1.x y MVC 4 | Microsoft Docs'
author: pfletcher
description: Usar SignalR de ASP.NET y ASP.NET MVC 4 para crear una aplicación de chat en tiempo real.
ms.author: riande
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 34a7ae97a0a0652c090aa72e2cb21a4bce13bd5c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828487"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Introducción a SignalR 1.x y MVC 4
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Este tutorial muestra cómo usar SignalR de ASP.NET para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación MVC 4 y crear una vista de conversación para enviar y mostrar mensajes.


## <a name="overview"></a>Información general

Este tutorial presentan al desarrollo de aplicaciones web en tiempo real con SignalR de ASP.NET y ASP.NET MVC 4. El tutorial utiliza el mismo código de aplicación de chat como el [tutorial de introducción a SignalR](tutorial-getting-started-with-signalr.md), pero se muestra cómo agregar a una aplicación MVC 4, según la plantilla de Internet.

En este tema obtendrá información sobre las siguientes tareas de desarrollo de SignalR:

- Agregar la biblioteca de SignalR a una aplicación MVC 4.
- Creación de una clase de concentrador para insertar contenido en los clientes.
- Uso de la biblioteca de jQuery SignalR en una página web para enviar mensajes y mostrar las actualizaciones desde el centro.

Captura de pantalla siguiente muestra la aplicación de chat completada que se ejecuta en un explorador.

![Instancias del chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Requisitos previos:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express. Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2012 Express herramienta de desarrollo gratuita.
- Para Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

En esta sección se muestra cómo crear una aplicación ASP.NET MVC 4, agregue la biblioteca SignalR y crear la aplicación de chat.

1. 1. En Visual Studio cree una aplicación ASP.NET MVC 4, asígnele el nombre SignalRChat y haga clic en Aceptar.

        > [!NOTE]
        > En VS 2010, seleccione **.NET Framework 4** en el control desplegable de versión de Framework. Código de SignalR se ejecuta en las versiones de .NET Framework 4 y 4.5.

        ![Creación de web de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Seleccione la plantilla de aplicación de Internet, desactive la opción de **crear un proyecto de prueba unitaria**y haga clic en Aceptar.

         ![Crear sitio de internet de mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecute el siguiente comando. Este paso agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que habilitan la funcionalidad de SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. En **el Explorador de soluciones** expanda la carpeta Scripts. Tenga en cuenta que se han agregado al proyecto de bibliotecas de scripts para SignalR.

         ![Referencias de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Nueva carpeta**, y agregue una nueva carpeta denominada **Hubs**.
      6. Haga clic en el **Hubs** carpeta, haga clic en **Add | Clase**y cree una nueva clase de C# denominada **ChatHub.cs**. Usará esta clase como un concentrador de servidor de SignalR que envía mensajes a todos los clientes.

> [!NOTE]
> Si usa Visual Studio 2012 y ha instalado el [actualización ASP.NET y Web Tools 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), puede usar la nueva plantilla de elemento de SignalR para crear la clase de concentrador. Para ello, haga clic en el **Hubs** carpeta, haga clic en **Add | Nuevo elemento**, seleccione **clase de concentrador de SignalR (v1)** y el nombre de la clase **ChatHub.cs**.


1. Reemplace el código en el **ChatHub** con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Abra el **Global.asax** de archivos para el proyecto y agregue una llamada al método `RouteTable.Routes.MapHubs();` como la primera línea de código en el `Application_Start` método. Este código registra la ruta predeterminada para los concentradores de SignalR y debe llamarse antes de registrar las otras rutas. Completado `Application_Start` método es similar al ejemplo siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Editar el `HomeController` clase se encuentra en **Controllers/HomeController.cs** y agregue el siguiente método a la clase. Este método devuelve el **Chat** vista que va a crear en un paso posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Haga doble clic en el `Chat` método que acaba de crea y haga clic en **agregar vista** para crear un nuevo archivo de vista.
5. En el **agregar vista** cuadro de diálogo, asegúrese de que la casilla de verificación para **utiliza una página de diseño o maestra** (desactive las casillas) y, a continuación, haga clic en **agregar**.

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edite el archivo de vista nuevo denominado **Chat.cshtml**. Después de la &lt;h2&gt; etiquetar, pegue el siguiente &lt;div&gt; sección y `@section scripts` bloque de código en la página. Este script permite a la página enviar mensajes de chat y mostrar mensajes desde el servidor. El código completo de la vista de conversación aparece en el siguiente bloque de código.

    > [!IMPORTANT]
    > Al agregar SignalR y otras bibliotecas de script al proyecto de Visual Studio, el Administrador de paquetes puede instalar las versiones de los scripts que son más recientes que las versiones que se muestran en este tema. Asegúrese de que las referencias de script en el código coinciden con las versiones de las bibliotecas de scripts instaladas en su proyecto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración.
2. En la línea de dirección del explorador, anexe **/home/chat** a la dirección URL de la página predeterminada para el proyecto. La página de Chat se carga en una instancia del explorador y solicitará un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Escriba un nombre de usuario.
4. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos. En cada instancia del explorador, escriba un nombre de usuario único.
5. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.
6. Captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador.

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Este nodo está visible en modo de depuración si está usando Internet Explorer como explorador. Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor. Si usa un explorador que no sea Internet Explorer, también puede acceder a la dinámica **hubs** archivo yendo a él directamente, por ejemplo http://mywebsite/signalr/hubs.

    ![Script hub generado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde scripts de jQuery en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.addNewMessageToPage**.

El **enviar** método muestra varios conceptos de hub:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedad para tener acceso a todos los clientes conectados a este centro.
- Llamar a una función de jQuery en el cliente (como el `addNewMessageToPage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

El **Chat.cshtml** Ver archivo en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR. Las tareas esenciales en el código están creando una referencia para el proxy generado automáticamente para el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al centro.

El código siguiente declara a un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas. El ejemplo de código hace referencia a C# **ChatHub** clase en jQuery como **chatHub**. Si desea hacer referencia a la `ChatHub` clase en jQuery con grafía Pascal convencional las mayúsculas y minúsculas como lo haría en C#, edite el archivo de clase ChatHub.cs. Agregar un `using` instrucción para hacer referencia a la `Microsoft.AspNet.SignalR.Hubs` espacio de nombres. A continuación, agregue el `HubName` atributo a la `ChatHub` clase, por ejemplo `[HubName("ChatHub")]`. Por último, actualice la referencia de jQuery a la `ChatHub` clase.


El código siguiente muestra cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. La llamada opcional para el `htmlEncode` función se muestra un camino hacia HTML codifica el contenido del mensaje antes de mostrarla en la página, como una manera de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página de Chat.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.

Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
