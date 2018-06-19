---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 2 | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875453"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de MVC de ASP.NET.


## <a name="adding-an-automatic-datetime-template"></a>Agregar una plantilla de fecha y hora automática

En la primera parte de este tutorial, ha visto cómo puede agregar atributos al modelo para especificar explícitamente el formato y cómo se puede especificar explícitamente la plantilla que se usa para representar el modelo. Por ejemplo, el [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) especifica el atributo en el siguiente código de forma explícita el formato para el `ReleaseDate` propiedad.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

En el ejemplo siguiente, la [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo, mediante el `Date` enumeración, especifica que se debe usar la plantilla de fecha para representar el modelo. Si no hay ninguna plantilla de fecha en el proyecto, se usa la plantilla de fechas integrados.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Sin embargo, ASP. MVC puede realizar con la convención-over-configuración, buscando una plantilla que coincida con el nombre de un tipo de coincidencia de tipos. Esto le permite crear una plantilla que automáticamente da formato a datos sin usar ningún atributo o el código en absoluto. Para esta parte del tutorial, creará una plantilla que se aplica automáticamente a las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). No tendrá que utilizar un atributo u otra configuración para especificar que se debe usar la plantilla para representar todas las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

También obtendrá información sobre una forma de personalizar la visualización de propiedades o campos individuales incluso individuales.

Para empezar, vamos a quitar la información de formato existente y mostrar fechas completas en la aplicación.

Abra la *Movie.cs* de archivos y marque como comentario el `DataType` del atributo en el `ReleaseDate` propiedad:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Presione CTRL+F5 para ejecutar la aplicación.

Tenga en cuenta que el `ReleaseDate` propiedad ahora muestra la fecha y la hora, dado que es el predeterminado cuando no se proporciona ninguna información de formato.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Agregar estilos CSS para probar nuevas plantillas

Antes de crear una plantilla para aplicar formato a fechas, agregará algunas reglas de estilo CSS que se pueden aplicar a las nuevas plantillas. Le ayudará a comprobar que la página representada está utilizando la nueva plantilla.

Abra la *Content\Site.cs*s archivo y agregue las siguientes reglas CSS a la parte inferior del archivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Agregar plantillas de presentación de fecha y hora

Ahora puede crear la nueva plantilla. En el *Views\Movies* carpeta, cree un *DisplayTemplates* carpeta.

En el *Views\Shared* carpeta, cree un *DisplayTemplates* carpeta y un *EditorTemplates* carpeta.

Las plantillas de presentación en el *Views\Shared\DisplayTemplates* carpeta se usará en todos los controladores. Las plantillas de presentación en el *Views\Movie\DisplayTemplates* carpeta va a usar solo el `Movie` controlador. (Si aparece una plantilla con el mismo nombre en ambas carpetas, la plantilla en el *Views\Movie\DisplayTemplates* carpeta, es decir, la plantilla más específica, tiene prioridad para las vistas devuelto por el `Movie` controlador.)

En **el Explorador de soluciones**, expanda la *vistas* carpeta, expanda la *Shared* carpeta y, a continuación, haga la *Views\Shared\DisplayTemplates* carpeta.

Haga clic en **agregar** y, a continuación, haga clic en **vista**. El **agregar vista** se muestra el cuadro de diálogo.

En el **nombre de la vista** , escriba `DateTime`. (Use este nombre para que coincida con el nombre del tipo).

Seleccione el **crear como una vista parcial** casilla de verificación. Asegúrese de que el **usar una página de diseño o maestra** y **crear una vista fuertemente tipada** no están activadas las casillas de verificación.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Haga clic en **Agregar**. A *DateTime.cshtml* plantilla se crea en el *Views\Shared\DisplayTemplates*.

La siguiente imagen muestra la *vistas* carpeta en **el Explorador de soluciones** después de que el `DateTime` se crean las plantillas de presentación y el editor.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra la *Views\Shared\DisplayTemplates\DateTime.cshtml* de archivos y agregue el siguiente marcado, que usa el [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) método para dar formato a la propiedad como una fecha sin la hora. (El `{0:d}` formato especifica el formato de fecha corta.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita este paso para crear un `DateTime` plantilla en el *Views\Movie\DisplayTemplates* carpeta. Utilice el código siguiente en el *Views\Movie\DisplayTemplates\DateTime.cshtml* archivo.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La `loud-1` clase CSS que hace que la fecha se muestre en texto rojo y negrita. Ha agregado la `loud-1` clase CSS simplemente como una medida temporal para que pueda ver fácilmente cuándo se está usando esta plantilla en particular.

Lo que ha hecho, se crea y plantillas que ASP.NET usará para mostrar las fechas personalizadas. La plantilla más general (en el *Views\Shared\DisplayTemplates* carpeta) muestra una fecha corta simple. La plantilla que está específicamente diseñado para la `Movie` controlador (en el *Views\Movies\DisplayTemplates* carpeta) muestra una fecha corta que también se da formato como texto rojo y negrita.

Presione CTRL+F5 para ejecutar la aplicación. El explorador representa la vista de índice para la aplicación.

El `ReleaseDate` propiedad ahora muestra la fecha en negrita rojo sin la hora. Esto le ayudará a confirmar que la `DateTime` aplicación auxiliar con plantilla en el *Views\Movies\DisplayTemplates* carpeta tiene prioridad sobre la `DateTime` aplicación auxiliar con plantilla en la carpeta compartida (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Ahora el nombre de la *Views\Movies\DisplayTemplates\DateTime.cshtml* del archivo a *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Presione CTRL+F5 para ejecutar la aplicación.

Esta vez el `ReleaseDate` propiedad muestra una fecha sin la hora y sin la fuente de color rojo negrita. Esto ilustra que el tipo de una plantilla que tiene el nombre de los datos (en este caso `DateTime`) se usa automáticamente para mostrar todas las propiedades del modelo de ese tipo. Después de cambiar el nombre de la *DateTime.cshtml* del archivo a *LoudDateTime.cshtml*, ASP.NET ya no se encuentra una plantilla en el *Views\Movies\DisplayTemplates* carpeta, por lo que usar el *DateTime.cshtml* plantilla desde el * Views\Movies\Shared\* carpeta.

(La plantilla coincidencia distingue mayúsculas de minúsculas, por lo que podría haber creado el nombre de archivo de plantilla con las mayúsculas y minúsculas. Por ejemplo, *DATETIME.chstml, datetime.cshtml*, y *DaTeTiMe.cshtml* coincidiría con todos los `DateTime` tipo.)

Para revisar: en este momento, el `ReleaseDate` campo se muestra mediante el *Views\Movies\DisplayTemplates\DateTime.cshtml* plantilla, que muestra los datos utilizando un formato de fecha corta, pero en caso contrario, se agrega ningún formato especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usar UIHint para especificar una plantilla de presentación

Si la aplicación web tiene muchos `DateTime` campos y desea mostrar todos o la mayoría de ellos en formato de solo fecha, de forma predeterminada el *DateTime.cshtml* plantilla es un enfoque adecuado. Pero ¿qué ocurre si tiene unas fechas donde desea que aparezca la fecha completa y hora? No hay problema. Puede crear una plantilla de adicional y usar el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo para especificar el formato de fecha completa y hora. A continuación, puede aplicar de forma selectiva esa plantilla. Puede usar el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo en el nivel de modelo o puede especificar la plantilla dentro de una vista. En esta sección, verá cómo utilizar el `UIHint` atributo para cambiar el formato de algunas instancias de campos de fecha y hora de manera selectiva.

Abra la *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* archivo y reemplace el código existente con lo siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Esto hace que la fecha y hora completas que se mostrará y agrega la clase CSS que hace que el texto verde y grandes.

Abra el *Movie.cs* de archivos y agregar el [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribuir a la `ReleaseDate` propiedad, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Esto indica a ASP.NET MVC que cuando se muestre la `ReleaseDate` propiedad (en concreto y no cualquier `DateTime` objeto), se debe utilizar el *LoudDateTime.cshtml* plantilla.

Presione CTRL+F5 para ejecutar la aplicación.

Tenga en cuenta que el `ReleaseDate` propiedad ahora muestra la fecha y hora en una fuente grande de color verde.

Vuelva a la `UIHint` de atributo en el *Movie.cs* de archivos y marcarla como comentario fuera por lo que la *LoudDateTime.cshtml* no puede usar la plantilla. Vuelva a ejecutar la aplicación. No se muestra la fecha de lanzamiento grandes y verde. Esto comprueba que la *Views\Shared\DisplayTemplates\DateTime.cshtml* plantilla se usa en las vistas de índice y los detalles.

Como se mencionó anteriormente, también puede aplicar una plantilla en una vista, que le permite aplicar la plantilla a una instancia individual de algunos datos. Abra la *Views\Movies\Details.cshtml* vista. Agregar `"LoudDateTime"` como el segundo parámetro de la [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) llamar a para el `ReleaseDate` campo. El código completo es similar al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Esto especifica que el `LoudDateTime` plantilla debe utilizarse para mostrar la propiedad de modelo, independientemente de qué atributos se aplican al modelo.

Presione CTRL+F5 para ejecutar la aplicación.

Compruebe que está usando la página de índice de la película el *Views\Shared\DisplayTemplates\DateTime.cshtml* plantilla (rojo negrita) y la *Movie\Details* página está usando el *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* plantilla (grande y verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

En la siguiente sección, creará una plantilla para un tipo complejo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
