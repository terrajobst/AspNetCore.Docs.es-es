---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Introducción a SignalR 2 | Microsoft Docs'
author: pfletcher
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree a un pa HTML...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 676dc0854ef6f041e705ed6b39432e11dd8643ed
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910907"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Tutorial: Introducción a SignalR 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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

Este tutorial presenta SignalR desarrollo mostrándole cómo compilar una aplicación de chat basada en explorador sencillo. Agregará la biblioteca SignalR para una aplicación web vacía de ASP.NET, cree una clase de hub para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat. Para ver un tutorial similar que muestra cómo crear una aplicación de chat en MVC 5 con una vista de MVC, consulte [Introducción a SignalR 2 y MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Este tutorial muestra cómo crear aplicaciones SignalR en la versión 2. Para obtener más información sobre los cambios entre SignalR 1.x y 2, consulte [SignalR actualizar proyectos de 1.x](../releases/upgrading-signalr-1x-projects-to-20.md) y [notas de la versión de Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR es una biblioteca de .NET de código abierto para compilar aplicaciones web que requieren la interacción del usuario en directo o actualizaciones de datos en tiempo real. Algunos ejemplos son aplicaciones sociales, juegos multiusuario, meteorológicos de colaboración y de noticias, business o aplicaciones financieras de actualización. A menudo se denominan aplicaciones en tiempo real.

SignalR simplifica el proceso de creación de aplicaciones en tiempo real. Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para facilitar la administración de conexiones de cliente / servidor e insertar las actualizaciones de contenido a los clientes. Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener la funcionalidad en tiempo real.

El tutorial muestra las siguientes tareas de desarrollo de SignalR:

- Agregar la biblioteca de SignalR a una aplicación web ASP.NET.
- Creación de una clase de concentrador para insertar contenido en los clientes.
- Creación de una clase de inicio OWIN para configurar la aplicación.
- Uso de la biblioteca de jQuery SignalR en una página web para enviar mensajes y mostrar las actualizaciones desde el centro.

Captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador. Cada nuevo usuario puede publicar comentarios y ver comentarios agregados después de que el usuario une a la conversación.

![Instancias del chat](tutorial-getting-started-with-signalr/_static/image1.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo usar Visual Studio 2013 y la versión 2 de SignalR para crear una aplicación web ASP.NET vacía, agregar SignalR y crear la aplicación de chat.

Requisitos previos:

- Visual Studio 2013. Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2013 Express herramienta de desarrollo gratuita.

Los pasos siguientes usan Visual Studio 2013 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca SignalR:

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Creación de web](tutorial-getting-started-with-signalr/_static/image2.png)
2. En el **nuevo proyecto ASP.NET** ventana, deje **vacía** seleccionado y haga clic en **crear proyecto**.

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image3.png)
3. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Clase de concentrador SignalR (v2)**. Nombre de la clase **ChatHub.cs** y agréguelo al proyecto. Este paso se crea el **ChatHub** clase y un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR se agrega al proyecto.

    > [!NOTE]
    > También puede agregar SignalR a un proyecto abriendo el **Herramientas > Administrador de paquetes NuGet > consola del Administrador de paquetes** y ejecutar un comando:

    `install-package Microsoft.AspNet.SignalR`

    Si utiliza la consola para agregar SignalR, cree la clase de concentrador SignalR como un paso independiente después de agregar SignalR.

    > [!NOTE]
    > Si usa Visual Studio 2012, el **clase de concentrador de SignalR (v2)** plantilla no estará disponible. Puede agregar un simple **clase** llamado `ChatHub` en su lugar.
4. En **el Explorador de soluciones**, expanda el nodo secuencias de comandos. Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.
5. Reemplace el código en el nuevo **ChatHub** con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Clase de inicio OWIN**. Nombre de la nueva clase `Startup` y haga clic en Aceptar.

    > [!NOTE]
    > Si usa Visual Studio 2012, el **clase de inicio OWIN** plantilla no estará disponible. Puede agregar un simple **clase** llamado `Startup` en su lugar.
7. Cambiar el contenido de la nueva clase de inicio a la siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Página HTML**. Nombre de la nueva página `index.html`.
    >[!NOTE]
    >Es posible que deba cambiar los números de versión para las referencias a bibliotecas de JQuery y SignalR
9. En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.
10. Reemplace el código predeterminado en la página HTML con el código siguiente.

    > [!NOTE]
    > Una versión posterior de los scripts de SignalR puede instalarse mediante el Administrador de paquetes. Compruebe que las referencias del script se corresponden con las versiones de los archivos de script en el proyecto (que será diferentes si agrega mediante NuGet, en lugar de agregar un concentrador de SignalR.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración. Carga la página HTML en una instancia del explorador y solicitará un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image4.png)
2. Escriba un nombre de usuario.
3. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos. En cada instancia del explorador, escriba un nombre de usuario único.
4. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.

    Captura de pantalla siguiente muestra la aplicación de chat que se ejecutan en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.

El **enviar** método muestra varios conceptos de hub:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectados a este centro.
- Llamar a una función en el cliente (como el `broadcastMessage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

La página HTML en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR. Las tareas esenciales en el código declara un proxy para hacer referencia el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara una referencia a un proxy de concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas. El ejemplo de código hace referencia a C# **ChatHub** clases en JavaScript como **chatHub**.


El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. Las dos líneas que codifica como HTML el contenido antes de mostrarla son opcionales y muestran una manera sencilla de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.

Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [usando SignalR con Web Apps en Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
