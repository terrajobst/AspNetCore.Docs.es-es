---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Escalabilidad horizontal de SignalR con Azure Service Bus (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 751de591ec62942c2c9d8b26af40101eb2e922d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820945"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Escalabilidad horizontal de SignalR con Azure Service Bus (SignalR 1.x)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

En este tutorial, implementará una aplicación de SignalR para un rol Web de Windows Azure mediante el backplane de Service Bus para distribuir mensajes a cada instancia de rol.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Requisitos previos:

- Una cuenta de Windows Azure.
- El [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

También es compatible con el backplane de bus de servicio [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1.1. Sin embargo, no es compatible con la versión 1.0 de Service Bus para Windows Server.

## <a name="pricing"></a>Precio

El backplane de Service Bus usa temas para enviar mensajes. Para la información de precio más reciente, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). En el momento de redactar este artículo, puede enviar 1.000.000 mensajes al mes por menos de $1. El backplane envía un mensaje de bus de servicio para cada invocación de un método de concentrador SignalR. También hay algunos mensajes de control para las conexiones, desconexiones, dejando o unirse a grupos y así sucesivamente. En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.

## <a name="overview"></a>Información general

Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.

1. Usar el portal de Windows Azure para crear un nuevo espacio de nombres de Service Bus.
2. Agregue estos paquetes de NuGet para la aplicación: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Cree una aplicación de SignalR.
4. Agregue el código siguiente en Global.asax para configurar la placa posterior: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Para cada aplicación, elija un valor diferente para "YourAppName". No use el mismo valor en varias aplicaciones.

## <a name="create-the-azure-services"></a>Crear los servicios de Azure

Crear un servicio en la nube, como se describe en [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Siga los pasos descritos en la sección "Cómo: crear un servicio de nube mediante Creación rápida". Para este tutorial, no es necesario cargar un certificado.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Crear un nuevo espacio de nombres de Service Bus, como se describe en [cómo a usar temas/suscripciones de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Siga los pasos descritos en la sección "Crear unas Namespace de servicio".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres de Service Bus.


## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Inicie Visual Studio. Desde el **archivo** menú, haga clic en **nuevo proyecto**.

En el **nuevo proyecto** cuadro de diálogo, expanda **Visual C#**. En **plantillas instaladas**, seleccione **en la nube** y, a continuación, seleccione **Windows Azure Cloud Service**. Mantenga el valor predeterminado de .NET Framework 4.5. Nombre de la aplicación ChatService y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, seleccione ASP.NET MVC 4 Web Role. Haga clic en el botón de flecha derecha (**&gt;**) para agregar el rol a la solución.

Mantenga el mouse sobre el nuevo rol, por lo que el icono de lápiz visible. Haga clic en este icono para cambiar el nombre de la función. Nombre de la función "SignalRChat" y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

En el **nuevo proyecto de ASP.NET MVC 4** asistente, seleccione **aplicación de Internet**. Haga clic en **Aceptar**. El Asistente para proyecto crea dos proyectos:

- ChatService: Este proyecto es la aplicación de Windows Azure. Define los roles de Azure y otras opciones de configuración.
- SignalRChat: Este proyecto es el proyecto de ASP.NET MVC 4.

## <a name="create-the-signalr-chat-application"></a>Crear la aplicación de Chat de SignalR

Para crear la aplicación de chat, siga los pasos del tutorial [Introducción a SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Use NuGet para instalar las bibliotecas necesarias. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En el **Package Manager Console** ventana, escriba los siguientes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use el `-ProjectName` opción para instalar los paquetes en el proyecto de ASP.NET MVC, en lugar de en el proyecto de Windows Azure.

## <a name="configure-the-backplane"></a>Configurar la placa posterior

En el archivo Global.asax de la aplicación, agregue el código siguiente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Ahora deberá obtener la cadena de conexión del bus de servicio. En el portal de Azure, seleccione el espacio de nombres del bus de servicio que ha creado y haga clic en el icono de la clave de acceso.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Copie la cadena de conexión en el Portapapeles y, después, péguelo en el *connectionString* variable.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implementar en Azure

En el Explorador de soluciones, expanda el **Roles** carpeta dentro del proyecto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

Haga clic en el rol SignalRChat y seleccione **propiedades**. Seleccione el **configuración** ficha. En **instancias** seleccione 2. También se puede establecer el tamaño de máquina virtual como **Extra pequeño**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Guarde los cambios.

En el Explorador de soluciones, haga clic en el proyecto ChatService. Seleccione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Si se trata de su primera publicación de tiempo en Windows Azure, debe descargar las credenciales. En el **publicar** asistente, haga clic en "Iniciar sesión descargar credenciales". Esto le pedirá que inicie sesión en el portal de Windows Azure y descargar un archivo de configuración de publicación.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Haga clic en **importación** y seleccione el archivo de configuración de publicación que descargó.

Haga clic en **Siguiente**. En el **configuración de publicación** cuadro de diálogo, en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Haga clic en **Publicar**. Puede tardar unos minutos en implementar la aplicación e iniciar las máquinas virtuales.

Ahora al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, con un tema de Service Bus. Un tema es una cola de mensajes que permite que varios suscriptores.

El backplane crea automáticamente el tema y las suscripciones. Para ver las suscripciones y la actividad de mensaje, abra Azure portal, seleccione el espacio de nombres de Service Bus y haga clic en "Temas".

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Pueden pasar unos minutos para que se muestran en el panel de la actividad de mensaje.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR administra la duración de tema. Siempre y cuando se implementa la aplicación, no intente eliminar temas manualmente o cambiar la configuración en el tema.
