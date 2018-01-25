---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: SignalR autohospedaje | Documentos de Microsoft'
author: pfletcher
description: "Este tutorial muestra cómo crear un servidor de SignalR 2 hospedado por sí mismo y cómo conectarse a él con un cliente de JavaScript. Versiones de software que se usa en el tutorial V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-signalr-self-host"></a>Tutorial: SignalR autohospedaje
====================
por [Patrick Fletcher](https://github.com/pfletcher)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> Este tutorial muestra cómo crear un servidor de SignalR 2 hospedado por sí mismo y cómo conectarse a él con un cliente de JavaScript.
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
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

Un servidor de SignalR normalmente se hospeda en una aplicación ASP.NET en IIS, pero también puede ser hospedado por sí mismo (como en una aplicación de consola o el servicio de Windows) mediante la biblioteca de host propio. Esta biblioteca, como ocurre con todas SignalR 2, se basa en OWIN ([interfaz Web abierta para .NET](http://owin.org)). OWIN define una abstracción entre servidores web de .NET y las aplicaciones web. OWIN separa la aplicación web desde el servidor, lo que hace OWIN ideal para el autohospedaje una aplicación web en su propio proceso, fuera de IIS.

Motivos para no se hospeda en IIS:

- Entornos donde IIS no está disponible o deseable, por ejemplo, una granja de servidores existente sin IIS.
- La sobrecarga de rendimiento de IIS debe evitarse.
- Funcionalidad de SignalR consiste en Agregar a una aplicación existente que se ejecuta en un servicio de Windows, el rol de trabajador de Azure u otro proceso.

Si se está desarrollando una solución como autohospedaje por motivos de rendimiento, se recomienda también prueba la aplicación hospedada en IIS para determinar la ventaja de rendimiento.

Este tutorial contiene las siguientes secciones:

- [Crear el servidor](#server)
- [Tener acceso al servidor con un cliente de JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Crear el servidor

En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor puede estar hospedado en algún tipo de proceso, como un servicio de Windows o un rol de trabajador de Azure. Para obtener código de ejemplo para hospedar un servidor de SignalR en un servicio de Windows, vea [Self-Hosting SignalR en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Abra Visual Studio 2013 con privilegios de administrador. Seleccione **archivo**, **nuevo proyecto**. Seleccione **Windows** en el **Visual C#** nodo en el **plantillas** panel y seleccione el **aplicación de consola** plantilla. Asigne al nuevo proyecto "SignalRSelfHost" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Abra la consola del Administrador de paquetes de biblioteca seleccionando **herramientas**, **Administrador de paquetes de biblioteca**, **Package Manager Console**.
3. En la consola de administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Este comando agrega las bibliotecas de SignalR 2 Self-Host al proyecto.
4. En la consola de administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Este comando agrega la biblioteca Microsoft.Owin.Cors al proyecto. Esta biblioteca se usará para la compatibilidad entre dominios, que es necesario para las aplicaciones que hospedan SignalR y un cliente de la página web en dominios diferentes. Puesto que va a hospedar el servidor de SignalR y el cliente web en puertos diferentes, esto significa que entre dominios deben estar habilitada para la comunicación entre estos componentes.
5. Reemplace el contenido de Program.cs por el código siguiente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    El código anterior incluye tres clases:

    - **Programa**, incluido el **Main** definir la ruta de acceso principal de la ejecución del método. En este método, una aplicación web de tipo **inicio** se inicia en la dirección URL especificada (`http://localhost:8080`). Si es necesaria la seguridad en el punto de conexión, se puede implementar SSL. Vea [Cómo: configurar un puerto con un certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obtener más información.
    - **Inicio**, la clase que contiene la configuración para el servidor de SignalR (la única configuración que utiliza este tutorial es la llamada a `UseCors`) y la llamada a `MapSignalR`, que crea rutas para los objetos de base de datos central en el proyecto.
    - **MyHub**, la clase de concentrador de SignalR que la aplicación proporcionará a los clientes. Esta clase tiene un método único, **enviar**, que los clientes llamarán para difundir un mensaje a todos los demás clientes conectados.
6. Compile y ejecute la aplicación. La dirección que se ejecuta el servidor debe mostrar en una ventana de consola.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Si se produce un error en la ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled`, debe reiniciar Visual Studio con privilegios de administrador.
8. Detener la aplicación antes de continuar con la siguiente sección.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Tener acceso al servidor con un cliente de JavaScript

En esta sección, se usará el mismo cliente de JavaScript desde el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md). Sólo se realizará una modificación en el cliente, que se usa para definir explícitamente la dirección URL del concentrador. Con una aplicación hospedada por sí mismo, el servidor no necesariamente esté en la misma dirección que la dirección URL de conexión (debido a los servidores proxy inversos y equilibradores de carga), por lo que las necesidades de la dirección URL que se defina explícitamente.

1. En **el Explorador de soluciones**, haga doble clic en la solución y seleccione **agregar**, **nuevo proyecto**. Seleccione el **Web** y seleccione el **aplicación Web ASP.NET** plantilla. Denomine el proyecto "JavascriptClient" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Seleccione el **vacía** plantilla y deje que se anula la selección de las opciones restantes. Seleccione **crear proyecto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. En la consola de administrador de paquetes, seleccione el proyecto "JavascriptClient" en la **proyecto predeterminado** lista desplegable y ejecute el comando siguiente:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Este comando instala las bibliotecas de SignalR y JQuery que necesitará en el cliente.
4. Haga doble clic en el proyecto y seleccione **agregar**, **nuevo elemento**. Seleccione el **Web** nodo y seleccione página HTML. Nombre de la página **Default.html**.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Reemplace el contenido de la nueva página HTML con el código siguiente. Compruebe que las referencias de script aquí coinciden con las secuencias de comandos en la carpeta Scripts del proyecto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    El código siguiente (resaltado en el ejemplo de código anterior) es la suma que haya realizado al cliente usado en el tutorial de introducción quedé (además de actualizar el código a la versión beta de SignalR versión 2). Esta línea de código establece explícitamente la dirección URL de conexión de la base de SignalR en el servidor.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Haga doble clic en la solución y seleccione **Establecer proyectos de inicio...** . Seleccione el **proyectos de inicio múltiples** botón de radio y establezca ambos proyectos **acción** a **iniciar**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Haga doble clic en "Default.html" y seleccione **establecer como página principal**.
8. Ejecute la aplicación. Se iniciarán la página y el servidor. Puede que necesite volver a cargar la página web (o seleccione **continuar** en el depurador) si la página se cargue antes de que se inicia el servidor.
9. En el explorador, proporcione un nombre de usuario cuando se le solicite. Copiar dirección URL de la página en otra ficha del explorador o ventana y proporcione un nombre de usuario diferente. Podrá enviar mensajes desde el panel del explorador de uno a otro, como se muestra en el tutorial de introducción.
