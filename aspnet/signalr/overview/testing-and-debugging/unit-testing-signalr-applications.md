---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Pruebas unitarias de aplicaciones SignalR | Documentos de Microsoft
author: pfletcher
description: "En este artículo se describe cómo usar las características de pruebas unitarias de SignalR 2.0."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a>Aplicaciones de SignalR de pruebas unitarias
====================
por [Patrick Fletcher](https://github.com/pfletcher)

> Este artículo describe cómo utilizar las características de pruebas unitarias de SignalR 2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Pruebas unitarias de aplicaciones SignalR

Puede usar las características de pruebas unitarias en SignalR 2 para crear pruebas unitarias para la aplicación de SignalR. SignalR 2 incluye la [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfaz, que se puede usar para crear un objeto ficticio para simular los métodos de concentrador de pruebas.

En esta sección, agregará las pruebas unitarias para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) con [XUnit.net](https://github.com/xunit/xunit) y [Moq](https://github.com/Moq/moq4).

XUnit.net se usará para controlar la prueba; Moq se usará para crear un [simular](http://en.wikipedia.org/wiki/Mock_object) objeto para realizar pruebas. Se pueden usar otros marcos de simulación si lo desea; [NSubstitute](http://nsubstitute.github.io/) también es una buena elección. Este tutorial muestra cómo configurar el objeto ficticio de dos maneras: en primer lugar, utilizando un `dynamic` objeto (introducida en .NET Framework 4) y, después, mediante una interfaz.

### <a name="contents"></a>Contenido

Este tutorial contiene las siguientes secciones.

- [Pruebas unitarias con dinámicos](#dynamic)
- [Pruebas por tipo de unidad](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Pruebas unitarias con dinámicos

En esta sección, agregará una prueba unitaria para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) mediante un objeto dinámico.

1. Instalar el [extensión XUnit ejecutor](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.
2. Completar la [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), o descargar la aplicación completa de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Si está utilizando la versión de descarga de la aplicación de introducción, abra **Package Manager Console** y haga clic en **restaurar** para agregar el paquete de SignalR para el proyecto.

    ![Restaurar los paquetes](unit-testing-signalr-applications/_static/image1.png)
4. Agregar un proyecto a la solución para la prueba unitaria. Haga clic en la solución en **el Explorador de soluciones** y seleccione **agregar**, **nuevo proyecto...** . En el **C#** nodo, seleccione el **Windows** nodo. Seleccione **biblioteca de clases**. Asigne al nuevo proyecto **TestLibrary** y haga clic en **Aceptar**.

    ![Crear biblioteca de prueba](unit-testing-signalr-applications/_static/image2.png)
5. Agregue una referencia en el proyecto de biblioteca de prueba al proyecto SignalRChat. Haga clic en el **TestLibrary** de proyecto y seleccione **agregar**, **referencia...** . Seleccione el **proyectos** nodo bajo el **solución** nodo y verificación **SignalRChat**. Haga clic en **Aceptar**.

    ![Agregar referencia de proyecto](unit-testing-signalr-applications/_static/image3.png)
6. Agregar los paquetes SignalR, Moq y XUnit el **TestLibrary** proyecto. En el **Package Manager Console**, establezca el **proyecto predeterminado** control dropdown **TestLibrary**. Ejecute los siguientes comandos en la ventana de consola:

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Instalar paquetes](unit-testing-signalr-applications/_static/image4.png)
7. Crear el archivo de prueba. Haga clic en el **TestLibrary** del proyecto y haga clic en **agregar...** , **Clase**. La nueva clase el nombre **Tests.cs**.
8. Reemplace el contenido de Tests.cs con el código siguiente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    En el código anterior, se crea un cliente de prueba mediante el `Mock` objeto desde el [Moq](https://github.com/Moq/moq4) biblioteca, de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el tipo parámetro). La `IHubCallerConnectionContext` interfaz es el objeto de proxy con el que invocar métodos en el cliente. El `broadcastMessage` función, a continuación, se define para el cliente ficticio para que se puede llamar a través del `ChatHub` clase. El motor de pruebas, a continuación, llama a la `Send` método de la `ChatHub` (clase), que a su vez llama a la simuladas `broadcastMessage` función.
9. Compile la solución presionando **F6**.
10. Ejecute la prueba unitaria. En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**. En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image5.png)
11. Compruebe que se superó la prueba activando el panel inferior de la ventana del explorador de pruebas. La ventana mostrará que se superó la prueba.

    ![Prueba superada](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Pruebas por tipo de unidad

En esta sección, agregará una prueba para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) usando una interfaz que contiene el método que se va a probar.

1. Complete los pasos del 1 al 7 en el [pruebas unitarias con dinámicos](#dynamic) tutorial anterior.
2. Reemplace el contenido de Tests.cs con el código siguiente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    En el código anterior, se crea una interfaz definiendo la firma de la `broadcastMessage` método para el que el motor de pruebas creará un cliente ficticio. A continuación, se crea un cliente ficticio mediante la `Mock` objeto del tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en SignalR 2.1, asigne `dynamic` para el parámetro de tipo.) La `IHubCallerConnectionContext` interfaz es el objeto de proxy con el que invocar métodos en el cliente.

    La prueba, a continuación, crea una instancia de `ChatHub`y, a continuación, crea una versión ficticia de la `broadcastMessage` método, que a su vez, se invoca mediante una llamada a la `Send` método en el concentrador.
3. Compile la solución presionando **F6**.
4. Ejecute la prueba unitaria. En Visual Studio, seleccione **prueba**, **Windows**, **Explorador de pruebas**. En la ventana Explorador de pruebas, haga clic en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image7.png)
5. Compruebe que se superó la prueba activando el panel inferior de la ventana del explorador de pruebas. La ventana mostrará que se superó la prueba.

    ![Prueba superada](unit-testing-signalr-applications/_static/image8.png)
