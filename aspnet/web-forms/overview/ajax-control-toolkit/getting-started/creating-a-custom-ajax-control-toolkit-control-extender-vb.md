---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Crear un personalizado de AJAX Control extensor de Control Toolkit (VB) | Microsoft Docs
author: microsoft
description: Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f0cbee47b541e31f3e9f01e42afeabcd7b9769f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829792"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Crear un extensor de Control del Kit de herramientas de Control personalizado de AJAX (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles de ASP.NET sin tener que crear nuevas clases.


En este tutorial, obtendrá información sobre cómo crear un extensor de control personalizado de AJAX Control Toolkit. Crearemos una sencilla pero útil, nuevo extensor que cambia el estado de un botón de deshabilitado a habilitado cuando se escribe texto en un cuadro de texto. Después de leer este tutorial, podrá ampliar el Kit de herramientas de AJAX de ASP.NET con sus propios controles extensores.

Puede crear los extensores de control personalizado con Visual Studio o Visual Web Developer (asegúrese de que tiene la versión más reciente de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Información general del extensor DisabledButton

Nuestro nuevo extensor de control se denomina el extensor DisabledButton. Este extensor tendrá tres propiedades:

- TargetControlID - el cuadro de texto que se extiende el control.
- TargetButtonIID - el botón que se ha deshabilitado o habilitado.
- DisabledText - el texto que se muestra inicialmente en el botón. Cuando empiece a escribir, el botón muestra el valor de la propiedad de texto del botón.

Enlazar el extensor DisabledButton a un control TextBox y Button. Antes de escribir cualquier texto, el botón está deshabilitado y el TextBox y Button este aspecto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Después de que comience a escribir texto, el botón está habilitado y el TextBox y Button este aspecto:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Para crear el extensor de control, es necesario crear los tres archivos siguientes:

- DisabledButtonExtender.vb: este archivo es la clase de control de servidor que administrará la creación del dispositivo extender y permiten establecer las propiedades en tiempo de diseño. También define las propiedades que se pueden establecer en el dispositivo extender. Estas propiedades son accesibles a través del código y en tiempo de diseño y coinciden con las propiedades definidas en el archivo DisableButtonBehavior.js.
- DisabledButtonBehavior.js--Este archivo es donde agregará todos de la lógica de script de cliente.
- DisabledButtonDesigner.vb - esta clase habilita la funcionalidad de tiempo de diseño. Necesita esta clase si desea que el extensor de control para que funcione correctamente con el Diseñador de Visual Studio o Visual Web Developer.

Por lo que un extensor de control consta de un control de servidor, un comportamiento del lado cliente y una clase de diseñador del lado servidor. Aprenda a crear los tres de estos archivos en las secciones siguientes.

## <a name="creating-the-custom-extender-website-and-project"></a>Crear el proyecto y el sitio Web de extensor personalizado

El primer paso es crear un proyecto de biblioteca de clases y el sitio Web en Visual Studio o Visual Web Developer. Nos ll crear el extensor personalizado en el proyecto de biblioteca de clases y probar el extensor personalizado en el sitio Web.

Permiten s iniciar con el sitio Web. Siga estos pasos para crear el sitio Web:

1. Seleccione la opción de menú **archivo, nuevo sitio Web**.
2. Seleccione el **sitio Web de ASP.NET** plantilla.
3. Nombre del nuevo sitio Web *Website1*.
4. Haga clic en el **Aceptar** botón.

A continuación, se debe crear el proyecto de biblioteca de clases que contendrá el código para el extensor de control:

1. Seleccione la opción de menú **nuevo proyecto de archivos, agregar,**.
2. Seleccione el **biblioteca de clases** plantilla.
3. Nombre de la nueva biblioteca de clases con el nombre **CustomExtenders**.
4. Haga clic en el **Aceptar** botón.

Después de completar estos pasos, la ventana Explorador de soluciones debería parecerse a la figura 1.


[![Solución de proyecto de biblioteca de clase y de sitio Web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: solución de proyecto de biblioteca de clase y de sitio Web ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


A continuación, deberá agregar todas las referencias de ensamblado necesarias para el proyecto de biblioteca de clases:

1. Haga clic en el proyecto CustomExtenders y seleccione la opción de menú **Agregar referencia**.
2. Seleccione la pestaña. NET.
3. Agregue referencias a los siguientes ensamblados:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Seleccione la pestaña Examinar.
5. Agregue una referencia al ensamblado AjaxControlToolkit.dll. Este ensamblado se encuentra en la carpeta donde descargó el AJAX Control Toolkit.

Puede comprobar que ha agregado todas las referencias de la derecha haciendo clic en el proyecto, seleccione Propiedades y, al hacer clic en la pestaña referencias (consulte la figura 2).


[![Carpeta de referencias con las referencias necesarias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: carpeta de referencias con las referencias necesarias ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Crear el extensor de Control personalizado

Ahora que tenemos nuestra biblioteca de clases, podemos comenzar a crear nuestro control extensor. Permiten s comenzar con el nivel más básico de una clase de control extensor personalizado (consulte el listado 1).

**Listado 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Hay varias cosas que observará acerca de la clase del extensor de control en el listado 1. En primer lugar, tenga en cuenta que la clase hereda de la clase ExtenderControlBase base. Todos los controles extensores de AJAX Control Toolkit se derivan de esta clase base. Por ejemplo, la clase base incluye la propiedad TargetID que es una propiedad obligatoria de cada extensor de control.

A continuación, observe que la clase incluye los siguientes dos atributos relacionados con el script de cliente:

- WebResource - hace que un archivo se incluye como un recurso incrustado en un ensamblado.
- ClientScriptResource - hace que un recurso de script que deben recuperarse de un ensamblado.

El atributo WebResource se utiliza para incrustar el archivo MyControlBehavior.js JavaScript en el ensamblado cuando se compila el extensor personalizado. El atributo ClientScriptResource se usa para recuperar el script MyControlBehavior.js desde el ensamblado cuando se usa el extensor personalizado en una página web.


En el orden de los atributos de WebResource y ClientScriptResource funcione, debe compilar el archivo JavaScript como un recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *recurso incrustado* a la **acción de compilación** propiedad.


Tenga en cuenta que el extensor de control también incluye un atributo TargetControlType. Este atributo se utiliza para especificar el tipo de control que se extiende por el extensor de control. En el caso de la lista 1, se usa el extensor de control para ampliar un cuadro de texto.

Por último, tenga en cuenta que el extensor incluye una propiedad denominada MyProperty. La propiedad se marca con el atributo ExtenderControlProperty. Los métodos GetPropertyValue() y SetPropertyValue() se utilizan para pasar el valor de propiedad desde el extensor de control de servidor para el comportamiento del lado cliente.

Permiten s continúe e implemente el código para el extensor DisabledButton. El código para este extensor puede encontrarse en el listado 2.

**Listado 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

El extensor DisabledButton en el listado 2 tiene dos propiedades denominadas TargetButtonID y DisabledText. El IDReferenceProperty aplicado a la propiedad TargetButtonID evita que cualquier cosa que no sea el identificador de un control de botón se asignen a esta propiedad.

Los atributos de WebResource y ClientScriptResource asocian un comportamiento de cliente ubicado en un archivo denominado DisabledButtonBehavior.js con este extensor. Trataremos este archivo de JavaScript en la sección siguiente.

## <a name="creating-the-custom-extender-behavior"></a>Crear el comportamiento del extensor personalizado

El componente de cliente de un extensor de control se denomina un comportamiento. La lógica real para deshabilitar y habilitar el botón está contenida en el comportamiento de DisabledButton. El código de JavaScript para el comportamiento se incluye en el listado 3.

**Listado 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

El archivo de JavaScript en el listado 3 contiene una clase de cliente denominada DisabledButtonBehavior. Esta clase, como su gemelo del lado servidor, incluye dos propiedades denominadas TargetButtonID y obtenga DisabledText que puede tener acceso mediante\_TargetButtonID/set\_TargetButtonID y obtenga\_DisabledText/set\_ DisabledText.

El método initialize() asocia un controlador de evento keyup con el elemento de destino para el comportamiento. Cada vez que escriba una letra en el cuadro de texto asociado a este comportamiento, se ejecuta el controlador keyup. El controlador keyup habilita o deshabilita el botón dependiendo de si el cuadro de texto asociado con el comportamiento contiene cualquier texto.

Recuerde que debe compilar el archivo de JavaScript en el listado 3 como recurso incrustado. Seleccione el archivo en la ventana Explorador de soluciones, abra la hoja de propiedades y asigne el valor *recurso incrustado* a la **acción de compilación** propiedad (consulte la figura 3). Esta opción está disponible en Visual Studio y Visual Web Developer.


[![Adición de un archivo JavaScript como un recurso incrustado](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: adición de un archivo JavaScript como un recurso incrustado ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Creando el Diseñador de extensor personalizado

Hay una clase último que se debe crear para completar nuestro extensor. Es necesario crear la clase de diseñador en el listado 4. Esta clase es necesaria para que el extensor se comporten correctamente con el Diseñador de Visual Studio o Visual Web Developer.

**Listado 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Asociar el diseñador en el listado 4 con el extensor DisabledButton con el atributo de diseñador. Deberá aplicar el atributo de diseñador a la clase DisabledButtonExtender similar al siguiente:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Usar el extensor personalizado

Ahora que hemos terminado de crear el extensor de control DisabledButton, es el tiempo para usarlo en nuestro sitio Web ASP.NET. En primer lugar, necesitamos agregar el extensor personalizado al cuadro de herramientas. Siga estos pasos:

1. Haga doble clic en la página en la ventana Explorador de soluciones, abra una página ASP.NET.
2. Haga clic en el cuadro de herramientas y seleccione la opción de menú **elegir elementos**.
3. En el cuadro de diálogo Elegir elementos del cuadro de herramientas, busque el ensamblado CustomExtenders.dll.
4. Haga clic en el **Aceptar** botón para cerrar el cuadro de diálogo.

Después de completar estos pasos, el extensor de control DisabledButton debe aparecer en el cuadro de herramientas (consulte la figura 4).


[![DisabledButton en el cuadro de herramientas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton en el cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


A continuación, se debe crear una nueva página ASP.NET. Siga estos pasos:

1. Cree una nueva página ASP.NET denominada ShowDisabledButton.aspx.
2. Arrastre un ScriptManager en la página.
3. Arrastre un control de cuadro de texto a la página.
4. Arrastre un control de botón en la página.
5. En la ventana Propiedades, cambie la propiedad de Id. del botón en el valor <em>btnSave</em> y la propiedad de texto en el valor *guardar\**.
  

Hemos creado una página con un control ASP.NET TextBox y Button estándar.

A continuación, necesitamos ampliar el control de cuadro de texto con el extensor DisabledButton:

1. Seleccione el **Agregar extensor** opción para abrir el cuadro de diálogo del Asistente para Extender la tarea (consulte la figura 5). Tenga en cuenta que el cuadro de diálogo incluye el extensor DisabledButton personalizado.
2. Seleccione el extensor DisabledButton y haga clic en el **Aceptar** botón.


[![El cuadro de diálogo del Asistente para Extender](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: cuadro de diálogo del Asistente de extensor ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Por último, podemos establecer las propiedades del extensor DisabledButton. Puede modificar las propiedades del extensor DisabledButton modificando las propiedades del control de cuadro de texto:

1. Seleccione el cuadro de texto en el diseñador.
2. En la ventana Propiedades, expanda el nodo de extensores (consulte la figura 6).
3. Asigne el valor *guardar* a la propiedad DisabledText y el valor *btnSave* a la propiedad TargetButtonID.


[![Establecer las propiedades de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: establecer las propiedades de extensor ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Al ejecutar la página (presionando F5), el control de botón está deshabilitado inicialmente. Tan pronto como se comienza a escribir texto en el cuadro de texto, el botón de control está habilitado (consulte la figura 7).


[![El extensor DisabledButton en acción](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: DisabledButton el extensor en acción ([haga clic aquí para ver imagen en tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede ampliar el AJAX Control Toolkit con controles de extensor personalizado. En este tutorial, creamos un extensor de control DisabledButton simple. Este extensor se implementa mediante la creación de una clase DisabledButtonExtender, un comportamiento DisabledButtonBehavior JavaScript y una clase DisabledButtonDesigner. Siga un conjunto similar de pasos cada vez que cree un extensor de control personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
