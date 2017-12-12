---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: "Ampliación de SignalR con SQL Server (SignalR 1.x) | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>Ampliación de SignalR con SQL Server (SignalR 1.x)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

En este tutorial, utilizará SQL Server para distribuir mensajes en una aplicación de SignalR que se implementa en dos instancias independientes de IIS. También puede ejecutar este tutorial en un equipo de prueba individual, pero para obtener el efecto completo, debe implementar la aplicación de SignalR en dos o más servidores. También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente. Otra opción consiste en ejecutar el tutorial usando máquinas virtuales en Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Requisitos previos

Microsoft SQL Server 2005 o posterior. El backplane es compatible con las ediciones de escritorio y servidores de SQL Server. No es compatible con SQL Server Compact Edition o base de datos de SQL Azure. (Si la aplicación se hospeda en Azure, se recomienda el backplane de Bus de servicio en su lugar.)

## <a name="overview"></a>Información general

Antes de entrar en el tutorial detallado, presentamos una introducción rápida de lo va a hacer.

1. Crear una nueva base de datos vacía. El backplane creará las tablas necesarias en esta base de datos.
2. Agregue estos paquetes de NuGet para la aplicación: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Cree una aplicación de SignalR.
4. Agregue el código siguiente a Global.asax para configurar el plano posterior: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Configurar la base de datos

Decidir si la aplicación utilizará la autenticación de Windows o autenticación de SQL Server para tener acceso a la base de datos. En cualquier caso, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.

Crear una nueva base de datos para el plano posterior a la utilice. Puede utilizar cualquier nombre para la base de datos. No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Habilitar Service Broker

Se recomienda habilitar Service Broker para la base de datos de backplane. Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server, lo que permite el backplane recibir actualizaciones de forma más eficaz. (Sin embargo, el plano posterior también funciona sin el Service Broker).

Para comprobar si está habilitado Service Broker, consultar la **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Para habilitar a Service Broker, use la siguiente consulta SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Si aparece esta consulta generar un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.


Si ha habilitado la traza, los seguimientos también mostrará si Service Broker está habilitado.

## <a name="create-a-signalr-application"></a>Crear una aplicación de SignalR

Crear una aplicación de SignalR, siga cualquiera de estos tutoriales:

- [Introducción a SignalR](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introducción a SignalR y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

A continuación, modificaremos la aplicación de chat para admitir la ampliación con SQL Server. En primer lugar, agregue el paquete de SignalR.SqlServer NuGet al proyecto. En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

A continuación, abra el archivo Global.asax. Agregue el código siguiente a la **aplicación\_iniciar** método:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Implementar y ejecutar la aplicación

Preparar las instancias del servidor de Windows para implementar la aplicación de SignalR.

Agregue el rol IIS. Incluye características de "Desarrollo de aplicaciones", incluido el protocolo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

También incluye el servicio de administración (aparecen en "Herramientas de administración").

![](scaleout-with-sql-server/_static/image5.png)

**Instalar Web Deploy 3.0.** Cuando se ejecuta el Administrador de IIS, se le pedirá que instale la plataforma Web de Microsoft, o bien puede [descargar el intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). En el instalador de plataforma, buscar Web Deploy e instalar Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Compruebe que se está ejecutando el servicio de administración de Web. Si no es así, inicie el servicio. (Si no ve el servicio de administración Web en la lista de servicios de Windows, asegúrese de que instala el servicio de administración cuando se agrega el rol IIS.)

Por último, abrir el puerto 8172 para TCP. Éste es el puerto que utiliza la herramienta de implementación Web.

Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor. En el Explorador de soluciones, haga clic en la solución y haga clic en **publicar**.

Para obtener información más detallada acerca de la implementación de web, consulte [mapa de contenido de implementación Web para Visual Studio y ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si implementa la aplicación en dos servidores, puede abrir cada instancia en otra ventana del explorador y vea que reciban mensajes de SignalR desde la otra. (Por supuesto, en un entorno de producción, los dos servidores se colocan detrás de un equilibrador de carga.)

![](scaleout-with-sql-server/_static/image7.png)

Después de ejecutar la aplicación, puede ver que SignalR crea automáticamente tablas en la base de datos:

![](scaleout-with-sql-server/_static/image8.png)

SignalR administra las tablas. Siempre y cuando se implementa la aplicación, no eliminar filas, modificar la tabla y así sucesivamente.
