---
uid: visual-studio/overview/2013/using-browser-link
title: Mediante el vínculo de explorador en Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 96add0de1c1e4366353137898f1ba102aec7f754
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399326"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Mediante el vínculo de explorador en Visual Studio 2013
====================
por [Mike Wasson](https://github.com/MikeWasson)

Vínculo de explorador es una característica nueva en Visual Studio 2013 que crea un canal de comunicación entre el entorno de desarrollo y uno o varios exploradores web. Puede usar el vínculo de explorador para actualizar la aplicación web en varios exploradores a la vez, que es útil para probar varios exploradores.

- [Actualizar del explorador](#browser-refresh)
- [Ver el panel de vínculos de explorador](#dashboard)
- [Habilitar vínculo con exploradores para archivos HTML estáticos](#static-html)
- [Deshabilitar el vínculo de explorador](#disabling)
- [¿Cómo funciona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Actualizar del explorador

Con sólo actualizar del explorador, puede actualizar varios exploradores que están conectados a Visual Studio a través del vínculo de explorador.

Para usar actualizar del explorador, cree primero una aplicación de ASP.NET, mediante cualquiera de las plantillas de proyecto. Depurar la aplicación presionando F5 o haga clic en el icono de flecha en la barra de herramientas:

![](using-browser-link/_static/image1.png)

También puede usar la lista desplegable para seleccionar un explorador específico para la depuración.

![](using-browser-link/_static/image2.png)

Para depurar con varios exploradores, seleccione **explorar con**. En el **explorar con** cuadro de diálogo, mantenga presionada la tecla CTRL para seleccionar más de un explorador. Haga clic en **examinar** para depurar con los exploradores seleccionados. Vínculo de explorador también funciona si se ejecutará un explorador desde fuera de Visual Studio y navegue a la dirección URL de la aplicación.

![](using-browser-link/_static/image3.png)

Los controles de vínculo de explorador se encuentran en la lista desplegable con el icono de flecha circular. El icono de flecha es el **actualizar** botón.

![](using-browser-link/_static/image4.png)

Para ver qué exploradores se conectan, mantenga el mouse sobre el **actualizar** botón durante la depuración. Los exploradores conectados se muestran en una ventana de información sobre herramientas.

![](using-browser-link/_static/image5.png)

Para actualizar los exploradores conectados, haga clic en el **actualizar** botón o presione CTRL + ALT + ENTRAR. Por ejemplo, la captura de pantalla siguiente muestra un proyecto ASP.NET, que he creado mediante la plantilla de proyecto de MVC 5. Puede ver la aplicación en ejecución en los dos exploradores en la parte superior. En la parte inferior, el proyecto está abierto en Visual Studio.

![](using-browser-link/_static/image6.png)

En Visual Studio, he cambiado el &lt;h1&gt; encabezado de la página principal:

![](using-browser-link/_static/image7.png)

Cuando hace clic en el **actualizar** button, el cambio aparece en dos ventanas del explorador:

![](using-browser-link/_static/image8.png)

**Notas**

- Para habilitar el vínculo de explorador, establezca `debug=true` en el [ &lt;compilación&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elemento en el archivo Web.config del proyecto.
- La aplicación debe estar ejecutando en el host local.
- La aplicación debe tener como destino .NET 4.0 o posterior.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Ver el panel de vínculos de explorador

El panel de vínculos de explorador muestra información sobre las conexiones de vínculo de explorador. Para ver el panel, seleccione el menú desplegable de vínculo de explorador (la flecha pequeña situada junto a la **actualizar** botón). A continuación, haga clic en **panel vínculo de explorador**.

![](using-browser-link/_static/image9.png)

El panel enumera los exploradores conectados y la dirección URL a la que ha navegado cada explorador.

![](using-browser-link/_static/image10.png)

El **requisitos previos** sección muestra los pasos necesarios para habilitar el vínculo de explorador para ese proyecto. Por ejemplo, la captura de pantalla siguiente muestra un proyecto donde "debug" se establece en false en el archivo Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Habilitar vínculo con exploradores para archivos HTML estáticos

Para habilitar el vínculo de explorador para archivos HTML estáticos, agregue lo siguiente al archivo Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Por motivos de rendimiento, quite esta configuración al publicar el proyecto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Deshabilitar el vínculo de explorador

Vínculo de explorador está habilitado de forma predeterminada. Hay varias maneras para deshabilitarlo:

- En el menú desplegable de vínculo de explorador, desactive la opción **habilitar Browser Link**. 

    ![](using-browser-link/_static/image12.png)
- En el archivo Web.config, agregue una clave denominada "vs: EnableBrowserLink" con el valor "false" en la sección appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- En el archivo Web.config, establezca la depuración en false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>¿Cómo funciona?

Vínculo de explorador utiliza [SignalR](../../../signalr/index.md) para crear un canal de comunicación entre Visual Studio y el explorador. Cuando se habilita el vínculo de explorador, Visual Studio actúa como un servidor de SignalR que varios clientes (exploradores) pueden conectarse a. Vínculo de explorador también registra un módulo HTTP con ASP.NET. Este módulo inyecta especial &lt;script&gt; referencias en cada solicitud de página desde el servidor. Puede ver las referencias de script, seleccione "Ver origen" en el explorador.

![](using-browser-link/_static/image13.png)

No se modifican los archivos de origen. El módulo HTTP inserta dinámicamente las referencias de script.

Dado que el código del lado del explorador es JavaScript todas, funciona en todos los exploradores que [SignalR admite](../../../signalr/overview/getting-started/supported-platforms.md), sin necesidad de cualquier complemento del explorador.
