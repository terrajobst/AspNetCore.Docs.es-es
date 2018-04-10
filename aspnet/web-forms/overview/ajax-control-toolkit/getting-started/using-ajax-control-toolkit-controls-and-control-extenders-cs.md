---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Usar controles de Kit de herramientas de Control de AJAX y los extensores de Control (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar controles de AJAX Control Toolkit y los extensores a las páginas ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Usar controles de Kit de herramientas de Control de AJAX y los extensores de Control (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo agregar controles de AJAX Control Toolkit y los extensores a las páginas ASP.NET.


El Kit de herramientas de Control de AJAX contiene un conjunto de controles y los extensores de control. En este breve tutorial, aprenderá a agregar controles y extensores de control a una página ASP.NET.

> [!NOTE] 
> 
> Para obtener instrucciones sobre cómo instalar el Kit de herramientas de Control de AJAX y agregar el Kit de herramientas de Control de AJAX en el cuadro de herramientas de Visual Studio o Visual Web Developer, vea el tutorial [empezar a trabajar con el Kit de herramientas de Control de AJAX](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>Usar controles de Kit de herramientas de Control de AJAX

Un control AJAX Control Toolkit funciona igual que un control ASP.NET normal. Puede arrastrar el control desde el cuadro de herramientas en una página ASP.NET. Puede agregar el control a la página en la vista de diseño o vista del origen.

Hay un requisito especial cuando usa los controles desde el Kit de herramientas de Control de AJAX. La página debe contener un control ScriptManager. El control ScriptManager es responsable para incluir todo el código de JavaScript necesario requerido por los controles del Kit de herramientas de Control de AJAX.

Por ejemplo, la pestaña del Kit de herramientas de Control de AJAX incluye un control denominado el control del Editor. Este control muestra un editor de HTML enriquecido. Siga estos pasos para agregar el control de Editor a una página:

1. Crear una nueva página ASP.NET denominada ShowEditor.aspx
2. Seleccione el control ScriptManager de la pestaña Extensiones de AJAX en el cuadro de herramientas y arrastre el control a la página.
3. Seleccione el control del Editor de la pestaña del Kit de herramientas de Control de AJAX en el cuadro de herramientas y arrastre el control a la página (consulte la figura 1). El diseñador debería parecerse a la figura 2.
4. Ejecutar el sitio web seleccionando la opción de menú **depurar, Iniciar depuración** o presionar la tecla F5.
5. Debería ver la página en la figura 3.


[![Selecciona el control de Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: selecciona el control de Editor HTML ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Diseñador de Visual Studio con el control ScriptManager y editar](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: Diseñador de Visual Studio con el control ScriptManager y edición ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![La página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: DisplayEditor.aspx la página ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Usar extensores de Control del Kit de herramientas de Control de AJAX

El Kit de herramientas de Control de AJAX también contiene los extensores de control. Como sugiere su nombre, un extensor de control extiende la funcionalidad de un control existente. Por ejemplo, el extensor de control ConfirmButton amplía el control de botón de ASP.NET estándar. El dispositivo extender cambia el comportamiento de s de control de botón para que el botón muestra un cuadro de diálogo de confirmación cuando haga clic en él.

Un extensor de control, al igual que un control AJAX Control Toolkit, requiere un control ScriptManager. Debe agregar un control ScriptManager a una página para empezar a usar extensores de control en la página.

Siga estos pasos para usar el extensor de control ConfirmButton:

1. Crear una nueva página ASP.NET denominada ShowConfirmButton.aspx
2. Agregar un control ScriptManager a la página arrastrando el control en la página de la pestaña Extensiones de AJAX.
3. Agregar un control de botón estándar a la página arrastrando el botón de la ficha estándar en el cuadro de herramientas hasta la superficie del diseñador.
4. Haga clic en el **Agregar extensor** opción de tareas (consulte la figura 4).
5. En el cuadro de diálogo Elegir extensor, seleccione ConfirmButtonExtender (Véase la figura 5) y haga clic en el botón Aceptar.
6. Seleccione el control de botón en el diseñador y expanda el dispositivo Extender, Button1\_ConfirmButtonExtender nodo en la ventana Propiedades (consulte la figura 6). Asigne el valor *realmente?* a la propiedad ConfirmText.
7. Ejecute la página seleccionando la opción de menú **depurar, Iniciar depuración** o presionar la tecla F5.


[![La opción de la tarea de agregar Extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: opción de agregar el extensor de tarea ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![Al seleccionar el extensor de control ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: seleccionar el extensor de control ConfirmButton ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: establecer una propiedad de ConfirmButton ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Cuando se abre la página, verá un botón. Al hacer clic en el botón, obtendrá el cuadro de diálogo de confirmación en la figura 7.


[![Mostrar el cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: mostrar el cuadro de diálogo de confirmación ([haga clic aquí para ver la imagen a tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Tenga en cuenta que normalmente no arrastra un extensor de control a una página. En su lugar, use la **Agregar extensor** opción para agregar un dispositivo extender a un control que ya ha agregado a una página de tareas. Tenga en cuenta, además, establecer control de propiedades de extensor abriendo la hoja de propiedades para el control que se va a extender.

Un control ASP.NET solo se puede extender mediante varios extensores de control. La hoja de propiedades para el control que se va a extender mostrará una lista de todos los extensores de control asociados al control.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
