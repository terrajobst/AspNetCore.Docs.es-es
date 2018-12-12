---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: Interna de SignalR | Microsoft Docs'
author: pfletcher
description: Este tutorial muestra cómo crear un servidor de SignalR 2 alojado en sí mismos y cómo conectarse a ella con un cliente de JavaScript. Versiones de software que se usa en el tutorial V...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 48cb3d4d71c33ac3382b2b35b5a19fa1c4958874
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287408"
---
<a name="tutorial-signalr-self-host"></a>Tutorial: Interna de SignalR
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Descargue el proyecto completado](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Este tutorial muestra cómo crear un servidor de SignalR 2 alojado en sí mismos y cómo conectarse a ella con un cliente de JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
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
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Un servidor de SignalR normalmente se hospeda en una aplicación ASP.NET en IIS, pero también puede ser autohospedado (como en una aplicación de consola o un servicio de Windows) mediante la biblioteca de host propio. Esta biblioteca, al igual que todas SignalR 2, se basa en OWIN ([Open Web Interface para .NET](http://owin.org)). OWIN define una abstracción entre los servidores web de .NET y aplicaciones web. OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS.

No se hospeda en IIS razones:

- Entornos donde IIS no está disponible o deseable, por ejemplo, una granja de servidores existente sin IIS.
- Debe evitarse la sobrecarga de rendimiento de IIS.
- Funcionalidad de SignalR es que se agregarán a una aplicación existente que se ejecuta en un servicio de Windows, el rol de trabajo de Azure u otro proceso.

Si se está desarrollando una solución como autohospedaje por motivos de rendimiento, se recomienda también prueba la aplicación hospedada en IIS para determinar la ventaja de rendimiento.

Este tutorial contiene las siguientes secciones:

- [Crear el servidor](#server)
- [Acceso al servidor con un cliente de JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Crear el servidor

En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor se puede hospedar en cualquier tipo de proceso, como un servicio de Windows o un rol de trabajo de Azure. Para el código de ejemplo para hospedar un servidor de SignalR en un servicio de Windows, consulte [Self-Hosting SignalR en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Abra Visual Studio 2013 con privilegios de administrador. Seleccione **archivo**, **nuevo proyecto**. Seleccione **Windows** bajo el **Visual C#** nodo en el **plantillas** panel y seleccione el **aplicación de consola** plantilla. El nuevo proyecto el nombre "SignalRSelfHost" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Abra la consola del Administrador de paquetes de NuGet seleccionando **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.
3. En la consola de administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Este comando agrega las bibliotecas de SignalR 2 Self al proyecto.
4. En la consola de administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Este comando agrega la biblioteca Microsoft.Owin.Cors al proyecto. Esta biblioteca se usará para la compatibilidad entre dominios, que es necesario para las aplicaciones que hospedan SignalR y un cliente de la página web en dominios diferentes. Puesto que se va a hospedar el servidor de SignalR y el cliente web en puertos diferentes, esto significa que entre dominios deben estar habilitada para la comunicación entre estos componentes.
5. Reemplace el contenido de Program.cs por el código siguiente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    El código anterior incluye tres clases:

    - **Programa**, incluido el **Main** definir la ruta de acceso principal de la ejecución del método. En este método, una aplicación web de tipo **inicio** se inicia en la dirección URL especificada (`http://localhost:8080`). Si se requiere seguridad en el punto de conexión, se puede implementar SSL. Vea [Cómo: Configurar un puerto con un certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obtener más información.
    - **Inicio**, la clase que contiene la configuración para el servidor de SignalR (la única configuración de este tutorial se usa es la llamada a `UseCors`) y la llamada a `MapSignalR`, lo que crea rutas para los objetos del concentrador en el proyecto.
    - **MyHub**, la clase de concentrador de SignalR que la aplicación proporciona a los clientes. Esta clase tiene un único método, **enviar**, que llamará los clientes para difundir un mensaje a todos los demás clientes conectados.
6. Compile y ejecute la aplicación. La dirección que se está ejecutando el servidor debe mostrar en una ventana de consola.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Si se produce un error de ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled`, deberá reiniciar Visual Studio con privilegios de administrador.
8. Detener la aplicación antes de continuar con la siguiente sección.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Acceso al servidor con un cliente de JavaScript

En esta sección, usará el mismo cliente de JavaScript desde el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md). Solo se hará una modificación al cliente, que se usa para definir explícitamente la URL del centro. Con una aplicación autohospedada, el servidor no necesariamente esté en la misma dirección como la dirección URL de conexión (debido a los servidores proxy inversos y equilibradores de carga), por lo que la dirección URL se debe definir explícitamente.

1. En **el Explorador de soluciones**, haga doble clic en la solución y seleccione **agregar**, **nuevo proyecto**. Seleccione el **Web** nodo y seleccione el **aplicación Web ASP.NET** plantilla. Denomine el proyecto "JavascriptClient" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Seleccione el **vacía** plantilla y deje sin selección las opciones restantes. Seleccione **crear proyecto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. En la consola de administrador de paquetes, seleccione el proyecto "JavascriptClient" en el **proyecto predeterminado** lista desplegable y ejecute el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Este comando instala las bibliotecas de SignalR y JQuery que necesitará en el cliente.
4. Haga doble clic en el proyecto y seleccione **agregar**, **nuevo elemento**. Seleccione el **Web** nodo y seleccionar la página HTML. Nombre de la página **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Reemplace el contenido de la nueva página HTML con el código siguiente. Compruebe que las referencias del script coinciden con las secuencias de comandos en la carpeta Scripts del proyecto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    El código siguiente (resaltado en el ejemplo de código anterior) es la suma que ha realizado en el cliente que se usa en el tutorial de introducción al uso (además de actualizar el código a SignalR 2 de la versión beta). Esta línea de código establece explícitamente la dirección URL de conexión de base de SignalR en el servidor.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Haga doble clic en la solución y seleccione **Establecer proyectos de inicio...** . Seleccione el **varios proyectos de inicio** botón de radio y establezca ambos proyectos **acción** a **iniciar**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Haga doble clic en "Default.html" y seleccione **establecer como página principal**.
8. Ejecute la aplicación. Se iniciará el servidor y la página. Es posible que deba volver a cargar la página web (o seleccione **continuar** en el depurador) si la página se carga antes de inicia el servidor.
9. En el explorador, proporcione un nombre de usuario cuando se le solicite. Copie la dirección URL de la página en otra pestaña del explorador o la ventana y proporcione un nombre de usuario diferente. Podrá enviar mensajes desde el panel de un explorador a otro, como se muestra en el tutorial de introducción.
