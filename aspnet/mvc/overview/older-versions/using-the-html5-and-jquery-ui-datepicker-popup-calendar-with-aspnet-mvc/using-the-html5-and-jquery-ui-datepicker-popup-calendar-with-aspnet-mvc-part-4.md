---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 4 | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: c6df727107b0a045341badefbf99eec773cd4eff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874790"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Usar el calendario HTML5 y jQuery UI Datepicker emergente con ASP.NET MVC - parte 4
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de MVC de ASP.NET.


### <a name="adding-a-template-for-editing-dates"></a>Agregar una plantilla para editar las fechas

En esta sección creará una plantilla para editar las fechas que se aplicarán cuando ASP.NET MVC muestra la interfaz de usuario para editar propiedades del modelo que se marcan con la **fecha** enumeración de la [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) encuentra. La plantilla representará solo la fecha; no se mostrará el tiempo. En la plantilla usará el [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendario emergente para proporcionar una manera para editar las fechas.

Para comenzar, abra el *Movie.cs* de archivos y agregar el [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribuir a la **fecha** enumeración para la `ReleaseDate` propiedad, como se muestra en el código siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Este código hace que el `ReleaseDate` que se muestre sin la hora en ambos muestran plantillas y editar plantillas de campo. Si la aplicación contiene un *date.cshtml* plantilla en el *Views\Shared\EditorTemplates* carpeta o en la *Views\Movies\EditorTemplates* carpeta, esa plantilla se usará para representar cualquier `DateTime` propiedad durante la edición. En caso contrario, el sistema de creación de plantillas ASP.NET integrado mostrará la propiedad como una fecha.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición para comprobar que el campo de entrada para la fecha de lanzamiento muestra solo la fecha.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

En **el Explorador de soluciones**, expanda la *vistas* carpeta, expanda la *Shared* carpeta y, a continuación, haga la *Views\Shared\EditorTemplates* carpeta.

Haga clic en **agregar**y, a continuación, haga clic en **vista**. El **agregar vista** se muestra el cuadro de diálogo.

En el **nombre de la vista** , escriba &quot;fecha&quot;.

Seleccione el **crear como una vista parcial** casilla de verificación. Asegúrese de que el **usar una página de diseño o maestra** y **crear una vista fuertemente tipada** no están activadas las casillas de verificación.

Haga clic en **Agregar**. El *Views\Shared\EditorTemplates\Date.cshtml* se crea la plantilla.

Agregue el código siguiente a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La primera línea declara el modelo como un `DateTime` tipo. Aunque no es necesario declarar el tipo de modelo en Editar y mostrar las plantillas, es una práctica recomendada para que obtenga el tiempo de compilación la comprobación del modelo que se pasa a la vista. (Otra ventaja es que, a continuación, obtendrá IntelliSense para el modelo en la vista en Visual Studio). Si no se declara el tipo de modelo, ASP.NET MVC considera un [dinámica](https://msdn.microsoft.com/library/dd264741.aspx) escriba y no hay ningún tiempo de compilación la comprobación de tipos. Si se declara el modelo como un `DateTime` tipo, se deja de estar fuertemente tipada.

La segunda línea es simplemente literal marcado HTML que muestra &quot;con plantilla de fecha&quot; antes de un campo de fecha. Usará esta línea temporalmente para comprobar que se está usando esta plantilla de fecha.

La siguiente línea es un [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) auxiliar que presenta un `input` campo que es un cuadro de texto. El tercer parámetro de la aplicación auxiliar utiliza un tipo anónimo para establecer la clase de cuadro de texto a `datefield` y el tipo a `date`. (Dado que `class` es un reservada en C#, debe usar el `@` carácter para escapar el `class` atributo en el analizador de C#.)

El `date` tipo es un tipo de entrada de HTML5 que permite que los exploradores compatibles con HTML5 representar un control de calendario de HTML5. Más adelante, podrá agregar algunos JavaScript para enlazar el datepicker de jQuery para la `Html.TextBox` elemento mediante la `datefield` clase.

Presione CTRL+F5 para ejecutar la aplicación. Puede comprobar que el `ReleaseDate` propiedad en la vista de edición es mediante la plantilla de edición porque muestra la plantilla de &quot;utilizando la plantilla de fecha&quot; justo antes del `ReleaseDate` cuadro, de la entrada de texto como se muestra en esta imagen:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

En el explorador, ver el código fuente de la página. (Por ejemplo, haga clic en la página y seleccione **ver código fuente**.) El siguiente ejemplo muestran algunos el marcado de la página, que ilustra el `class` y `type` atributos en el HTML representado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Vuelva a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla y quite el &quot;con plantilla de fecha&quot; marcado. Ahora la plantilla completada tiene un aspecto similar al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Agregar un calendario emergente de interfaz de usuario Datepicker de jQuery con NuGet

En esta sección agregará el [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente para la plantilla de edición de fecha. El [jQuery UI](http://jqueryui.com/) biblioteca proporciona compatibilidad para animación, efectos y widgets personalizables avanzados. Se se basa en la biblioteca de JavaScript de jQuery. El calendario emergente de datepicker facilita y natural a escribir las fechas con un calendario en lugar de escribir una cadena. El calendario emergente también limita a los usuarios a las fechas legales: entrada de texto normal para una fecha permita escribir algo parecido a `2/33/1999` (febrero 33rd, 1999), pero la [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente no permite que.

En primer lugar, tendrá que instalar las bibliotecas de interfaz de usuario de jQuery. Para ello, deberá usar NuGet, que es un administrador de paquetes que se incluye en las versiones SP1 de Visual Studio 2010 y Visual Web Developer.

En Visual Web Developer, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, seleccione **administrar paquetes de NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: Si la **herramientas** menú no se muestra la **Administrador de paquetes de biblioteca** comando, debe instalar NuGet, siga las instrucciones que aparecen la [instalación de NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) página de el sitio Web de NuGet.   
  
Si está usando Visual Studio en lugar de Visual Web Developer, en la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, seleccione **Agregar referencia de paquetes de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

En el **MVCMovie - administrar paquetes de NuGet** cuadro de diálogo, haga clic en el **en línea** ficha de la izquierda y, a continuación, escriba &quot;jQuery.UI&quot; en el cuadro de búsqueda. Seleccione j **WIdgets de interfaz de usuario de consulta: Datepicker**, a continuación, seleccione la **instalar** botón.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet agrega estas versiones de depuración y versiones reducidas de jQuery principales de la interfaz de usuario y el selector de fecha de jQuery UI al proyecto:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Nota: Las versiones de depuración (los archivos sin la *. min.js* extensión) son útiles para la depuración, pero en un sitio de producción, también debe incluir solo las versiones reducidas.

Para utilizar realmente el selector de fecha de jQuery, debe crear un script de jQuery que enlazará el widget de calendario en la plantilla de edición. En **el Explorador de soluciones**, haga clic en el *Scripts* carpeta y seleccione **agregar**, a continuación, **nuevo elemento**y, a continuación, **JScript Archivo**. Asignar nombre al archivo *DatePickerReady.js*.

Agregue el código siguiente a la *DatePickerReady.js* archivo:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Si no está familiarizado con jQuery, aquí es una breve explicación de lo que esto hace: la primera línea es el &quot;jQuery listo&quot; función, que se llama cuando se han cargado todos los elementos DOM en una página. La segunda línea selecciona todos los elementos DOM que tienen el nombre de clase `datefield`, a continuación, se invoca el `datepicker` función para cada uno de ellos. (Recuerde que ha agregado el `datefield` clase a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla anteriormente en el tutorial.)

A continuación, abra el *Views\Shared\\_Layout.cshtml* archivo. Debe agregar referencias a los archivos siguientes, que son todo lo necesarios para que pueda utilizar el selector de fecha:

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

En el ejemplo siguiente se muestra el código real que debe agregar en la parte inferior de la `head` elemento en el *Views\Shared\\_Layout.cshtml* archivo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

La sección completa `head` sección se muestra aquí:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

El [contenido auxiliar de dirección URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) método convierte la ruta de acceso de recursos en una ruta de acceso absoluta. Debe usar `@URL.Content` hacer referencia correctamente a estos recursos cuando se ejecuta la aplicación en IIS.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición, a continuación, coloque el punto de inserción en el **ReleaseDate** campo. Se muestra el calendario emergente de interfaz de usuario de jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Al igual que la mayoría de los controles de jQuery, el selector de fechas permite personalizarlo en gran medida. Para obtener información, consulte [personalización Visual: diseñar un tema de la interfaz de usuario jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) en el [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) sitio.

### <a name="supporting-the-html5-date-input-control"></a>Compatibilidad con el Control de entrada de fecha de HTML5

Como los exploradores más compatibilidad con HTML5, querrá usar la HTML5 nativo de entrada, como el `date` elemento de entrada y no utilice el calendario de la interfaz de usuario de jQuery. Puede agregar lógica a la aplicación para usar automáticamente controles HTML5 si el explorador admite. Para ello, reemplace el contenido de la *DatePickerReady.js* archivo con lo siguiente:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La primera línea de este script usa Modernizr para comprobar que se admite la entrada de fecha de HTML5. Si no se admite, el selector de fecha de la interfaz de usuario de jQuery se enlaza en su lugar. ([Modernizr](http://www.modernizr.com/docs/) es una biblioteca de JavaScript de código abierto que detecta la disponibilidad de las implementaciones nativas de HTML5 y CSS3. Modernizr se incluye en los proyectos de MVC de ASP.NET creados por usted.)

Después de realizar este cambio, se puede probar mediante un explorador compatible con HTML5, como 11 Opera. Ejecute la aplicación mediante un explorador compatible con HTML5 y editar una entrada de la película. El control de fecha de HTML5 se utiliza en lugar del calendario emergente de interfaz de usuario de jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Dado que las nuevas versiones de los exploradores implementa HTML5 incrementalmente, un buen método para ahora consiste en Agregar código a su sitio Web que dé cabida a una amplia variedad de compatibilidad con HTML5. Por ejemplo, un más sólida *DatePickerReady.js* secuencia de comandos se muestra a continuación que permite los exploradores de compatibilidad de sitio solo parcialmente compatibles con el control de fecha HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Esta secuencia de comandos selecciona HTML5 `input` elementos de tipo `date` que no son totalmente compatibles con el control de fecha HTML5. Para los elementos, enlaza el calendario emergente de interfaz de usuario de jQuery y, a continuación, cambia el `type` de atributo de `date` a `text`. Cambiando el `type` de atributo de `date` a `text`, se ha eliminado la compatibilidad parcial de fecha HTML5. Más robustas *DatePickerReady.js* secuencia de comandos se puede encontrar en [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Agregar fechas que aceptan valores NULL a las plantillas

Si utiliza una de las plantillas de fecha existente y pasar una fecha null, obtendrá un error en tiempo de ejecución. Para hacer que las plantillas de fecha más sólida, va a cambiar para controlar valores null. Para admitir las fechas que aceptan valores NULL, cambie el código de la *Views\Shared\DisplayTemplates\DateTime.cshtml* al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

El código devuelve una cadena vacía cuando el modelo es **null**.

Cambie el código de la *Views\Shared\EditorTemplates\Date.cshtml* archivo al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Cuando este código se ejecuta, si el modelo no es null, el modelo `DateTime` se utiliza el valor. Si el modelo es null, se utiliza en su lugar la fecha actual.

### <a name="wrapup"></a>Wrapup

Este tutorial trata los conceptos básicos de las aplicaciones auxiliares con plantilla de ASP.NET y muestra cómo usar el calendario emergente de jQuery UI datepicker en una aplicación ASP.NET MVC. Para obtener más información, pruebe estos recursos:

- Para obtener información sobre la localización, consulte el blog de Rajeesh [JQueryUI Datepicker en ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obtener información acerca de jQuery UI, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obtener información sobre cómo localizar el control datepicker, consulte [Datepicker/interfaz de usuario/localización](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obtener más información acerca de las plantillas de MVC de ASP.NET, vea la serie de blogs de Brad Wilson en [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Aunque la serie se escribió para ASP.NET MVC 2, el material sigue siendo válido para la versión actual de ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
