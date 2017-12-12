---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "Tutorial: Introducción a SignalR 2 | Documentos de Microsoft"
author: pfletcher
description: "En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y crear a un pa HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3bec32b9b21325cde461541d7a313f401a0cfce7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2"></a>Tutorial: Introducción a SignalR 2
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes. 
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

Este tutorial presentan SignalR desarrollo mostrándole cómo crear una aplicación de chat simple basada en explorador. Agregará la biblioteca de SignalR a una aplicación web ASP.NET vacía, cree una clase de base de datos central para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat. Para obtener un tutorial similar que muestra cómo crear una aplicación de chat en MVC 5 mediante una vista de MVC, vea [Getting Started with SignalR 2 y MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Este tutorial muestra cómo crear aplicaciones SignalR en la versión 2. Para obtener más información sobre los cambios entre SignalR 1.x y 2, consulte [SignalR actualizar 1.x proyectos](../releases/upgrading-signalr-1x-projects-to-20.md) y [notas de versión de Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR es una biblioteca de .NET de código abierto para la creación de aplicaciones web que requieren la interacción del usuario en vivo o las actualizaciones de datos en tiempo real. Algunos ejemplos son aplicaciones sociales, juegos multiusuario, el tiempo de colaboración y de noticias, business o aplicaciones de actualización financiera. A menudo se denominan aplicaciones en tiempo real.

SignalR simplifica el proceso de creación de aplicaciones en tiempo real. Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para que resulten más fáciles de administrar las conexiones de cliente y servidor e insertar las actualizaciones de contenido a los clientes. Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener una funcionalidad en tiempo real.

El tutorial muestra las tareas de desarrollo de SignalR siguientes:

- Agregar la biblioteca de SignalR a una aplicación web ASP.NET.
- Crear una clase de base de datos central para insertar contenido en los clientes.
- Creación de una clase de inicio OWIN para configurar la aplicación.
- Usa la biblioteca de jQuery de SignalR en una página web para enviar mensajes y muestra las actualizaciones desde el concentrador.

La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en un explorador. Cada nuevo usuario puede registrar comentarios y ver comentarios agregados después de que el usuario une a la conversación.

![Instancias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

Secciones:

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examine el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Esta sección muestra cómo usar Visual Studio 2013 y SignalR versión 2 para crear una aplicación web ASP.NET vacía, agregue SignalR y crear la aplicación de chat.

Requisitos previos:

- Visual Studio 2013. Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2013 Express herramienta de desarrollo.

Los pasos siguientes utilizan Visual Studio 2013 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca de SignalR:

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Crear sitio web](tutorial-getting-started-with-signalr/_static/image2.png)
2. En el **nuevo proyecto ASP.NET** ventana, deje **vacía** seleccionada y haga clic en **crear proyecto**.

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image3.png)
3. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Clase de concentrador de SignalR (v2)**. Nombre de la clase **ChatHub.cs** y agregarlo al proyecto. Este paso se crea el **ChatHub** clase y se agrega al proyecto un conjunto de archivos de script y las referencias de ensamblado que admiten SignalR.

    > [!NOTE]
    > También puede agregar SignalR a un proyecto, abra el **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes** y ejecutar un comando:

    `install-package Microsoft.AspNet.SignalR`

    Si utiliza la consola para agregar SignalR, cree la clase de concentrador de SignalR como un paso independiente después de agregar SignalR.

    > [!NOTE]
    > Si usa Visual Studio 2012, el **clase de concentrador de SignalR (v2)** plantilla no estará disponible. Puede agregar un simple **clase** denominado `ChatHub` en su lugar.
4. En **el Explorador de soluciones**, expanda el nodo de secuencias de comandos. Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.
5. Reemplace el código en el nuevo **ChatHub** clase con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Clase de inicio OWIN**. La nueva clase el nombre `Startup` y haga clic en Aceptar.

    > [!NOTE]
    > Si usa Visual Studio 2012, el **clase de inicio de OWIN** plantilla no estará disponible. Puede agregar un simple **clase** denominado `Startup` en su lugar.
7. Cambiar el contenido de la nueva clase de inicio al siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Página HTML**. Nombre de la nueva página `index.html`.
    >[!NOTE]
    >Tendrá que cambiar los números de versión para las referencias a bibliotecas de JQuery y SignalR
9. En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.
10. Reemplace el código predeterminado en la página HTML con el código siguiente.

    > [!NOTE]
    > El Administrador de paquetes puede instalarse una versión posterior de las secuencias de comandos de SignalR. Compruebe que las referencias de script siguientes corresponden a las versiones de los archivos de script en el proyecto (que será diferentes si ha agregado usando NuGet en lugar de agregar un concentrador de SignalR.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración. La página HTML se carga en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image4.png)
2. Escriba un nombre de usuario.
3. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más. En cada instancia del explorador, escriba un nombre de usuario único.
4. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde los comentarios de todos los usuarios actuales. Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.

    La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde secuencias de comandos en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.

El **enviar** método muestra varios conceptos de base de datos central:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectan a este concentrador.
- Llamar a una función en el cliente (como el `broadcastMessage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

La página HTML en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR. Las tareas esenciales en el código declara un proxy para hacer referencia al concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara una referencia a un proxy de concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel. El ejemplo de código hace referencia a C# **ChatHub** clase en JavaScript como **chatHub**.


El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. Las dos líneas que HTML codificar el contenido antes de mostrarlo son opcionales y mostrar una manera sencilla de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real. También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.

Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR en Azure, consulte [utilizando SignalR con las aplicaciones Web en el servicio de aplicación de Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
