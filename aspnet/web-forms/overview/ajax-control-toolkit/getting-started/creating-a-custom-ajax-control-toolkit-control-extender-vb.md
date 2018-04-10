---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Crear una personalizada AJAX Control extensor de Control del Kit de herramientas (VB) | Documentos de Microsoft
author: microsoft
description: Extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Creación de un extensor de Control del Kit de herramientas de Control de AJAX personalizado (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.


En este tutorial, aprenderá a crear un extensor de control AJAX Control Toolkit personalizado. Se crea un sencillo, pero útil, nuevo extensor que cambia el estado de un botón de deshabilitada a habilitada cuando se escribe texto en un cuadro de texto. Después de leer este tutorial, podrá ampliar el Kit de herramientas de AJAX de ASP.NET con su propios dispositivos Extender de control.

Puede crear extensores de control personalizado con Visual Studio o Visual Web Developer (asegúrese de que tiene la versión más reciente de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Información general sobre el dispositivo Extender DisabledButton

Nuestro nuevo extensor de control se denomina el extensor de DisabledButton. Este extensor tendrá tres propiedades:

- TargetControlID - el cuadro de texto que se extiende el control.
- TargetButtonIID - el botón que está habilitado o deshabilitado.
- DisabledText - el texto que se muestra inicialmente en el botón. Cuando empiece a escribir, el botón muestra el valor de la propiedad de texto del botón.

Enlazar el extensor de DisabledButton a un cuadro de texto y un control Button. Antes de escribir cualquier texto, el botón está deshabilitado y el cuadro de texto y el botón un aspecto similar al siguiente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Después de que comience a escribir texto, el botón está habilitado y el cuadro de texto y el botón un aspecto similar al siguiente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Para crear nuestra extensor de control, es preciso crear los tres archivos siguientes:

- DisabledButtonExtender.vb: este archivo es la clase de control de servidor que administrará la creación del dispositivo extender y le permiten establecer las propiedades en tiempo de diseño. También define las propiedades que se pueden establecer en el dispositivo extender. Estas propiedades son accesibles a través del código y en tiempo de diseño y coincidan con las propiedades definidas en el archivo DisableButtonBehavior.js.
- DisabledButtonBehavior.js: Este archivo es donde agregará todos de la lógica de script de cliente.
- DisabledButtonDesigner.vb - esta clase habilita la funcionalidad en tiempo de diseño. Necesita esta clase si desea que el extensor de control para funcionar correctamente con el Diseñador de Visual Studio o Visual Web Developer.

Por lo que un extensor de control consta de un control de servidor, un comportamiento de cliente y una clase de diseñador de servidor. Obtenga información acerca de cómo crear los tres de estos archivos en las secciones siguientes.

## <a name="creating-the-custom-extender-website-and-project"></a>Crear el sitio Web de Extender personalizado y el proyecto

El primer paso es crear un proyecto de biblioteca de clases y el sitio Web en Visual Studio o Visual Web Developer. Se ll crear el extensor personalizado en el proyecto de biblioteca de clases y probar el extensor personalizado en el sitio Web.

Permiten s iniciar con el sitio Web. Siga estos pasos para crear el sitio Web:

1. Seleccione la opción de menú **archivo, nuevo sitio Web**.
2. Seleccione el **sitio Web de ASP.NET** plantilla.
3. Nombre del nuevo sitio Web *Website1*.
4. Haga clic en el **Aceptar** botón.

A continuación, necesitamos crear el proyecto de biblioteca de clases que contendrá el código para el extensor de control:

1. Seleccione la opción de menú **archivos, agregar, nuevo proyecto**.
2. Seleccione el **biblioteca de clases** plantilla.
3. El nombre de la nueva biblioteca de clases con el nombre **CustomExtenders**.
4. Haga clic en el **Aceptar** botón.

Después de completar estos pasos, la ventana del explorador de soluciones debería parecerse a la figura 1.


[![Solución con el proyecto de biblioteca de clase y de sitio Web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: solución con el proyecto de biblioteca de clase y de sitio Web ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


A continuación, debe agregar todas las referencias de ensamblado necesarias para el proyecto de biblioteca de clases:

1. Haga clic en el proyecto CustomExtenders y seleccione la opción de menú **Agregar referencia**.
2. Seleccione la pestaña. NET.
3. Agregue referencias a los siguientes ensamblados:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Seleccione la ficha Examinar.
5. Agregue una referencia al ensamblado AjaxControlToolkit.dll. Este ensamblado se encuentra en la carpeta donde descargó el Kit de herramientas de Control de AJAX.

Puede comprobar que ha agregado todas las referencias de la derecha haciendo clic en el proyecto, seleccione Propiedades y haga clic en la ficha referencias (consulte la figura 2).


[![En la carpeta referencias con las referencias necesarias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: en la carpeta referencias con las referencias necesarias ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Crear el extensor de Control personalizado

Ahora que tenemos nuestra biblioteca de clases, se puede iniciar la creación de nuestro control extensor. Permiten s iniciar con el nivel más básico de una clase de control extensor personalizado (consulte el listado 1).

**Lista 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Hay varios aspectos que tenga en cuenta acerca de la clase de control extensor en la lista 1. En primer lugar, tenga en cuenta que la clase hereda de la clase ExtenderControlBase base. Todos los controles extensores de AJAX Control Toolkit derivan de esta clase base. Por ejemplo, la clase base incluye la propiedad TargetID que es una propiedad obligatoria del extensor de cada control.

A continuación, tenga en cuenta que la clase incluye los siguientes dos atributos relacionados con script de cliente:

- Recursos Web - hace que un archivo que debe incluirse como un recurso incrustado en un ensamblado.
- ClientScriptResource - hace que un recurso de script va a recuperar de un ensamblado.

El atributo de recursos Web se usa para incrustar el archivo MyControlBehavior.js JavaScript en el ensamblado cuando se compila el extensor personalizado. El atributo ClientScriptResource se usa para recuperar el script MyControlBehavior.js desde el ensamblado cuando se usa el extensor personalizado en una página web.


En el orden de los atributos de recursos Web y ClientScriptResource para que funcione, debe compilar el archivo de JavaScript como un recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asignar el valor *recurso incrustado* a la **acción de compilación** propiedad.


Tenga en cuenta que el extensor de control también incluye un atributo TargetControlType. Este atributo se utiliza para especificar el tipo de control que se extiende por el extensor de control. En el caso de listado 1, se usa el extensor de control para extender un cuadro de texto.

Por último, tenga en cuenta que el dispositivo extender personalizado incluye una propiedad denominada MyProperty. La propiedad se marca con el atributo ExtenderControlProperty. Los métodos GetPropertyValue() y SetPropertyValue() se utilizan para pasar el valor de propiedad desde el extensor de control de servidor para el comportamiento de cliente.

Permiten s continúe e implemente el código para que nuestro extender DisabledButton. El código de este extensor puede encontrarse en el listado 2.

**La lista 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

El extensor de DisabledButton en el listado 2 tiene dos propiedades denominadas TargetButtonID y DisabledText. El IDReferenceProperty aplicado a la propiedad TargetButtonID evita que cualquier cosa que no sea el identificador de un control de botón se asignen a esta propiedad.

Los atributos de recursos Web y ClientScriptResource asocian un comportamiento de cliente ubicado en un archivo denominado DisabledButtonBehavior.js con este extensor. Trataremos este archivo de JavaScript en la sección siguiente.

## <a name="creating-the-custom-extender-behavior"></a>Crear el comportamiento del extensor personalizado

El componente de cliente de un extensor de control se denomina un comportamiento. La lógica real para deshabilitar y habilitar el botón se encuentra en el comportamiento de DisabledButton. El código de JavaScript para el comportamiento se incluye en el listado 3.

**El listado 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

El archivo de JavaScript en el listado 3 contiene una clase de cliente denominada DisabledButtonBehavior. Esta clase, como su gemelas de servidor incluye dos propiedades denominadas TargetButtonID y DisabledText que se puede tener acceso mediante obtener\_TargetButtonID/set\_TargetButtonID y obtener\_DisabledText/set\_ DisabledText.

El método initialize() asocia un controlador de evento keyup al elemento de destino para el comportamiento. Cada vez que se escribe una letra en el cuadro de texto asociado a este comportamiento, se ejecuta el controlador keyup. El controlador keyup habilita o deshabilita el botón dependiendo de si el cuadro de texto asociado con el comportamiento contiene cualquier texto.

Recuerde que debe compilar el archivo de JavaScript en el listado 3 como un recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asignar el valor *recurso incrustado* a la **acción de compilación** propiedad (consulte la figura 3). Esta opción está disponible en Visual Studio y Visual Web Developer.


[![Agregar un archivo JavaScript como un recurso incrustado](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: agregar un archivo JavaScript como un recurso incrustado ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Crear el diseñador extensor personalizado

Hay una clase último que se debe crear para completar el dispositivo extender. Es necesario crear la clase de diseñador en el listado 4. Esta clase es necesaria para que el dispositivo extender se comporten correctamente con el Diseñador de Visual Studio o Visual Web Developer.

**Listado 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Asociar el diseñador en el listado 4 con el extensor DisabledButton con el atributo Designer. Debe aplicar el atributo Designer para la clase DisabledButtonExtender similar al siguiente:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Usar el extensor personalizado

Ahora que hemos terminado de crear el extensor de control DisabledButton, es el tiempo que desea utilizar en nuestro sitio Web ASP.NET. En primer lugar, necesitamos agregar el dispositivo extender personalizado al cuadro de herramientas. Siga estos pasos:

1. Abra una página ASP.NET haciendo doble clic en la página en la ventana Explorador de soluciones.
2. Haga clic en el cuadro de herramientas y seleccione la opción de menú **elegir elementos**.
3. En el cuadro de diálogo Elegir elementos del cuadro de herramientas, vaya al ensamblado CustomExtenders.dll.
4. Haga clic en el **Aceptar** botón para cerrar el cuadro de diálogo.

Después de completar estos pasos, el extensor de control DisabledButton debe aparecer en el cuadro de herramientas (consulte la figura 4).


[![DisabledButton en el cuadro de herramientas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton en el cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


A continuación, necesitamos crear una nueva página ASP.NET. Siga estos pasos:

1. Crear una nueva página ASP.NET denominada ShowDisabledButton.aspx.
2. Arrastre un ScriptManager a la página.
3. Arrastre un control de cuadro de texto a la página.
4. Arrastre un control de botón a la página.
5. En la ventana Propiedades, cambie la propiedad de Id. de botón en el valor <em>btnSave</em> y la propiedad de texto en el valor *guardar\**.
  

Se crea una página con un control de cuadro de texto de ASP.NET y botón estándar.

A continuación, necesitamos extender el control de cuadro de texto con el extensor DisabledButton:

1. Seleccione el **Agregar extensor** opción para abrir el cuadro de diálogo del Asistente para Extender la tarea (Véase la figura 5). Tenga en cuenta que el cuadro de diálogo incluye el extensor DisabledButton personalizado.
2. Seleccione el dispositivo extender DisabledButton y haga clic en el **Aceptar** botón.


[![El cuadro de diálogo del Asistente para Extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: cuadro de diálogo del asistente Extender ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Por último, podemos establecer las propiedades del extensor DisabledButton. Puede modificar las propiedades del extensor DisabledButton modificando las propiedades del control de cuadro de texto:

1. Seleccione el cuadro de texto en el diseñador.
2. En la ventana Propiedades, expanda el nodo dispositivos Extender (consulte la figura 6).
3. Asigne el valor *guardar* a la propiedad DisabledText y el valor *btnSave* a la propiedad TargetButtonID.


[![Establecer las propiedades de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: establecer propiedades de extensor ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Cuando ejecute la página (presionando F5), el control de botón está inicialmente deshabilitado. Tan pronto como comienza a escribir texto en el cuadro de texto, el botón control está habilitado (consulte la figura 7).


[![El extensor DisabledButton en acción](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: DisabledButton el extensor de acción ([haga clic aquí para ver la imagen a tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede ampliar el Kit de herramientas de Control de AJAX con controles extensores personalizados. En este tutorial, creamos un extensor de control DisabledButton simple. Implementamos este extensor mediante la creación de una clase DisabledButtonExtender, un comportamiento DisabledButtonBehavior JavaScript y una clase DisabledButtonDesigner. Siga un conjunto similar de pasos cada vez que cree un extensor de control personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
