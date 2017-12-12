---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: "Ampliación de SignalR con el Bus de servicio de Azure | Documentos de Microsoft"
author: MikeWasson
description: "Versiones de software usan en esta versión de Visual Studio 2013 .NET 4.5 SignalR tema 2 versiones anteriores de esta versión de 1.x para SignalR el tema de este tema..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 857fc8baa61549e2fabbb8da012b1fa23950237d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Ampliación de SignalR con el Bus de servicio de Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Bus de servicio para distribuir mensajes a cada instancia de rol. (También puede usar el backplane de Bus de servicio con [aplicaciones de servicio de aplicaciones de Azure web](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Requisitos previos:

- Una cuenta de Windows Azure.
- El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 o 2013.

El backplane de bus de servicio también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/en-us/library/windowsazure/dn282144.aspx), versión 1.1. Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.

## <a name="pricing"></a>Precio

El backplane de Bus de servicio utiliza temas para enviar mensajes. Para la información más reciente sobre los precios, consulte [Bus de servicio](https://azure.microsoft.com/pricing/details/service-bus/). En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes de menos de 1 $. El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador de SignalR. También hay algunos mensajes de control para las conexiones, cuando se producen desconexiones, dejando o unirse a grupos y así sucesivamente. En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.

## <a name="overview"></a>Información general

Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.

1. Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Bus de servicio.
2. Agregue estos paquetes de NuGet para la aplicación: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Cree una aplicación de SignalR.
4. Agregue el código siguiente a Startup.cs para configurar el plano posterior: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Este código configura el plano posterior con los valores predeterminados de [TopicCount](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obtener información acerca de cómo cambiar estos valores, consulte [SignalR rendimiento: ampliación métricas](signalr-performance.md#scaleout_metrics).

Para cada aplicación, elegir un valor diferente para "YourAppName". No utilice el mismo valor en varias aplicaciones.

## <a name="create-the-azure-services"></a>Crear los servicios de Azure

Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida". Para este tutorial, no es necesario cargar un certificado.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Crear un nuevo espacio de nombres de Bus de servicio, como se describe en [cómo para el Bus de servicio de uso temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Siga los pasos descritos en la sección "Crear un Namespace de servicio".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Asegúrese de seleccionar la misma región para el servicio de nube y el espacio de nombres de Bus de servicio.


## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Inicie Visual Studio. Desde el **archivo** menú, haga clic en **nuevo proyecto**.

En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**. En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio de nube de Windows Azure**. Mantenga el valor predeterminado de .NET Framework 4.5. Nombre de la aplicación ChatChannel y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione rol Web de ASP.NET. Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.

Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible. Haga clic en este icono para cambiar el nombre de la función. Nombre de la función "SignalRChat" y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione **MVC**y haga clic en Aceptar.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

El Asistente para proyecto crea dos proyectos:

- ChatChannel: Este proyecto es la aplicación de Windows Azure. Define las funciones de Azure y otras opciones de configuración.
- SignalRChat: Este proyecto es el proyecto de MVC de ASP.NET 5.

## <a name="create-the-signalr-chat-application"></a>Crear la aplicación de Chat de SignalR

Para crear la aplicación de chat, siga los pasos del tutorial [Getting Started with SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Use NuGet para instalar las bibliotecas necesarias. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En el **Package Manager Console** ventana, escriba los siguientes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use la `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.

## <a name="configure-the-backplane"></a>Configurar el Backplane

En el archivo de Startup.cs de la aplicación, agregue el código siguiente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Ahora debe obtener la cadena de conexión de bus de servicio. En el portal de Azure, seleccione el espacio de nombres de bus de servicio que creó y haga clic en el icono de la clave de acceso.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copiar la cadena de conexión en el Portapapeles, a continuación, péguelo en el *connectionString* variable.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implementar en Azure

En el Explorador de soluciones, expanda la **Roles** carpeta dentro del proyecto ChatChannel.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Haga clic en el rol de SignalRChat y seleccione **propiedades**. Seleccione el **configuración** ficha. En **instancias** seleccione 2. También puede establecer el tamaño de VM en **Extra pequeño**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Guarde los cambios.

En el Explorador de soluciones, haga clic en el proyecto ChatChannel. Seleccione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Si se trata de la primera publicación de tiempo en Windows Azure, debe descargar sus credenciales. En el **publicar** asistente, haga clic en "Iniciar sesión para descargar credenciales". Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.

Haga clic en **Siguiente**. En el **configuración de publicación** cuadro de diálogo, en **servicio de nube**, seleccione el servicio de nube que creó anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Haga clic en **Publicar**. Puede tardar unos minutos para implementar la aplicación e iniciar las máquinas virtuales.

Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través del Bus de servicio de Azure, con un tema de Bus de servicio. Un tema es una cola de mensajes que permite a varios suscriptores.

El backplane crea automáticamente el tema y las suscripciones. Para ver las suscripciones y la actividad de un mensaje, abra el portal de Azure, seleccione el espacio de nombres de Bus de servicio y haga clic en "Temas".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Puede durar unos minutos para la actividad de mensaje que aparezcan en el panel.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR administra la duración de tema. Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.

## <a name="troubleshooting"></a>Solución de problemas

**System.InvalidOperationException "El solo IsolationLevel compatible es 'IsolationLevel.Serializable'".**

Este error puede producirse si el nivel de transacción para una operación se establece a algo distinto de `Serializable`. Compruebe que no se se realiza ninguna operación con otros niveles de transacción.
