---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Escalabilidad horizontal de SignalR con Redis (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 3f70536a4bda90a0298f6bb4f7ef47b8a257175b
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287551"
---
<a name="signalr-scaleout-with-redis-signalr-1x"></a>Escalabilidad horizontal de SignalR con Redis (SignalR 1.x)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En este tutorial, usará [Redis](http://redis.io/) para distribuir los mensajes a través de una aplicación de SignalR que se implementa en dos instancias independientes de IIS.

Redis es un almacén de pares clave-valor en memoria. También admite un sistema de mensajería con un modelo de publicación/suscripción. El backplane SignalR Redis usa la característica de pub/sub para reenviar los mensajes a otros servidores.

![](scaleout-with-redis/_static/image1.png)

Para este tutorial, usará los tres servidores:

- Dos servidores que ejecuten Windows, que usará para implementar una aplicación de SignalR.
- Un servidor que ejecuta Linux, que usará para ejecutar Redis. Para las capturas de pantalla en este tutorial, usé Ubuntu 12.04 TLS.

Si no tiene tres servidores físicos a usar, puede crear máquinas virtuales en Hyper-V. Otra opción es crear máquinas virtuales en Azure.

Aunque este tutorial usa la implementación de Redis oficial, hay también un [Windows puerto de Redis](https://github.com/MSOpenTech/redis) desde MSOpenTech. Instalación y configuración son diferentes, pero en caso contrario, los pasos son los mismos.

> [!NOTE] 
> 
> Escalabilidad horizontal de SignalR con Redis no admite clústeres de Redis.


## <a name="overview"></a>Información general

Antes de entrar en el tutorial detallado, le presentamos una introducción rápida de lo que hará.

1. Para instalar Redis e inicie el servidor de Redis.
2. Agregue estos paquetes de NuGet para la aplicación: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.Redis](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. Cree una aplicación de SignalR.
4. Agregue el código siguiente en Global.asax para configurar la placa posterior: 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu en Hyper-V

Con Windows Hyper-V, puede crear fácilmente una VM de Ubuntu en Windows Server.

Descargue el archivo ISO de Ubuntu de [ http://www.ubuntu.com ](http://www.ubuntu.com/).

En Hyper-V, agregue una nueva máquina virtual. En el **conectar disco duro Virtual** paso, seleccione **crear un disco duro virtual**.

![](scaleout-with-redis/_static/image2.png)

En el **opciones de instalación** paso, seleccione **archivo de imagen (.iso)**, haga clic en **examinar**y vaya a la imagen ISO de instalación de Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Para instalar Redis

Siga los pasos de [ http://redis.io/download ](http://redis.io/download) descarguen y compilen Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Esto genera los archivos binarios de Redis el `src` directory.

De forma predeterminada, Redis no requiere una contraseña. Para establecer una contraseña, edite el `redis.conf` archivo, que se encuentra en el directorio raíz del código fuente. (Realizar una copia de seguridad del archivo antes de modificarlo!) Agregue la siguiente directiva a `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Ahora, inicie el servidor de Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Abrir el puerto 6379, que es el puerto predeterminado que Redis escucha en. (Puede cambiar el número de puerto en el archivo de configuración).

## <a name="create-the-signalr-application"></a>Crear la aplicación de SignalR

Cree una aplicación de SignalR con cualquiera de estos tutoriales:

- [Introducción a SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introducción a SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

A continuación, modificaremos la aplicación de chat para admitir la escalabilidad horizontal con Redis. En primer lugar, agregue el paquete de SignalR.Redis NuGet al proyecto. En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

A continuación, abra el archivo Global.asax. Agregue el código siguiente a la **aplicación\_iniciar** método:

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "server" es el nombre del servidor que se está ejecutando Redis.
- *puerto* es el número de puerto
- "password" es la contraseña que ha definido en el archivo redis.conf.
- "AppName" es cualquier cadena. SignalR crea un canal de pub/sub de Redis con este nombre.

Por ejemplo:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Implemente y ejecute la aplicación

Preparar las instancias de Windows Server para implementar la aplicación de SignalR.

Agregue el rol IIS. Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.

![](scaleout-with-redis/_static/image5.png)

También incluyen el servicio de administración (que se muestran en "Herramientas de administración").

![](scaleout-with-redis/_static/image6.png)

**Instalar Web Deploy 3.0.** Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0

![](scaleout-with-redis/_static/image7.png)

Compruebe que se está ejecutando el servicio de administración Web. Si no es así, inicie el servicio. (Si no ve el servicio de administración en la lista de servicios de Windows, asegúrese de que instaló el servicio de administración cuando se agrega el rol de IIS.)

De forma predeterminada, el servicio de administración Web escucha en el puerto TCP 8172. En el Firewall de Windows, cree una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172. Para obtener más información, consulte [configurar reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Si va a hospedar las máquinas virtuales en Azure, puede hacerlo directamente en el portal de Azure. Consulte [cómo configurar extremos en una máquina Virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)

Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo para el servidor. En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.

Para obtener información más detallada sobre la implementación web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra. (Por supuesto, en un entorno de producción, los dos servidores sentaba detrás de un equilibrador de carga.)

![](scaleout-with-redis/_static/image8.png)

Si siente curiosidad por ver los mensajes que se envían a Redis, puede usar el **redis-cli** cliente, que se instala con Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
