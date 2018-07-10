---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Uso de los controles de AJAX Control Toolkit y los controles extensores (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 2c978bec3780ef2e83aee32a084d1baf5066e0e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817231"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Uso de los controles de AJAX Control Toolkit y los controles extensores (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo agregar controles de AJAX Control Toolkit y extensores para las páginas ASP.NET.


AJAX Control Toolkit contiene un conjunto de controles y extensores de control. En este breve tutorial, obtendrá información sobre cómo agregar controles y extensores de control a una página ASP.NET.

> [!NOTE] 
> 
> Para obtener instrucciones sobre cómo instalar AJAX Control Toolkit y adición de AJAX Control Toolkit al cuadro de herramientas de Visual Studio o Visual Web Developer, vea el tutorial [Introducción a AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>Uso de los controles de AJAX Control Toolkit

Un control de AJAX Control Toolkit funciona igual que un control ASP.NET normal. Puede arrastrar el control de cuadro de herramientas a una página ASP.NET. Puede agregar el control a la página en la vista Diseño o vista del origen.

Hay uno de los requisitos especial al utilizar los controles de AJAX Control Toolkit. La página debe contener un control ScriptManager. El control ScriptManager es responsable de incluir todo el código JavaScript necesario requerido por los controles de AJAX Control Toolkit.

Por ejemplo, la pestaña de AJAX Control Toolkit incluye un control denominado el control del Editor. Este control muestra un editor de HTML enriquecido. Siga estos pasos para agregar el control del Editor a una página:

1. Cree una nueva página ASP.NET denominada ShowEditor.aspx
2. Seleccione el control ScriptManager de la pestaña Extensiones de AJAX en el cuadro de herramientas y arrastre el control a la página.
3. Seleccione el control del Editor de la pestaña de AJAX Control Toolkit en el cuadro de herramientas y arrastre el control a la página (consulte la figura 1). El diseñador debería parecerse a la figura 2.
4. Ejecute el sitio web de seleccionando la opción de menú **depurar, Iniciar depuración** o presionar la tecla F5.
5. Debería ver la página en la figura 3.


[![Selecciona el control de Editor HTML](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Figura 01**: selecciona el control de Editor HTML ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Diseñador de Visual Studio con el control ScriptManager y edición](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Figura 02**: Diseñador de Visual Studio con el control ScriptManager y edición ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![La página DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Figura 03**: DisplayEditor.aspx la página ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Uso de los extensores de AJAX Control Toolkit Control

El Kit de herramientas de Control de AJAX también contiene los extensores de control. Como sugiere su nombre, un extensor de control amplía la funcionalidad de un control existente. Por ejemplo, el extensor de control ConfirmButton amplía el control de botón de ASP.NET estándar. El extensor cambia el comportamiento de s de control de botón para que el botón muestra un cuadro de diálogo de confirmación cuando haga clic en él.

Un extensor de control, al igual que un control de AJAX Control Toolkit, requiere un control ScriptManager. Debe agregar un control ScriptManager a una página para empezar a usar los controles extensores en la página.

Siga estos pasos para usar el extensor de control ConfirmButton:

1. Cree una nueva página ASP.NET denominada ShowConfirmButton.aspx
2. Agregue un control ScriptManager en la página, arrastre el control a la página de la pestaña Extensiones de AJAX.
3. Agregue un control de botón estándar a la página arrastrando el botón de la ficha estándar en el cuadro de herramientas hasta la superficie del diseñador.
4. Haga clic en el **Agregar extensor** opción de tarea (consulte la figura 4).
5. En el cuadro de diálogo Elegir extensor, seleccione ConfirmButtonExtender (consulte la figura 5) y haga clic en el botón Aceptar.
6. Seleccione el control de botón en el diseñador y expanda los extensores, Button1\_ConfirmButtonExtender nodo en la ventana Propiedades (consulte la figura 6). Asigne el valor *realmente?* a la propiedad ConfirmText.
7. Ejecute la página seleccionando la opción de menú **depurar, Iniciar depuración** o presione la tecla F5.


[![La opción de la tarea de agregar Extender](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Figura 04**: opción de la tarea de agregar el extensor ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![Seleccionar el extensor ConfirmButton de control](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Figura 05**: seleccionar el extensor de control ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Establecer una propiedad ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Figura 06**: establecer una propiedad ConfirmButton ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Cuando se abre la página, verá un botón. Al hacer clic en el botón, obtenga el cuadro de diálogo de confirmación en la figura 7.


[![Mostrar el cuadro de diálogo de confirmación](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Figura 07**: mostrar el cuadro de diálogo de confirmación ([haga clic aquí para ver imagen en tamaño completo](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Tenga en cuenta que normalmente no arrastre un extensor de control a una página. En su lugar, usa el **Agregar extensor** opción para agregar un dispositivo extender a un control que ya ha agregado a una página de tareas. Además, tenga en cuenta establece control propiedades extensoras abriendo la hoja de propiedades para el control que se va a extender.

Un único control ASP.NET puede ampliarse mediante varios extensores de control. La hoja de propiedades para el control que se va a extender enumerará todos los extensores de control asociados al control.

> [!div class="step-by-step"]
> [Anterior](get-started-with-the-ajax-control-toolkit-cs.md)
> [Siguiente](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
