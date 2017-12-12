---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Empezar a trabajar con el Kit de herramientas de Control de AJAX (VB) | Documentos de Microsoft
author: microsoft
description: "Obtenga información acerca de todo lo que necesita saber para empezar a usar el Kit de herramientas de Control de AJAX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 0bbf6dc0be8a96ecd47b8620a6ba3220b50f10d4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a>Empezar a trabajar con el Kit de herramientas de Control de AJAX (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de todo lo que necesita saber para empezar a usar el Kit de herramientas de Control de AJAX.


El Kit de herramientas de Control de AJAX contiene más de 30 controles libres que puede usar en las aplicaciones ASP.NET. En este tutorial, aprenderá a descargar el Kit de herramientas de Control de AJAX y agregar los controles del Kit de herramientas a su cuadro de herramientas de Visual Studio o Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Descargar el Kit de herramientas de Control de AJAX

El [AJAX Control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la Comunidad de ASP.NET y el equipo ASP.NET.


[![Descargar el Kit de herramientas de Control de AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)

**Figura 01**: descargar el Kit de herramientas de Control de AJAX ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))


Después de descargar el archivo, debe desbloquear el archivo. Haga clic en el archivo, seleccione Propiedades y haga clic en el **Unblock** botón (consulte la figura 2).


[![Desbloquear el archivo ZIP de Kit de herramientas de Control de AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)

**Figura 02**: desbloquear el archivo ZIP de Kit de herramientas de Control de AJAX ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))


Tras desbloquear el archivo, puede descomprimir el archivo: haga clic en el archivo y seleccione la **extraer todo** opción de menú. Ahora, estamos preparados para agregar el Kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Agregar el Kit de herramientas de Control de AJAX al cuadro de herramientas

La manera más fácil de usar el Kit de herramientas de Control de AJAX consiste en agregar el Kit de herramientas en el cuadro de herramientas de Visual Studio o Visual Web Developer (consulte la figura 3). De este modo, puede simplemente arrastrar un control del Kit de herramientas en una página si desea usarlo.


[![Kit de herramientas de Control de AJAX aparece en el cuadro de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)

**Figura 03**: Kit de herramientas de Control de AJAX aparece en el cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))


En primer lugar, debe agregar una ficha de AJAX Control Toolkit al cuadro de herramientas. Siga estos pasos.

1. Crear un nuevo sitio Web de ASP.NET, seleccione la opción de menú archivo, nuevo sitio Web. Haga doble clic en Default.aspx en la ventana Explorador de soluciones para abrir el archivo en el editor.
2. Haga clic en el cuadro de herramientas debajo de la ficha General y seleccione la opción de menú **Agregar pestaña** (consulte la figura 4).
3. Escriba una nueva ficha denominada Kit de herramientas de Control de AJAX.


[![Agregar una nueva pestaña](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)

**Figura 04**: agregar una nueva pestaña ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))


A continuación, debe agregar los controles de AJAX Control Toolkit para la nueva pestaña. Siga estos pasos:

- Haga clic en debajo de la ficha del Kit de herramientas de Control de AJAX y seleccione la opción de menú **elegir elementos (Véase la figura 5)**.
- Vaya a la ubicación donde descomprimió el Kit de herramientas de Control de AJAX y seleccione el ensamblado AjaxControlToolkit.dll.


[![Elija los elementos que desea agregar al cuadro de herramientas](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)

**Figura 05**: elija los elementos que desea agregar al cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))


Después de completar estos pasos, todos los controles del Kit de herramientas aparecerán en el cuadro de herramientas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Actualizar a una nueva versión del Kit de herramientas

Si usaba una versión anterior del Kit de herramientas y ahora tiene que mover a una versión posterior aquí son los pasos recomendados:

- Los archivos binarios - eliminar la versión anterior del ensamblado AjaxControlToolkit.dll desde la carpeta Bin del sitio Web.
- Elementos de cuadro de herramientas - eliminar la ficha del Kit de herramientas de Control de AJAX y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit.dll.

>[!div class="step-by-step"]
[Anterior](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Siguiente](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
