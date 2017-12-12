---
uid: visual-studio/overview/2013/using-browser-link
title: "Mediante el vínculo de explorador en Visual Studio 2013 | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 14f67d81a5b460da591b8fb27fedf53d228e7717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-browser-link-in-visual-studio-2013"></a>Uso de vínculo de explorador en Visual Studio 2013
====================
por [Mike Wasson](https://github.com/MikeWasson)

Vínculo de explorador es una característica nueva en Visual Studio 2013 que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web. Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, lo que resulta útil para las pruebas de varios exploradores.

- [Actualizar del explorador](#browser-refresh)
- [Ver el panel de vínculos de explorador](#dashboard)
- [Habilitar vínculo de explorador para los archivos HTML estáticos](#static-html)
- [Deshabilitar vínculo de explorador](#disabling)
- [¿Cómo funciona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Actualizar del explorador

Con sólo actualizar el explorador, puede actualizar varios exploradores que están conectados a Visual Studio a través del vínculo de explorador.

Para usar actualizar del explorador, primero cree una aplicación de ASP.NET, mediante cualquiera de las plantillas de proyecto. Depurar la aplicación presionando F5 o haciendo clic en el icono de flecha en la barra de herramientas:

![](using-browser-link/_static/image1.png)

También puede utilizar la lista desplegable para seleccionar un explorador específico para la depuración.

![](using-browser-link/_static/image2.png)

Para depurar con varios exploradores, seleccione **explorar con**. En el **explorar con** cuadro de diálogo, mantenga presionada la tecla CTRL para seleccionar más de un explorador. Haga clic en **examinar** para depurar con los exploradores seleccionados. Vínculo de explorador también funciona si iniciar un explorador desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.

![](using-browser-link/_static/image3.png)

Los controles de vínculo de explorador se encuentran en la lista desplegable con el icono de flecha circular. El icono de flecha es el **actualizar** botón.

![](using-browser-link/_static/image4.png)

Para ver qué exploradores están conectados, mantenga el mouse sobre la **actualizar** botón durante la depuración. Los exploradores conectados se muestran en una ventana de información sobre herramientas.

![](using-browser-link/_static/image5.png)

Para actualizar los exploradores conectados, haga clic en el **actualizar** botón o presione CTRL + ALT + ENTRAR. Por ejemplo, la captura de pantalla siguiente muestra un proyecto de ASP.NET, que yo mismo creé con la plantilla de proyecto de MVC 5. Puede ver la aplicación que se ejecuta en dos exploradores en la parte superior. En la parte inferior, el proyecto está abierto en Visual Studio.

![](using-browser-link/_static/image6.png)

En Visual Studio, he cambiado el &lt;h1&gt; encabezado de la página principal:

![](using-browser-link/_static/image7.png)

Cuando hace clic en el **actualizar** botón, el cambio aparece en ambas ventanas del explorador:

![](using-browser-link/_static/image8.png)

**Notas**

- Para habilitar el vínculo de explorador, establezca `debug=true` en el [ &lt;compilación&gt; ](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) elemento en el archivo Web.config del proyecto.
- La aplicación debe ejecutarse en el host local.
- La aplicación debe tener como destino .NET 4.0 o posterior.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Ver el panel de vínculos de explorador

El panel de vínculo de explorador muestra información acerca de las conexiones de vínculo de explorador. Para ver el panel, seleccione el menú desplegable de vínculo de explorador (la flecha pequeña situada junto a la **actualizar** botón). A continuación, haga clic en **panel de vínculos de explorador**.

![](using-browser-link/_static/image9.png)

El panel enumera los exploradores conectados y la dirección URL a la que ha navegado cada explorador.

![](using-browser-link/_static/image10.png)

El **requisitos previos** sección muestra los pasos necesarios para habilitar el vínculo de explorador para ese proyecto. Por ejemplo, la captura de pantalla siguiente muestra un proyecto que "debug" se establece en false en el archivo Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Habilitar vínculo de explorador para los archivos HTML estáticos

Para habilitar el vínculo de explorador para archivos HTML estáticos, agregue lo siguiente al archivo Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Por motivos de rendimiento, quite esta configuración al publicar el proyecto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Deshabilitar vínculo de explorador

Vínculo de explorador está habilitada de forma predeterminada. Hay varias maneras para deshabilitarlo:

- En el menú desplegable de vínculo de explorador, desactive la opción **habilitar vínculo de explorador**. 

    ![](using-browser-link/_static/image12.png)
- En el archivo Web.config, agregue una clave denominada "vs: EnableBrowserLink" con el valor "false" en la sección appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- En el archivo Web.config, ha establecido la depuración en false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>¿Cómo funciona?

Vínculo de explorador utiliza [SignalR](../../../signalr/index.md) para crear un canal de comunicación entre Visual Studio y el explorador. Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a. Vínculo de explorador también registra un módulo HTTP con ASP.NET. Este módulo inserta especial &lt;script&gt; referencias en cada solicitud de página desde el servidor. Puede ver las referencias de script, seleccione "Ver origen" en el explorador.

![](using-browser-link/_static/image13.png)

No se modifican los archivos de origen. El módulo HTTP inserta dinámicamente las referencias de script.

Dado que el código del lado del explorador es JavaScript todos, funciona en todos los exploradores que [SignalR admite](../../../signalr/overview/getting-started/supported-platforms.md), sin necesidad de cualquier complemento de explorador.
