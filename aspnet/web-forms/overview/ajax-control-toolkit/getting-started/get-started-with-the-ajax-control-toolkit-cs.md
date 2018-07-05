---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introducción a AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Aprenda todo lo que necesita saber para empezar a usar AJAX Control Toolkit.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 120213ce27858e2f0650b71c82f689ae4493d8a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399602"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Introducción a AJAX Control Toolkit (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Aprenda todo lo que necesita saber para empezar a usar AJAX Control Toolkit.


AJAX Control Toolkit contiene más de 30 controles gratuitos que puede usar en sus aplicaciones ASP.NET. En este tutorial, aprenderá a descargar AJAX Control Toolkit y agregar los controles del Kit de herramientas a su cuadro de herramientas de Visual Studio o Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Descarga de AJAX Control Toolkit

El [AJAX Control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la Comunidad de ASP.NET y el equipo de ASP.NET. 


[![Descarga de AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: descarga de AJAX Control Toolkit ([haga clic aquí para ver imagen en tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Después de descargar el archivo, deberá desbloquear el archivo. Haga clic en el archivo, seleccione Propiedades y haga clic en el **desbloquear** botón (consulte la figura 2).


[![Desbloquear el archivo ZIP de AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: desbloquear el archivo ZIP de AJAX Control Toolkit ([haga clic aquí para ver imagen en tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Después de desbloquear el archivo, puede descomprimir el archivo: haga clic en el archivo y seleccione el **extraer todo** opción de menú. Ahora, estamos listos para agregar el Kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Adición de AJAX Control Toolkit al cuadro de herramientas

La manera más fácil de usar AJAX Control Toolkit consiste en agregar el Kit de herramientas a su cuadro de herramientas de Visual Studio o Visual Web Developer (consulte la figura 3). De este modo, se puede simplemente arrastre un control del Kit de herramientas a una página cuando desee usarlo.


[![AJAX Control Toolkit aparece en el cuadro de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: AJAX Control Toolkit aparece en el cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


En primer lugar, deberá agregar una pestaña de AJAX Control Toolkit al cuadro de herramientas. Siga estos pasos.

1. Crear un nuevo sitio Web de ASP.NET si selecciona la opción de menú archivo, nuevo sitio Web. Haga doble clic en Default.aspx en la ventana Explorador de soluciones para abrir el archivo en el editor.
2. Haga clic en el cuadro de herramientas debajo de la ficha General y seleccione la opción de menú **Agregar pestaña** (consulte la figura 4).
3. Escriba una nueva ficha denominada AJAX Control Toolkit.


[![Agregar una nueva pestaña](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: agregar una ficha nueva ([haga clic aquí para ver imagen en tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


A continuación, deberá agregar los controles de AJAX Control Toolkit para la nueva pestaña. Siga estos pasos:

- Haga clic debajo de la ficha de AJAX Control Toolkit y seleccione la opción de menú **elegir elementos (consulte la figura 5)**.
- Vaya a la ubicación donde descomprimió el AJAX Control Toolkit y seleccione el ensamblado AjaxControlToolkit.dll.


[![Elija los elementos que desea agregar al cuadro de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: elija los elementos que desea agregar al cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Después de completar estos pasos, todos los controles del Kit de herramientas aparecerá en el cuadro de herramientas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Actualizar a una nueva versión del Kit de herramientas

Si usaba una versión anterior del Kit de herramientas y ahora necesita mover a una versión posterior aquí son los pasos recomendados:

- Binarios, elimine la versión anterior del ensamblado AjaxControlToolkit.dll desde la carpeta Bin del sitio Web.
- Elementos de cuadro de herramientas - eliminar la ficha de AJAX Control Toolkit y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit.dll.

> [!div class="step-by-step"]
> [Siguiente](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
