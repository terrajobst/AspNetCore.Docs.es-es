---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Tutorial: Introducción con SignalR 1.x | Documentos de Microsoft"
author: pfletcher
description: "Use ASP.NET SignalR para compilar una aplicación de chat en tiempo real en una página HTML."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Introducción con SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.


## <a name="overview"></a>Información general

Este tutorial presentan SignalR desarrollo mostrándole cómo crear una aplicación de chat simple basada en explorador. Agregará la biblioteca de SignalR a una aplicación web ASP.NET vacía, cree una clase de base de datos central para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat. Para obtener un tutorial similar que muestra cómo crear una aplicación de chat en MVC 4 mediante una vista de MVC, vea [Getting Started with SignalR y MVC 4](index.md).

> [!NOTE]
> Este tutorial usa la versión de lanzamiento (1.x) de SignalR. Para obtener más información sobre los cambios entre SignalR 1.x y 2.0, consulte [SignalR actualizar 1.x proyectos](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR es una biblioteca de .NET de código abierto para la creación de aplicaciones web que requieren la interacción del usuario en vivo o las actualizaciones de datos en tiempo real. Algunos ejemplos son aplicaciones sociales, juegos multiusuario, el tiempo de colaboración y de noticias, business o aplicaciones de actualización financiera. A menudo se denominan aplicaciones en tiempo real.

SignalR simplifica el proceso de creación de aplicaciones en tiempo real. Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para que resulten más fáciles de administrar las conexiones de cliente y servidor e insertar las actualizaciones de contenido a los clientes. Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener una funcionalidad en tiempo real.

El tutorial muestra las tareas de desarrollo de SignalR siguientes:

- Agregar la biblioteca de SignalR a una aplicación web ASP.NET.
- Crear una clase de base de datos central para insertar contenido en los clientes.
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

Esta sección muestra cómo crear una aplicación web ASP.NET vacía, agregue SignalR y crear la aplicación de chat.

Requisitos previos:

- Visual Studio 2010 SP1 o 2012. Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener la libre Visual Studio 2012 Express herramienta de desarrollo.
- [Microsoft Web ASP.NET y herramientas 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Para Visual Studio 2012, este instalador agrega nuevas características ASP.NET, incluidas las plantillas de SignalR a Visual Studio. Para Visual Studio 2010 SP1, un instalador no está disponible, pero puede completar el tutorial instalando el paquete SignalR NuGet tal y como se describe en los pasos de configuración.

Los pasos siguientes utilizan Visual Studio 2012 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca de SignalR:

1. En Visual Studio, cree una aplicación Web ASP.NET vacía.

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image2.png)
2. Abra la **Package Manager Console** seleccionando **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**. Escriba el siguiente comando en la ventana de consola:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Este comando instala la versión más reciente de SignalR 1.x.
3. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar | Clase**. La nueva clase el nombre **ChatHub**.
4. En **el Explorador de soluciones** expanda el nodo de secuencias de comandos. Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    ![Referencias de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. Reemplace el código de la **ChatHub** clase con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **clase de aplicación Global** y haga clic en **agregar**.

    ![Agregar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Agregue el siguiente `using` instrucciones después proporcionado `using` las instrucciones de la clase Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Agregue la siguiente línea de código en el `Application_Start` método de la clase Global para registrar la ruta predeterminada para los concentradores de SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. En **el Explorador de soluciones**, haga clic en el proyecto, a continuación, haga clic en **agregar | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la página Html y haga clic en **agregar**.
10. En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.
11. Reemplace el código predeterminado en la página HTML con el código siguiente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración. La página HTML se carga en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image5.png)
2. Escriba un nombre de usuario.
3. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir dos instancias del explorador más. En cada instancia del explorador, escriba un nombre de usuario único.
4. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde los comentarios de todos los usuarios actuales. Los usuarios que se unan a la conversación más adelante verá mensajes agregados desde el momento en que se unan.

    La captura de pantalla siguiente muestra la aplicación de chat que se ejecuta en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado **concentradores** que la biblioteca de SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el código de servidor y los scripts de jQuery.

    ![Script de concentrador generado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: creando un centro de que el objeto de coordinación principal en el servidor y utiliza la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de base de datos central y, a continuación, obtener acceso a estos métodos al llamarlas desde scripts de jQuery en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.

El **enviar** método muestra varios conceptos de base de datos central:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectan a este concentrador.
- Llamar a una función de jQuery en el cliente (como el `broadcastMessage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

La página HTML en el ejemplo de código muestra cómo utilizar la biblioteca de jQuery SignalR para comunicarse con un concentrador de SignalR. Las tareas esenciales en el código declara un proxy para hacer referencia al concentrador, declarar una función que el servidor puede llamar para contenido de inserción a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara a un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas camel. El ejemplo de código hace referencia a C# **ChatHub** clase jQuery como **chatHub**.


El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de base de datos central en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. Las dos líneas que HTML codificar el contenido antes de mostrarlo son opcionales y mostrar una manera sencilla de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para generar aplicaciones web en tiempo real. También ha aprendido a varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación de ASP.NET, cómo crear una clase de base de datos central y cómo enviar y recibir mensajes desde el concentrador.

Hacer que la aplicación de ejemplo en este tutorial u otras aplicaciones SignalR disponible a través de Internet mediante su implementación en un proveedor de hospedaje. Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios web en una segunda [cuenta de prueba de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR, consulte [publicar el ejemplo de introducción a SignalR como un sitio Web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [implementar una aplicación de ASP.NET en un sitio Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Nota: el transporte de WebSocket no se admite actualmente para sitios Web de Windows Azure. Transporte de WebSocket cuando no está disponible, SignalR usa los otros transportes disponibles, tal como se describe en la sección de transportes de la [Introducción al tema de SignalR](index.md).)

Para obtener información sobre conceptos de los desarrollos de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
