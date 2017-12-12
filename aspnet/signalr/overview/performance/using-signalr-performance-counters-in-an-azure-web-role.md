---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Mediante los contadores de rendimiento de SignalR en un rol Web de Azure | Documentos de Microsoft
author: guardrex
description: "Cómo instalar y utilizar contadores de rendimiento de SignalR en un rol Web de Azure."
keywords: Contador de ASP.NET,signalr,performance, rol web de azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Uso de contadores de rendimiento de SignalR en un rol Web de Azure

Por [Luke Latham](https://github.com/guardrex)

Contadores de rendimiento de SignalR se utilizan para supervisar el rendimiento de la aplicación en un rol Web de Azure. Los contadores se capturan por diagnósticos de Microsoft Azure. Instalar los contadores de rendimiento de SignalR en Azure con *signalr.exe*, la misma herramienta que se usa para aplicaciones independientes o de forma local. Puesto que las funciones de Azure son transitorias, configurar una aplicación para instalar y registrar los contadores de rendimiento de SignalR durante el inicio.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [SDK de Microsoft Azure para Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Nota: reiniciar el equipo después de instalar el SDK.**
* Suscripción de Microsoft Azure: para suscribirse a una cuenta de evaluación de Azure gratuita, consulte [prueba gratuita de Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Crear una aplicación de rol Web de Azure que expone los contadores de rendimiento de SignalR

1. Abra Visual Studio 2015.

2. En Visual Studio 2015, seleccione **archivo &gt; New &gt; proyecto**.

3. En el **plantillas** panel de la **nuevo proyecto** ventana bajo la **Visual C#** nodo, seleccione el **nube** nodo y seleccione el **Servicio de nube de azure** plantilla. Nombre de la aplicación **SignalRPerfCounters** y seleccione **Aceptar**.

   ![Nueva aplicación de nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. En el **nuevo servicio de nube de Microsoft Azure** cuadro de diálogo, seleccione **rol Web de ASP.NET** y seleccione la  **&gt;**  botón para agregar el rol para el proyecto. Seleccione **Aceptar**.

   ![Agregar rol Web de ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. En el **nueva aplicación Web de ASP.NET: WebRole1** cuadro de diálogo, seleccione la **MVC** plantilla y, a continuación, seleccione **Aceptar**.

   ![Agregar API Web y MVC](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. En **el Explorador de soluciones**, abra el *diagnostics.wadcfgx* de archivos en **WebRole1**.

   ![Diagnostics.wadcfgx del explorador de soluciones](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Reemplace el contenido del archivo con la siguiente configuración y guarde el archivo:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Abra la **consola de administrador de paquetes** de **herramientas &gt; Administrador de paquetes de NuGet**. Escriba los comandos siguientes para instalar la versión más reciente de SignalR y el paquete de utilidades de SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Configurar la aplicación para instalar los contadores de rendimiento de SignalR en la instancia de rol cuando inicia o se recicla. En **el Explorador de soluciones**, haga doble clic en el **WebRole1** de proyecto y seleccione **agregar &gt; nueva carpeta**. Nombre de la nueva carpeta *inicio*.

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Copia la *signalr.exe* archivo (agregado con la **Microsoft.AspNet.SignalR.Utils** paquete) de  **&lt;carpeta de proyecto&gt;\SignalRPerfCounters\packages\ Microsoft.AspNet.SignalR.Utils. &lt;versión&gt;\tools** a la *inicio* carpeta que creó en el paso anterior.

11. En **el Explorador de soluciones**, haga clic en el *inicio* carpeta y seleccione **agregar &gt; elemento existente**. En el cuadro de diálogo que aparece, seleccione *signalr.exe* y seleccione **agregar**.

    ![Agregue signalr.exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Haga doble clic en el *inicio* carpeta que ha creado. Seleccione **agregar &gt; nuevo elemento**. Seleccione el **General** nodo, seleccione **archivo de texto**y el nombre del nuevo elemento *SignalRPerfCounterInstall.cmd*. Este archivo de comandos va a instalar los contadores de rendimiento de SignalR en el rol web.

    ![Crear archivo de proceso por lotes de instalación de contadores de rendimiento de SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Cuando Visual Studio crea el *SignalRPerfCounterInstall.cmd* archivo, abrirá automáticamente en la ventana principal. Reemplace el contenido del archivo con el siguiente script, a continuación, guarde y cierre el archivo. Este script se ejecuta *signalr.exe*, que agrega los contadores de rendimiento de SignalR a la instancia de rol.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Seleccione el *signalr.exe* en el archivo **el Explorador de soluciones**. En el archivo **propiedades**, establezca **copiar en el directorio de salida** a **copiar siempre**.

    ![Establezca copiar en directorio de salida para copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Repita el paso anterior para el *SignalRPerfCounterInstall.cmd* archivo.

    
16. Haga doble clic en el *SignalRPerfCounterInstall.cmd* de archivos y seleccione **abrir con**. En el cuadro de diálogo que aparece, seleccione **Editor binario** y seleccione **Aceptar**.

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. En el editor binario, seleccione los bytes iniciales en el archivo y eliminarlos. Guarde y cierre el archivo.

    ![Eliminar bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Abra *ServiceDefinition.csdef* y agregue una tarea de inicio que se ejecuta el *SignalrPerfCounterInstall.cmd* cuando se inicia el servicio de archivos:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Abra `Views/Shared/_Layout.cshtml` y quitar la secuencia de comandos de agrupación de jQuery desde el final del archivo.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Agregar un cliente de JavaScript que continuamente llama el `increment` método en el servidor. Abra `Views/Home/Index.cshtml` y reemplace el contenido con el código siguiente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Crear una nueva carpeta en el **WebRole1** proyecto denominado *concentradores*. Haga clic en el *concentradores* carpeta **el Explorador de soluciones**, seleccione **Web &gt; SignalR**y seleccione **clase de concentrador de SignalR (v2)**. Nombre del nuevo centro de *MyHub.cs* y seleccione **agregar**.

    ![Agregar clase de concentrador de SignalR a la carpeta de bases de datos centrales en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* se abrirá automáticamente en la ventana principal. Reemplace el contenido con el código siguiente, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  es una densidad de conexión proporcionada con el código de base de SignalR de herramienta de prueba. Puesto que cigüeñal requiere una conexión persistente, agrega uno a su sitio para su uso cuando se prueba. Agregar una nueva carpeta para el **WebRole1** proyecto denominado *PersistentConnections*. Haga clic en esta carpeta y seleccione **agregar &gt; clase**. Asigne al nuevo archivo de clase *MyPersistentConnections.cs* y seleccione **agregar**.

24. Visual Studio abrirá el *MyPersistentConnections.cs* archivo en la ventana principal. Reemplace el contenido con el código siguiente, a continuación, guarde y cierre el archivo:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Mediante el `Startup` (clase), los objetos de SignalR se iniciará cuando se inicie OWIN. Abra o cree *Startup.cs* y reemplace el contenido con el código siguiente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    En el código anterior, el `OwinStartup` atributo marca esta clase para iniciar OWIN. El `Configuration` método comienza a SignalR.
    
26. Probar la aplicación en el emulador de Microsoft Azure presionando **F5**.

    > [!NOTE]
    > Si se produce un **FileLoadException** en **MapSignalR**, cambiar las redirecciones de enlace en *web.config* al siguiente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Espere aproximadamente un minuto. Abra la ventana de herramientas del explorador de la nube en Visual Studio (**vista &gt; Explorer nube**) y expanda la ruta de acceso `(Local)\Storage Accounts\(Development)\Tables`. Haga doble clic en **WADPerformanceCountersTable**. Debería ver los contadores de SignalR en la tabla de datos. Si no ve la tabla, debe volver a escribir las credenciales de almacenamiento de Azure. Debe seleccionar la **actualizar** botón para ver la tabla de **Explorer nube** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos en la tabla.

    ![Al seleccionar la tabla de contadores de rendimiento de WAD en el Explorador de Visual Studio en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Que muestra los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Para probar la aplicación en la nube, actualice el **ServiceConfiguration.Cloud.cscfg** de archivos y establezca la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` en una cadena de conexión de cuenta de almacenamiento de Azure válida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Implementar la aplicación en su suscripción de Azure. Para obtener más información sobre cómo implementar una aplicación en Azure, consulte [cómo crear e implementar un servicio de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Espere unos minutos. En **Explorer nube**, busque la cuenta de almacenamiento que configuró anteriormente y busque el `WADPerformanceCountersTable` tabla en ella. Debería ver los contadores de SignalR en la tabla de datos. Si no ve la tabla, debe volver a escribir las credenciales de almacenamiento de Azure. Debe seleccionar la **actualizar** botón para ver la tabla de **Explorer nube** o seleccione el **actualizar** botón en la ventana Abrir tabla para ver los datos en la tabla.

Agradecimientos especiales a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original utilizado en este tutorial.
