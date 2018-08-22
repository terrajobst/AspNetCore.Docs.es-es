---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Uso de contadores de rendimiento de SignalR en un rol Web de Azure | Microsoft Docs
author: guardrex
description: Cómo instalar y usar contadores de rendimiento de SignalR en un rol Web de Azure.
keywords: Contador ASP.NET,signalr,performance, rol web de azure
ms.author: riande
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: acc9836535466a801f1f46ec18d05937e2e42af2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838284"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Uso de contadores de rendimiento de SignalR en un rol Web de Azure

Por [Luke Latham](https://github.com/guardrex)

Contadores de rendimiento de SignalR se usan para supervisar el rendimiento de la aplicación en un rol Web de Azure. Los contadores se capturan por diagnósticos de Microsoft Azure. Instalar los contadores de rendimiento de SignalR en Azure con *signalr.exe*, la misma herramienta que se usa para las aplicaciones independientes o en el entorno local. Puesto que las funciones de Azure son transitorias, configurar una aplicación para instalar y registrar los contadores de rendimiento de SignalR tras el inicio.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK para Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Nota: reinicie el equipo después de instalar el SDK.**
* Suscripción de Microsoft Azure: para suscribirse a una cuenta de prueba gratuita de Azure, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Crear una aplicación de rol Web de Azure que expone los contadores de rendimiento de SignalR

1. Abra Visual Studio 2015.

2. En Visual Studio 2015, seleccione **archivo** > **New** > **proyecto**.

3. En el **plantillas** panel de la **nuevo proyecto** ventana bajo el **Visual C#** nodo, seleccione el **en la nube** nodo y seleccione el **Azure Cloud Service** plantilla. Nombre de la aplicación **SignalRPerfCounters** y seleccione **Aceptar**.

   ![Nueva aplicación en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. En el **nuevo servicio en la nube de Microsoft Azure** cuadro de diálogo, seleccione **rol Web de ASP.NET** y seleccione el > botón para agregar el rol al proyecto. Seleccione **Aceptar**.

   ![Agregar el rol Web de ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. En el **nueva aplicación Web de ASP.NET: WebRole1** cuadro de diálogo, seleccione el **MVC** plantilla y, a continuación, seleccione **Aceptar**.

   ![Incorporación de MVC y Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. En **el Explorador de soluciones**, abra el *diagnostics.wadcfgx* archivo **WebRole1**.

   ![Diagnostics.wadcfgx del explorador de soluciones](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Reemplace el contenido del archivo con la siguiente configuración y guarde el archivo:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Abra el **Package Manager Console** desde **herramientas** > **Administrador de paquetes de NuGet**. Escriba los comandos siguientes para instalar la versión más reciente de SignalR y el paquete de utilidades de SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Configure la aplicación para instalar los contadores de rendimiento de SignalR en la instancia de rol cuando se inicia o se recicla. En **el Explorador de soluciones**, haga doble clic en el **WebRole1** del proyecto y seleccione **agregar** > **nueva carpeta**. Nombre de la nueva carpeta *inicio*.

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Copia el *signalr.exe* archivo (agregado con el **Microsoft.AspNet.SignalR.Utils** paquete) de \<carpeta del proyecto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versión > / herramientas para el *inicio* carpeta que creó en el paso anterior.

11. En **el Explorador de soluciones**, haga clic en el *inicio* carpeta y seleccione **agregar** > **elemento existente**. En el cuadro de diálogo que aparece, seleccione *signalr.exe* y seleccione **agregar**.

    ![Agregar signalr.exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Haga doble clic en el *inicio* carpeta que ha creado. Seleccione **Agregar** > **Nuevo elemento**. Seleccione el **General** nodo, seleccione **archivo de texto**y el nombre del nuevo elemento *SignalRPerfCounterInstall.cmd*. Este archivo de comandos instala los contadores de rendimiento de SignalR en el rol web.

    ![Crear archivo de proceso por lotes de instalación del contador de rendimiento de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Cuando Visual Studio crea el *SignalRPerfCounterInstall.cmd* archivo, abrirá automáticamente en la ventana principal. Reemplace el contenido del archivo con el siguiente script, a continuación, guarde y cierre el archivo. Este script se ejecuta *signalr.exe*, que agrega los contadores de rendimiento de SignalR a la instancia de rol.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Seleccione el *signalr.exe* archivo **el Explorador de soluciones**. En el archivo **propiedades**, establezca **Copy to Output Directory** a **copiar siempre**.

    ![Conjunto de copia al directorio de salida para copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Repita el paso anterior para el *SignalRPerfCounterInstall.cmd* archivo.

    
16. Haga doble clic en el *SignalRPerfCounterInstall.cmd* de archivo y seleccione **abrir con**. En el cuadro de diálogo que aparece, seleccione **Editor binario** y seleccione **Aceptar**.

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. En el editor binario, seleccione todos los bytes iniciales en el archivo y eliminarlos. Guarde y cierre el archivo.

    ![Eliminar los bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Abra *ServiceDefinition.csdef* y agregue una tarea de inicio que se ejecuta el *SignalrPerfCounterInstall.cmd* cuando se inicia el servicio de archivos:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Abra `Views/Shared/_Layout.cshtml` y quitar la secuencia de comandos del paquete jQuery desde el final del archivo.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Agregar un cliente de JavaScript que continuamente llama el `increment` método en el servidor. Abra `Views/Home/Index.cshtml` y reemplace el contenido con el código siguiente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Cree una nueva carpeta en el **WebRole1** proyecto denominado *Hubs*. Haga clic en el *Hubs* carpeta **el Explorador de soluciones**, seleccione **Web** > **SignalR**y seleccione  **Clase de concentrador SignalR (v2)**. Nombre del nuevo centro *MyHub.cs* y seleccione **agregar**.

    ![Agregar clase de concentrador de SignalR a la carpeta de concentradores en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* se abrirá automáticamente en la ventana principal. Reemplace el contenido por el código siguiente, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  es probar herramienta proporcionada con la base de código de SignalR una densidad de la conexión. Como manivela requiere una conexión persistente, agrega uno a su sitio para su uso al probar. Agregar una nueva carpeta para el **WebRole1** proyecto denominado *PersistentConnections*. Haga clic en esta carpeta y seleccione **agregar** > **clase**. Asigne al nuevo archivo de clase *MyPersistentConnections.cs* y seleccione **agregar**.

24. Visual Studio abrirá el *MyPersistentConnections.cs* archivo en la ventana principal. Reemplace el contenido por el código siguiente, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Mediante el `Startup` (clase), los objetos de SignalR se iniciará cuando se inicie OWIN. Abra o cree *Startup.cs* y reemplace el contenido con el código siguiente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    En el código anterior, el `OwinStartup` atributo marca esta clase para el inicio de OWIN. El `Configuration` método comienza a SignalR.
    
26. Probar la aplicación en el emulador de Microsoft Azure presionando **F5**.

    > [!NOTE]
    > Si se produce un **FileLoadException** en **MapSignalR**, cambie las redirecciones de enlace en *web.config* al siguiente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Espere aproximadamente un minuto. Abra la ventana de herramientas de Cloud Explorer en Visual Studio (**vista** > **Cloud Explorer**) y expanda la ruta de acceso `(Local)/Storage Accounts/(Development)/Tables`. Haga doble clic en **WADPerformanceCountersTable**. Debería ver los contadores de SignalR en la tabla de datos. Si no ve la tabla, deberá volver a escribir sus credenciales de Azure Storage. Es posible que deba seleccionar el **actualizar** botón para ver la tabla de **Cloud Explorer** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos de la tabla.

    ![Seleccione la tabla de contadores de rendimiento de WAD en Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Que muestra los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Para probar la aplicación en la nube, actualice el **ServiceConfiguration.Cloud.cscfg** de archivos y establecer el `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` a una cadena de conexión de cuenta de almacenamiento de Azure válida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Implementar la aplicación en su suscripción de Azure. Para obtener más información sobre cómo implementar una aplicación en Azure, consulte [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Espere unos minutos. En **Cloud Explorer**, busque la cuenta de almacenamiento que configuró anteriormente y busque el `WADPerformanceCountersTable` tabla en ella. Debería ver los contadores de SignalR en la tabla de datos. Si no ve la tabla, deberá volver a escribir sus credenciales de Azure Storage. Es posible que deba seleccionar el **actualizar** botón para ver la tabla de **Cloud Explorer** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos de la tabla.

Agradecimientos especiales a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original utilizado en este tutorial.
