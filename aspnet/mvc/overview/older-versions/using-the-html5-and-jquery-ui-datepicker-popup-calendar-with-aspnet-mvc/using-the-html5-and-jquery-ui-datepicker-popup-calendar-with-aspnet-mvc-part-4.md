---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una máquina virtual de ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7ecd180b7608e82ea143575c6590574b92843dcf
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577501"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Uso de HTML5 y calendario emergente de la interfaz de usuario Datepicker de jQuery con ASP.NET MVC - parte 4
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, las plantillas de presentación y el calendario emergente de jQuery UI datepicker en una aplicación Web de ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Adición de una plantilla para editar las fechas

En esta sección creará una plantilla para editar las fechas que se aplicarán cuando ASP.NET MVC muestra la interfaz de usuario para editar las propiedades del modelo que se marcan con el **fecha** enumeración de la [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo. La plantilla representará solo la fecha; no se mostrará el tiempo. En la plantilla usará el [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendario emergente para proporcionar una forma de editar las fechas.

Para comenzar, abra el *Movie.cs* y agréguele el [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo con el **fecha** enumeración para el `ReleaseDate` propiedad, como se muestra en el código siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Este código hace que el `ReleaseDate` que se muestre sin el tiempo en ambos mostrar las plantillas y editar plantillas de campo. Si la aplicación contiene un *date.cshtml* plantilla en el *Views\Shared\EditorTemplates* carpeta o en el *Views\Movies\EditorTemplates* carpeta, esa plantilla se usará para representar cualquier `DateTime` propiedad durante la edición. En caso contrario, el sistema de creación de plantillas ASP.NET integrado mostrará la propiedad como una fecha.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición para comprobar que el campo de entrada para la fecha de lanzamiento muestra solo la fecha.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

En **el Explorador de soluciones**, expanda el *vistas* carpeta, expanda la *Shared* carpeta y, a continuación, con el botón secundario el *Views\Shared\EditorTemplates* carpeta.

Haga clic en **agregar**y, a continuación, haga clic en **vista**. El **agregar vista** se muestra el cuadro de diálogo.

En el **nombre de la vista** , escriba &quot;fecha&quot;.

Seleccione el **crear como una vista parcial** casilla de verificación. Asegúrese de que el **utiliza una página de diseño o maestra** y **crear una vista fuertemente tipada** no están activadas las casillas de verificación.

Haga clic en **Agregar**. El *Views\Shared\EditorTemplates\Date.cshtml* se crea la plantilla.

Agregue el código siguiente a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La primera línea declara el modelo sea un `DateTime` tipo. Aunque no es necesario declarar el tipo de modelo de edición y mostrar las plantillas, es una práctica recomendada para que obtenga el tiempo de compilación del modelo que se pasan a la vista de la comprobación. (Otra ventaja es que, a continuación, obtendrá IntelliSense para el modelo en la vista en Visual Studio). Si no se ha declarado el tipo de modelo, ASP.NET MVC considera un [dinámica](https://msdn.microsoft.com/library/dd264741.aspx) tipo y no hay ningún tiempo de compilación comprobación de tipos. Si el modelo se declara un `DateTime` tipo, se convierte en establecimiento inflexible.

La segunda línea es simplemente literal marcado HTML que muestra &quot;utilizando la plantilla de fecha&quot; antes de un campo de fecha. Usará esta línea temporalmente para comprobar que se está usando esta plantilla de fecha.

La siguiente línea es un [Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) auxiliar que presenta un `input` campo que es un cuadro de texto. El tercer parámetro para la aplicación auxiliar usa un tipo anónimo para definir la clase para el cuadro de texto `datefield` y el tipo a `date`. (Porque `class` está reservado en C#, deberá usar el `@` carácter para escapar el `class` atributo en el analizador de C#.)

El `date` tipo es un tipo de entrada de HTML5 que permite que los exploradores compatibles con HTML5 representar un control de calendario de HTML5. Más adelante agregará código de JavaScript para datepicker de jQuery para enlazar el `Html.TextBox` elemento mediante el `datefield` clase.

Presione CTRL+F5 para ejecutar la aplicación. Puede comprobar que la `ReleaseDate` propiedad en la vista de edición es mediante la plantilla de edición porque muestra la plantilla &quot;utilizando la plantilla de fecha&quot; justo antes del `ReleaseDate` , cuadro de entrada de texto como se muestra en esta imagen:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

En el explorador, vea el origen de la página. (Por ejemplo, haga clic en la página y seleccione **ver código fuente**.) El siguiente ejemplo muestra parte del marcado de la página, que ilustra el `class` y `type` atributos en el HTML representado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Vuelva a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla y quitar el &quot;utilizando la plantilla de fecha&quot; marcado. Ahora la plantilla completa tiene este aspecto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Adición de un calendario emergente de la interfaz de usuario Datepicker de jQuery mediante NuGet

En esta sección agregará el [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente para la plantilla de edición de la fecha. El [jQuery UI](http://jqueryui.com/) biblioteca proporciona compatibilidad para animación, efectos y widgets personalizables avanzados. Se basa en la biblioteca JavaScript jQuery. El calendario emergente datepicker hace fácil y natural a escribir las fechas mediante un calendario en lugar de escribir una cadena. El calendario emergente también limita a los usuarios a fechas legales: entrada de texto normal para una fecha permitiría escribir algo parecido a `2/33/1999` (febrero 33rd, 1999), pero la [datepicker de jQuery UI](http://jqueryui.com/demos/datepicker/) calendario emergente no permitirá que.

En primer lugar, debe instalar las bibliotecas de interfaz de usuario de jQuery. Para ello, deberá usar NuGet, que es un administrador de paquetes que se incluye en las versiones de SP1 de Visual Studio 2010 y Visual Web Developer.

En Visual Web Developer, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, seleccione **administrar paquetes de NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: Si el **herramientas** menú no se muestra el **Administrador de paquetes de biblioteca** de comandos, deberá instalar NuGet, siga las instrucciones de la [instalación de NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) página de el sitio Web de NuGet.   
  
Si usa Visual Studio en lugar de Visual Web Developer, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, seleccione **Agregar referencia de paquetes de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

En el **MVCMovie - administrar paquetes de NuGet** cuadro de diálogo, haga clic en el **Online** pestaña a la izquierda y, a continuación, escriba &quot;jQuery.UI&quot; en el cuadro de búsqueda. Seleccione j **WIdgets de interfaz de usuario de consulta: Datepicker**, a continuación, seleccione el **instalar** botón.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet agrega estas versiones de depuración y minimizados de jQuery UI Core y el selector de fecha de la interfaz de usuario de jQuery al proyecto:

- *jQuery.UI.Core.js*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.js*
- *jQuery.UI.DatePicker.min.js*

Nota: Las versiones de depuración (los archivos sin la *. min.js* extensión) son útiles para la depuración, pero en un sitio de producción, también debe incluir solo las versiones reducidas.

Para utilizar realmente el selector de fecha de jQuery, deberá crear una secuencia de comandos de jQuery se enlazará el widget de calendario a la plantilla de edición. En **el Explorador de soluciones**, haga clic en el *Scripts* carpeta y seleccione **agregar**, a continuación, **nuevo elemento**y, a continuación, **JScript Archivo**. Nombre del archivo *DatePickerReady.js*.

Agregue el código siguiente a la *DatePickerReady.js* archivo:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Si no está familiarizado con jQuery, presentamos una breve explicación de lo que hace: la primera línea es el &quot;jQuery listo&quot; función, que se llama cuando se han cargado todos los elementos DOM en una página. La segunda línea selecciona todos los elementos de DOM que tienen el nombre de clase `datefield`, a continuación, invoca el `datepicker` función para cada uno de ellos. (Recuerde que ha agregado el `datefield` clase a la *Views\Shared\EditorTemplates\Date.cshtml* plantilla anteriormente en este tutorial.)

A continuación, abra el *Views\Shared\\_Layout.cshtml* archivo. Es preciso agregar referencias a los archivos siguientes, que son necesarios para que pueda usar el selector de fecha:

- *Content/Themes/base/jQuery.UI.Core.CSS*
- *Content/Themes/base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/base/jQuery.UI.Theme.CSS*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.min.js*
- *DatePickerReady.js*

El ejemplo siguiente se muestra el código real que debe agregar en la parte inferior de la `head` elemento en el *Views\Shared\\_Layout.cshtml* archivo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

La completa `head` sección se muestra aquí:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

El [auxiliar contenido URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) método convierte la ruta de acceso de recursos en una ruta de acceso absoluta. Debe usar `@URL.Content` referencia correctamente a estos recursos cuando se ejecuta la aplicación en IIS.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición, a continuación, coloque el punto de inserción en el **ReleaseDate** campo. Se muestra el calendario emergente de la interfaz de usuario de jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Al igual que la mayoría de los controles de jQuery, datepicker permite personalizarla ampliamente. Para obtener información, consulte [personalización Visual: diseñar un tema de la interfaz de usuario de jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) en el [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) sitio.

### <a name="supporting-the-html5-date-input-control"></a>Compatibilidad con el Control de entrada de fecha de HTML5

Como los exploradores más compatibilidad con HTML5, querrá usar la HTML5 nativo de entrada, como el `date` elemento de entrada y no usan el calendario de la interfaz de usuario de jQuery. Puede agregar lógica a la aplicación para usar automáticamente controles HTML5 si el explorador admite. Para ello, reemplace el contenido de la *DatePickerReady.js* archivo por lo siguiente:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La primera línea de este script usa Modernizr para comprobar que se admite la entrada de fecha de HTML5. Si no se admite, el selector de fecha de la interfaz de usuario de jQuery está enlazado en su lugar. ([Modernizr](http://www.modernizr.com/docs/) es una biblioteca de JavaScript de código abierto que detecta la disponibilidad de implementaciones nativas de HTML5 y CSS3. Modernizr se incluye en los nuevos proyectos de ASP.NET MVC que cree.)

Después de realizar este cambio, puede probarla mediante un explorador compatible con HTML5, como 11 Opera. Ejecute la aplicación mediante un explorador compatible con HTML5 y editar una entrada de la película. El control de fecha de HTML5 se utiliza en lugar del calendario emergente de la interfaz de usuario de jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Dado que las nuevas versiones de los exploradores están implementando HTML5 incrementalmente, un buen enfoque por ahora es agregar código a su sitio Web que dé cabida a una amplia variedad de compatibilidad con HTML5. Por ejemplo, un más sólido *DatePickerReady.js* script se muestra a continuación que permite los exploradores de soporte técnico de sitio que admiten solo parcialmente el control de fecha HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Esta secuencia de comandos selecciona HTML5 `input` elementos de tipo `date` que no son totalmente compatibles con el control de fecha HTML5. Para esos elementos, enlaza el calendario emergente de la interfaz de usuario de jQuery y, a continuación, cambia el `type` de atributo de `date` a `text`. Cambiando el `type` de atributo de `date` a `text`, se ha eliminado la compatibilidad parcial con la fecha HTML5. Más robustas *DatePickerReady.js* script puede encontrarse en [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Agregar fechas que aceptan valores NULL a las plantillas

Si usa una de las plantillas existentes de fecha y pase una fecha null, obtendrá un error en tiempo de ejecución. Para que las plantillas de fecha más sólido, va a cambiar para controlar valores null. Para admitir las fechas que aceptan valores NULL, cambie el código en el *Views\Shared\DisplayTemplates\DateTime.cshtml* al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

El código devuelve una cadena vacía cuando el modelo es **null**.

Cambie el código en el *Views\Shared\EditorTemplates\Date.cshtml* archivo a la siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Cuando este código se ejecuta, si el modelo no es null, el modelo `DateTime` se usa el valor. Si el modelo es null, se usa en su lugar la fecha actual.

### <a name="wrapup"></a>Wrapup

Este tutorial trata los aspectos básicos de las aplicaciones auxiliares con plantilla de ASP.NET y muestra cómo usar el calendario de emergente datepicker de jQuery UI en una aplicación ASP.NET MVC. Para obtener más información, pruebe estos recursos:

- Para obtener información sobre la localización, consulte el blog de Rajeesh [JQueryUI Datepicker en ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obtener información acerca de la UI de jQuery, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obtener información sobre cómo localizar el control datepicker, consulte [Datepicker/interfaz de usuario/localización](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obtener más información acerca de las plantillas de ASP.NET MVC, vea la serie de blogs de Brad Wilson en [plantillas de ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Aunque la serie se escribió para ASP.NET MVC 2, el material todavía se aplica a la versión actual de ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
