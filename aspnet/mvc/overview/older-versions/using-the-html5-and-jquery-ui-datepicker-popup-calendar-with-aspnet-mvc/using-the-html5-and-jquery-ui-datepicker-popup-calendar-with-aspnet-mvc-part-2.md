---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 2 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: aspnetcontent
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 0dc9957bbb13394a4ee225d9966f4004de5714b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839312"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Adición de una plantilla de fecha y hora automática

En la primera parte de este tutorial, ha visto cómo puede agregar atributos al modelo para especificar explícitamente el formato y cómo puede especificar explícitamente la plantilla que se usa para representar el modelo. Por ejemplo, el [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo en el explícitamente de código siguiente especifica el formato para el `ReleaseDate` propiedad.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

En el ejemplo siguiente, la [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo, utilizando el `Date` enumeración, especifica que se debe usar la plantilla de fecha para representar el modelo. Si no hay ninguna plantilla de fecha en el proyecto, se usa la plantilla de fechas integrada.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Sin embargo, ASP. MVC puede realizar mediante convención-over-configuration, mediante la búsqueda de una plantilla que coincida con el nombre de un tipo de coincidencia de tipos. Esto le permite crear una plantilla que automáticamente da formato a datos sin usar cualquier código o atributos en absoluto. Para esta parte del tutorial, creará una plantilla que se aplica automáticamente a las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). No tendrá que utilizar un atributo u otra configuración para especificar que la plantilla se utiliza para representar todas las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

También aprenderá una manera de personalizar la visualización de propiedades o campos individuales incluso individuales.

Para empezar, vamos a quitar la información de formato existente y mostrar fechas completas en la aplicación.

Abra el *Movie.cs* de archivos y marque como comentario el `DataType` atributo el `ReleaseDate` propiedad:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Presione CTRL+F5 para ejecutar la aplicación.

Tenga en cuenta que el `ReleaseDate` propiedad ahora muestra la fecha y la hora, ya que es el valor predeterminado cuando no se proporciona ninguna información de formato.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Agregar estilos CSS para probar nuevas plantillas

Antes de crear una plantilla para aplicar formato a fechas, agregará algunas reglas de estilo CSS que se pueden aplicar a las nuevas plantillas. Le ayudará a comprobar que la página representada está utilizando la nueva plantilla.

Abra el *Content\Site.cs*s archivo y agregue las siguientes reglas CSS a la parte inferior del archivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Agregar las plantillas de presentación de fecha y hora

Ahora puede crear la nueva plantilla. En el *vistas\películas* carpeta, cree un *DisplayTemplates* carpeta.

En el *Views\Shared* carpeta, cree un *DisplayTemplates* carpeta y un *EditorTemplates* carpeta.

Las plantillas de presentación en el *Views\Shared\DisplayTemplates* carpeta se usará en todos los controladores. Las plantillas de presentación en el *Views\Movie\DisplayTemplates* carpeta va a usar solo el `Movie` controlador. (Si aparece una plantilla con el mismo nombre en ambas carpetas, la plantilla en el *Views\Movie\DisplayTemplates* carpeta, es decir, la plantilla más específica, tiene prioridad para las vistas devuelto por la `Movie` controlador.)

En **el Explorador de soluciones**, expanda el *vistas* carpeta, expanda la *Shared* carpeta y, a continuación, con el botón secundario el *Views\Shared\DisplayTemplates* carpeta.

Haga clic en **agregar** y, a continuación, haga clic en **vista**. El **agregar vista** se muestra el cuadro de diálogo.

En el **nombre de la vista** , escriba `DateTime`. (Debe usar este nombre para que coincida con el nombre del tipo).

Seleccione el **crear como una vista parcial** casilla de verificación. Asegúrese de que el **utiliza una página de diseño o maestra** y **crear una vista fuertemente tipada** no están activadas las casillas de verificación.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Haga clic en **Agregar**. Un *DateTime.cshtml* plantilla se crea en el *Views\Shared\DisplayTemplates*.

La siguiente imagen muestra la *vistas* carpeta en **el Explorador de soluciones** después de que el `DateTime` se crean plantillas de presentación y el editor.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra el *Views\Shared\DisplayTemplates\DateTime.cshtml* archivo y agregue el marcado siguiente, que usa el [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) método para dar formato a la propiedad como una fecha sin la hora. (El `{0:d}` formato especifica el formato de fecha corta.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita este paso para crear un `DateTime` plantilla en el *Views\Movie\DisplayTemplates* carpeta. Use el código siguiente en el *Views\Movie\DisplayTemplates\DateTime.cshtml* archivo.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La `loud-1` clase CSS que se hace que la fecha se muestre en texto rojo y negrita. Ha agregado la `loud-1` clase CSS simplemente como una medida temporal para que pueda ver fácilmente cuándo se está usando esta plantilla en particular.

Lo que ha hecho, se crea y plantillas que ASP.NET usará para mostrar las fechas personalizadas. La plantilla más general (en el *Views\Shared\DisplayTemplates* carpeta) muestra una fecha corta simple. La plantilla que está específicamente diseñado para la `Movie` controlador (en el *Views\Movies\DisplayTemplates* carpeta) muestra una fecha corta que también se da formato como texto rojo y negrita.

Presione CTRL+F5 para ejecutar la aplicación. El explorador representa la vista de índice para la aplicación.

El `ReleaseDate` propiedad ahora muestra la fecha en negrita rojo sin incluir el tiempo. Esto le ayudará a confirmar que la `DateTime` aplicación auxiliar con plantilla en el *Views\Movies\DisplayTemplates* se selecciona la carpeta a través de la `DateTime` aplicación auxiliar con plantilla en la carpeta compartida (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Ahora el nombre de la *Views\Movies\DisplayTemplates\DateTime.cshtml* archivo *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Presione CTRL+F5 para ejecutar la aplicación.

Esta vez el `ReleaseDate` propiedad muestra una fecha sin la hora y sin la fuente de color rojo en negrita. Esto ilustra que escribir una plantilla que tiene el nombre de los datos (en este caso `DateTime`) se usa automáticamente para mostrar todas las propiedades del modelo de ese tipo. Después de cambiar el nombre de la *DateTime.cshtml* archivo *LoudDateTime.cshtml*, ASP.NET ya no se encuentra una plantilla en el *Views\Movies\DisplayTemplates* carpeta, por lo que usa el *DateTime.cshtml* plantilla desde el * Views\Movies\Shared\* carpeta.

(La plantilla de coincidencia distingue mayúsculas de minúsculas, por lo que podría haber creado el nombre de archivo de plantilla con las mayúsculas y minúsculas. Por ejemplo, *DATETIME.chstml, datetime.cshtml*, y *DaTeTiMe.cshtml* coincidiría con todos los `DateTime` tipo.)

Revisar: en este momento, el `ReleaseDate` campo se muestra utilizando el *Views\Movies\DisplayTemplates\DateTime.cshtml* plantilla, que muestra los datos utilizando un formato de fecha corta, pero si no agrega ningún formato especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usar UIHint para especificar una plantilla de visualización

Si la aplicación web tiene muchos `DateTime` campos y desea mostrar todos o la mayoría de ellos en formato de solo fecha, de forma predeterminada el *DateTime.cshtml* plantilla es un buen enfoque. Pero ¿qué ocurre si tiene algunas fechas que desea mostrar la fecha y hora completas? No hay problema. Puede crear una plantilla de adicional y usar el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo para especificar el formato de fecha completa y hora. A continuación, puede aplicar selectivamente esa plantilla. Puede usar el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo en el nivel de modelo o puede especificar la plantilla dentro de una vista. En esta sección, verá cómo usar la `UIHint` atributo para cambiar de forma selectiva el formato en algunas instancias de los campos de fecha y hora.

Abra el *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* de archivo y reemplace el código existente con lo siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Esto hace que la fecha y hora completas que se mostrará y agrega la clase CSS que hace que el texto verde y grandes.

Abra el *Movie.cs* archivo y agregue el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo a la `ReleaseDate` propiedad, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Esto indica a ASP.NET MVC que cuando se muestre el `ReleaseDate` propiedad (en concreto y no cualquier `DateTime` objeto), debe usar el *LoudDateTime.cshtml* plantilla.

Presione CTRL+F5 para ejecutar la aplicación.

Tenga en cuenta que el `ReleaseDate` propiedad ahora muestra la fecha y hora en una gran fuente de color verde.

Vuelva a la `UIHint` atributo en el *Movie.cs* de archivos y comentario por lo que la *LoudDateTime.cshtml* no se usará la plantilla. Vuelva a ejecutar la aplicación. No se muestra la fecha de lanzamiento grandes y verde. Esto comprueba que el *Views\Shared\DisplayTemplates\DateTime.cshtml* plantilla se usa en las vistas de Index y Details.

Como se mencionó anteriormente, también puede aplicar una plantilla en una vista, que le permite aplicar la plantilla a una instancia individual de algunos datos. Abra el *Views\Movies\Details.cshtml* vista. Agregar `"LoudDateTime"` como segundo parámetro de la [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) llamar a para el `ReleaseDate` campo. El código completo es similar al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Esto especifica que el `LoudDateTime` plantilla se utiliza para mostrar la propiedad de modelo, independientemente de qué atributos se aplican al modelo.

Presione CTRL+F5 para ejecutar la aplicación.

Compruebe que está usando la página de índice de la película el *Views\Shared\DisplayTemplates\DateTime.cshtml* plantilla (rojo en negrita) y el *Movie\Details* página usa el *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* plantilla (grande y verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

En la siguiente sección, creará una plantilla para un tipo complejo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
