---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hospedar OWIN en un rol de trabajo de Azure | Microsoft Docs
author: MikeWasson
description: Este tutorial muestra cómo autohospedaje OWIN en un rol de trabajo de Microsoft Azure. Interfaz Web abierta para .NET (OWIN) define una abstracción entre el servidor web. NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 56019ef7bdf1c3e9a769ba43f624ef3c6a5e6d4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366710"
---
<a name="host-owin-in-an-azure-worker-role"></a>Hospedar OWIN en un rol de trabajo de Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Este tutorial muestra cómo autohospedaje OWIN en un rol de trabajo de Microsoft Azure.
> 
> [Interfaz Web abierta para .NET](http://owin.org/) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web. OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS: por ejemplo, dentro de un rol de trabajo de Azure.
> 
> En este tutorial, obtendrá información sobre cómo autohospedar un aplicaciones OWIN dentro de un rol de trabajo de Microsoft Azure. Para obtener más información acerca de los roles de trabajo, vea [modelos de ejecución de Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Azure SDK para .NET 2.3](https://azure.microsoft.com/downloads/)
> - [Microsoft.Owin.Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a>Crear un proyecto de Microsoft Azure

Inicie Visual Studio con privilegios de administrador. Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.

En el **archivo** menú, haga clic en **New**, a continuación, haga clic en **proyecto**. Desde **plantillas instaladas**, bajo Visual C#, haga clic en **en la nube** y, a continuación, haga clic en **Windows Azure Cloud Service**. Denomine el proyecto "AzureApp" y haga clic en **Aceptar**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, haga doble clic en **rol de trabajo**. Deje el nombre predeterminado ("WorkerRole1"). Este paso agrega un rol de trabajo a la solución. Haga clic en **Aceptar**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La solución de Visual Studio que se crea contiene dos proyectos:

- &quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.
- &quot;WorkerRole1&quot; contiene el código para el rol de trabajo.

En general, una aplicación de Azure puede contener varios roles, aunque este tutorial utiliza una única función.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Agregue los paquetes de autohospedaje de OWIN

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca**, a continuación, haga clic en **Package Manager Console**.

En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Agregar un extremo HTTP

En el Explorador de soluciones, expanda el proyecto AzureApp. Expanda el nodo Roles, haga clic en WorkerRole1 y seleccione **propiedades**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Haga clic en **extremos**y, a continuación, haga clic en **agregar punto de conexión**.

En el **protocolo** lista desplegable, seleccione "http". En **puerto público** y **puerto privado**, escriba 80. Estos números de puerto pueden ser diferentes. El puerto público es lo que los clientes usan cuando envía una solicitud a la función.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Crear la clase de inicio OWIN

En el Explorador de soluciones, haga clic en el proyecto WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `Startup`.

Reemplace todo el código reutilizable con lo siguiente:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

El `UseWelcomePage` método de extensión agrega una página HTML sencilla a la aplicación, para comprobar que funciona el sitio.

## <a name="start-the-owin-host"></a>Iniciar el Host OWIN

Abra el archivo WorkerRole.cs. Esta clase define el código que se ejecuta cuando se inicia y detiene el rol de trabajo.

Agregue la siguiente instrucción using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Agregar un **IDisposable** miembro a la `WorkerRole` clase:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

En el `OnStart` método, agregue el código siguiente para iniciar el host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

El **WebApp.Start** método inicia el host OWIN. El nombre de la `Startup` clase es un parámetro de tipo para el método. Por convención, el host llamará el `Configure` método de esta clase.

Invalidar el `OnStop` para desechar el  *\_aplicación* instancia:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Este es el código completo de WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure. Dependiendo de la configuración del firewall debe permitir que el emulador a través del firewall.

El emulador de proceso asigna una dirección IP local para el punto de conexión. Puede encontrar la dirección IP mediante la visualización de la interfaz de usuario del emulador de proceso. Haga clic en el icono del emulador en la barra del área de notificación de tareas y seleccione **Mostrar IU del emulador de proceso**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Buscar la dirección IP en implementaciones de servicios de implementación [id], detalles del servicio. Abra un explorador web y vaya a http://<em>dirección</em>, donde <em>dirección</em> es la dirección IP asignada por el emulador de proceso; por ejemplo, `http://127.0.0.1:80`. Debería ver la página de bienvenida de OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

Este paso, debe tener una cuenta de Azure. Si aún no tiene uno, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

En el Explorador de soluciones, haga clic en el proyecto AzureApp. Seleccione **Publicar**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Si no inició sesión en su cuenta de Azure, haga clic en **sesión**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Una vez que haya iniciado sesión, elija una suscripción y haga clic en **siguiente**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Escriba un nombre para el servicio en la nube y elija una región. Haga clic en **Crear**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Haga clic en **Publicar**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La ventana de registro de actividad de Azure muestra el progreso de la implementación. Cuando se implementa la aplicación, vaya a `http://appname.cloudapp.net/`, donde *appname* es el nombre del servicio en la nube.

## <a name="additional-resources"></a>Recursos adicionales

- [Información general del proyecto Katana](an-overview-of-project-katana.md)
- [Proyecto Katana en GitHub](https://github.com/aspnet/AspNetKatana/)
