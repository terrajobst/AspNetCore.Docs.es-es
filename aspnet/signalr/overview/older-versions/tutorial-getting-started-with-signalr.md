---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introducción a SignalR 1.x | Microsoft Docs'
author: pfletcher
description: Usar SignalR de ASP.NET para compilar una aplicación de chat en tiempo real en una página HTML.
ms.author: riande
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 720a4879f5fbe3c0c2b4c7809cb94c22547329c3
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287381"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Introducción a SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará SignalR a una aplicación web ASP.NET vacía y cree una página HTML para enviar y mostrar mensajes.


## <a name="overview"></a>Información general

Este tutorial presenta SignalR desarrollo mostrándole cómo compilar una aplicación de chat basada en explorador sencillo. Agregará la biblioteca SignalR para una aplicación web vacía de ASP.NET, cree una clase de hub para enviar mensajes a los clientes y crear una página HTML que permite a los usuarios enviar y recibir mensajes de chat. Para ver un tutorial similar que se muestra cómo crear una aplicación de chat en MVC 4 mediante una vista de MVC, consulte [Introducción a SignalR y MVC 4](index.md).

> [!NOTE]
> Este tutorial usa la versión de lanzamiento (1.x) de SignalR. Para obtener más información sobre los cambios entre SignalR 1.x y 2.0, consulte [SignalR actualizar proyectos de 1.x](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR es una biblioteca de .NET de código abierto para compilar aplicaciones web que requieren la interacción del usuario en directo o actualizaciones de datos en tiempo real. Algunos ejemplos son aplicaciones sociales, juegos multiusuario, meteorológicos de colaboración y de noticias, business o aplicaciones financieras de actualización. A menudo se denominan aplicaciones en tiempo real.

SignalR simplifica el proceso de creación de aplicaciones en tiempo real. Incluye una biblioteca de servidor ASP.NET y una biblioteca de cliente de JavaScript para facilitar la administración de conexiones de cliente / servidor e insertar las actualizaciones de contenido a los clientes. Puede agregar la biblioteca de SignalR a una aplicación ASP.NET existente para obtener la funcionalidad en tiempo real.

El tutorial muestra las siguientes tareas de desarrollo de SignalR:

- Agregar la biblioteca de SignalR a una aplicación web ASP.NET.
- Creación de una clase de concentrador para insertar contenido en los clientes.
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

En esta sección se muestra cómo crear una aplicación web ASP.NET vacía, agregar SignalR y crear la aplicación de chat.

Requisitos previos:

- Visual Studio 2010 SP1 o 2012. Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener el Visual Studio 2012 Express herramienta de desarrollo gratuita.
- [Microsoft ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Para Visual Studio 2012, este instalador agrega nuevas características ASP.NET, incluidas las plantillas de SignalR a Visual Studio. Para Visual Studio 2010 SP1, un instalador no está disponible, pero puede completar el tutorial instalando el paquete SignalR NuGet, como se describe en los pasos de configuración.

Los pasos siguientes usan Visual Studio 2012 para crear una aplicación Web ASP.NET vacía y agregar la biblioteca SignalR:

1. En Visual Studio, cree una aplicación Web vacía de ASP.NET.

    ![Crear sitio web vacío](tutorial-getting-started-with-signalr/_static/image2.png)
2. Abra el **Package Manager Console** seleccionando **herramientas | Administrador de paquetes de NuGet | Consola de administrador de paquetes**. En la ventana de consola, escriba el siguiente comando:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Este comando instala la versión más reciente de SignalR 1.x.
3. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **Add | Clase**. Nombre de la nueva clase **ChatHub**.
4. En **el Explorador de soluciones** expanda el nodo secuencias de comandos. Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    ![Referencias de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. Reemplace el código en el **ChatHub** con el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **clase de aplicación Global** y haga clic en **agregar**.

    ![Agregar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Agregue el siguiente `using` instrucciones después proporcionado `using` las instrucciones de la clase de Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Agregue la siguiente línea de código en el `Application_Start` método de la clase Global para registrar la ruta predeterminada para los concentradores de SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. En **el Explorador de soluciones**, haga clic en el proyecto y luego haga clic en **Add | Nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione página Html y haga clic en **agregar**.
10. En **el Explorador de soluciones**, haga clic en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.
11. Reemplace el código predeterminado en la página HTML con el código siguiente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Guardar todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración. Carga la página HTML en una instancia del explorador y solicitará un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image5.png)
2. Escriba un nombre de usuario.
3. Copie la dirección URL desde la línea de dirección del explorador y usarlo para abrir las instancias del explorador más dos. En cada instancia del explorador, escriba un nombre de usuario único.
4. En cada instancia del explorador, agregue un comentario y haga clic en **enviar**. Los comentarios deben aparecer en todas las instancias del explorador.

    > [!NOTE]
    > Esta aplicación de chat simple no mantiene el contexto de discusión en el servidor. El concentrador difunde comentarios a todos los usuarios actuales. Los usuarios que se unan a la charla más adelante verá mensajes agregados desde el momento en que se unen a.

    Captura de pantalla siguiente muestra la aplicación de chat que se ejecutan en tres instancias del explorador, todos ellos se actualizan cuando una instancia envía un mensaje:

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. En **el Explorador de soluciones**, inspeccionar la **documentos de Script** nodo para la aplicación en ejecución. Hay un archivo de script denominado **hubs** que la biblioteca SignalR genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código de servidor.

    ![Script hub generado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examine el código

La aplicación de chat de SignalR muestra dos tareas básicas de desarrollo de SignalR: crear un centro de que el objeto principal de coordinación en el servidor y mediante la biblioteca de jQuery de SignalR para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código la **ChatHub** clase se deriva de la **Microsoft.AspNet.SignalR.Hub** clase. Derivar de la **concentrador** clase es una forma útil de compilar una aplicación de SignalR. Puede crear métodos públicos en la clase de concentrador y, a continuación, obtener acceso a esos métodos al llamarlas desde scripts de jQuery en una página web.

En el código de chat, llame los clientes la **ChatHub.Send** método para enviar un mensaje nuevo. El concentrador a su vez envía el mensaje a todos los clientes mediante una llamada a **Clients.All.broadcastMessage**.

El **enviar** método muestra varios conceptos de hub:

- Declarar métodos públicos en un centro de modo que los clientes se les pueden llamar.
- Use la **Microsoft.AspNet.SignalR.Hub.Clients** propiedades dinámicas para tener acceso a todos los clientes conectados a este centro.
- Llamar a una función de jQuery en el cliente (como el `broadcastMessage` función) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR y jQuery

La página HTML en el ejemplo de código muestra cómo usar la biblioteca de jQuery SignalR para comunicarse con un concentrador SignalR. Las tareas esenciales en el código declara un proxy para hacer referencia el centro, declarar una función que el servidor puede llamar para insertar contenido a los clientes y, a partir de una conexión para enviar mensajes al concentrador.

El código siguiente declara a un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> En jQuery la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas. El ejemplo de código hace referencia a C# **ChatHub** clase en jQuery como **chatHub**.


El código siguiente es cómo crear una función de devolución de llamada en la secuencia de comandos. La clase de hub en el servidor llama a esta función para insertar las actualizaciones de contenido en cada cliente. Las dos líneas que codifica como HTML el contenido antes de mostrarla son opcionales y muestran una manera sencilla de evitar la inyección de script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

El código siguiente muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, pasa una función para controlar el evento click en la **enviar** botón en la página HTML.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecuta el controlador de eventos.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que SignalR es un marco para crear aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de SignalR: cómo agregar SignalR a una aplicación ASP.NET, cómo crear una clase de hub y cómo enviar y recibir mensajes desde el centro.

Puede realizar la aplicación de ejemplo en este tutorial u otras aplicaciones de SignalR disponibles a través de Internet implementando un proveedor de hospedaje. Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en una segunda oportunidad [cuenta de evaluación de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para ver un tutorial sobre cómo implementar la aplicación de ejemplo SignalR, consulte [publicar el ejemplo de introducción a SignalR como un sitio Web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [implementar una aplicación ASP.NET a un sitio Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Tenga en cuenta: El transporte de WebSocket no se admite actualmente para sitios Web de Windows Azure. Transporte de WebSocket cuando no está disponible, SignalR usa los otros transportes disponibles, como se describe en la sección de transportes de la [Introducción a SignalR tema](index.md).)

Para obtener información sobre los conceptos más avanzados de los desarrollos de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [Proyecto de SignalR](http://signalr.net)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
