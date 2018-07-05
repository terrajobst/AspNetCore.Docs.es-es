---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: El modelo de página 2.0 de ASP.NET | Microsoft Docs
author: microsoft
description: En ASP.NET 1.x, los desarrolladores tenían una elección entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente se pueden implementar mediante cualquier el attr Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379892"
---
<a name="the-aspnet-20-page-model"></a>El modelo de página 2.0 de ASP.NET
====================
por [Microsoft](https://github.com/microsoft)

> En ASP.NET 1.x, los desarrolladores tenían una elección entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente podría implementarse mediante el atributo Src o el atributo de código subyacente de la @Page directiva. En ASP.NET 2.0, los desarrolladores tienen todavía una elección entre el código en línea y código subyacente, pero ha habido importantes mejoras en el modelo de código subyacente.


En ASP.NET 1.x, los desarrolladores tenían una elección entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente podría implementarse mediante el atributo Src o el atributo de código subyacente de la @Page directiva. En ASP.NET 2.0, los desarrolladores tienen todavía una elección entre el código en línea y código subyacente, pero ha habido importantes mejoras en el modelo de código subyacente.

## <a name="improvements-in-the-code-behind-model"></a>Mejoras en el modelo de código subyacente

Para comprender totalmente los cambios en el modelo de código subyacente en ASP.NET 2.0, se recomienda revisar rápidamente el modelo tal como existía en ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>El modelo de código subyacente en ASP.NET 1.x

En ASP.NET 1.x, consistió en el modelo de código subyacente de un archivo ASPX (formularios Web Forms) y un archivo de código subyacente que contiene el código de programación. Los dos archivos estaban conectados mediante el @Page la directiva en el archivo ASPX. Cada control en la página ASPX tenía una declaración correspondiente en el archivo de código subyacente como una variable de instancia. El archivo de código subyacente también contiene código para el enlace de eventos y genera el código necesario para el Diseñador de Visual Studio. Este modelo funcionaba bastante bien, pero dado que todos los elementos en la página ASPX ASP.NET requería código correspondiente en el archivo de código subyacente, se ha producido ninguna separación de código y el contenido es true. Por ejemplo, si un diseñador, agrega un nuevo control de servidor a un archivo ASPX fuera del IDE de Visual Studio, la aplicación se interrumpiría debido a la ausencia de una declaración para ese control en el archivo de código subyacente.

## <a name="the-code-behind-model-in-aspnet-20"></a>El modelo de código subyacente en ASP.NET 2.0

ASP.NET 2.0 mejora en gran medida este modelo. En ASP.NET 2.0, código subyacente se implementa mediante el nuevo *clases parciales* incluido en ASP.NET 2.0. La clase de código subyacente de ASP.NET 2.0 es definen como una clase parcial, lo que significa que contiene solo una parte de la definición de clase. La parte restante de la definición de clase se genera dinámicamente mediante la página ASPX en tiempo de ejecución o cuando se precompila el sitio Web de ASP.NET 2.0. El vínculo entre el archivo de código subyacente y la página ASPX sigue establecido mediante la directiva @ Page. Sin embargo, en lugar de un atributo de código subyacente o Src, ASP.NET 2.0 ahora usa el atributo CodeFile. El atributo Inherits también se utiliza para especificar el nombre de clase para la página.

Una directiva @ Page típica podría tener este aspecto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definición de clase típica en un archivo de código subyacente de ASP.NET 2.0 podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# y Visual Basic son los únicos lenguajes administrados que son compatibles con las clases parciales. Por lo tanto, los desarrolladores que usan J# no podrá utilizar el modelo de código subyacente en ASP.NET 2.0.


El nuevo modelo mejora el modelo de código subyacente porque los desarrolladores ahora tendrá los archivos de código que contienen sólo el código que han creado. También proporciona una separación de código y contenido true porque no hay ninguna declaración de variable de instancia en el archivo de código subyacente.

> [!NOTE]
> Dado que la clase parcial para la página ASPX es donde realiza el enlace de eventos, los desarrolladores de Visual Basic pueden conseguir un aumento de ligeramente el rendimiento mediante el uso de la palabra clave Handles en el código subyacente para enlazar eventos. C# no tiene ninguna palabra clave equivalente.


## <a name="new--page-directive-attributes"></a>Nuevos atributos de la directiva @ Page

ASP.NET 2.0 agrega muchos atributos nuevos a la directiva @ Page. Los atributos siguientes son nuevos en ASP.NET 2.0.

## <a name="async"></a>Async

El atributo Async permite configurar la página que se ejecutará de forma asincrónica. Acá abarcaremos páginas asincrónicas más adelante en este módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Especifica el tiempo de espera para las páginas asincrónicas. El valor predeterminado es 45 segundos.

## <a name="codefile"></a>CodeFile

El atributo CodeFile es el reemplazo para el atributo de código subyacente en Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

El atributo CodeFileBaseClass se usa en los casos donde desee varias páginas pueden derivar una clase base. Debido a la implementación de clases parciales en ASP.NET, sin este atributo, una clase base que utiliza campos comunes compartidos para hacer referencia a los controles declarados en una página ASPX no funcionará correctamente porque ASP. El motor de compilación de redes crea automáticamente nuevos miembros en función de los controles en la página. Por lo tanto, si desea que una clase base común para dos o más páginas de ASP.NET, necesitará definir especifica la clase base en el atributo CodeFileBaseClass y, a continuación, se deriva cada clase de páginas de esa clase base. El atributo CodeFile también es necesario cuando se utiliza este atributo.

## <a name="compilationmode"></a>CompilationMode

Este atributo permite establecer la propiedad CompilationMode de la página ASPX. La propiedad CompilationMode es una enumeración que contiene los valores **siempre**, **automática**, y **nunca**. El valor predeterminado es **siempre**. El **automática** configuración impedirá que ASP.NET compilar dinámicamente la página si es posible. Exclusión de las páginas de la compilación dinámica aumenta el rendimiento. Sin embargo, si una página que se excluye contiene código que se debe compilar, se iniciará un error al examinar la página.

## <a name="enableeventvalidation"></a>EnableEventValidation

Este atributo especifica si se validan los eventos de postback y devolución de llamada. Cuando esta opción está habilitada, se comprueban los argumentos de devolución de datos o eventos de devolución de llamada para asegurarse de que se originaron desde el control de servidor que representaba originalmente.

## <a name="enabletheming"></a>EnableTheming

Este atributo especifica si se utilizan temas ASP.NET en una página. El valor predeterminado es **false**. Se tratan los temas de ASP.NET en [módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Este atributo especifica si se debe agregar pragmas de línea durante la compilación. Pragmas de línea son opciones que se usa en depuradores para marcar las secciones específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Este atributo especifica si se inserta JavaScript en la página con el fin de mantener la posición de desplazamiento entre las devoluciones de datos. Este atributo es **false** de forma predeterminada.

Cuando este atributo es **true**, ASP.NET agregará un &lt;script&gt; bloque en la devolución de datos que tiene este aspecto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Tenga en cuenta que para este bloque de script src WebResource.axd. Este recurso no es una ruta de acceso física. Cuando se solicita esta secuencia de comandos, ASP.NET compila dinámicamente la secuencia de comandos.

### <a name="masterpagefile"></a>MasterPageFile

Este atributo especifica el archivo de página maestra para la página actual. La ruta de acceso puede ser relativa o absoluta. Se tratan las páginas maestras en [módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Este atributo permite reemplazar las propiedades de apariencia de interfaz de usuario definidas por un tema de ASP.NET 2.0. Los temas se tratan en [módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Especifica el tema de la página. Si no se especifica un valor para el atributo StyleSheetTheme, el atributo de tema invalida todos los estilos aplicados a controles en la página.

## <a name="title"></a>Title

Establece el título de la página. El valor especificado aquí aparecerán en la &lt;título&gt; elemento de la página representada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Establece el valor de la enumeración ViewStateEncryptionMode. Los valores disponibles son **siempre**, **automática**, y **nunca**. El valor predeterminado es **automática**. Cuando este atributo se establece en un valor de **automática**, viewstate se cifra es lo solicite un control mediante una llamada a la **RegisterRequiresViewStateEncryption** método.

## <a name="setting-public-property-values-via-the--page-directive"></a>Establecer valores de propiedad pública a través de la directiva @ Page

Otra nueva capacidad de la directiva @ Page en ASP.NET 2.0 es la capacidad para establecer el valor inicial de las propiedades públicas de una clase base. Supongamos, por ejemplo, que tiene una propiedad pública denominada **SomeText** en su clase base y d al igual que lo que se inicialice con **Hello** cuando se carga una página. Para ello, basta con establecer el valor de la directiva @ Page de este modo:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

El **SomeText** atributo de la directiva @ Page establece el valor inicial de la propiedad SomeText en la clase base para *Hello!*. El vídeo siguiente es un tutorial de cómo configurar el valor inicial de una propiedad pública de una clase base mediante la directiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nuevas propiedades públicas de la clase de página

Las propiedades públicas siguientes son nuevas en ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Devuelve la ruta de acceso relativa a la aplicación a la página o control. Por ejemplo, para una página que se encuentra en http://app/folder/page.aspx, la propiedad devuelve ~ / carpeta /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Devuelve la ruta de acceso del directorio virtual relativo a la página o control. Por ejemplo, para una página que se encuentra en http://app/folder/page.aspx, la propiedad devuelve ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtiene o establece el tiempo de espera utilizado para el control de página asincrónica. (Las páginas asincrónicas se tratarán más adelante en este módulo).

## <a name="clientquerystring"></a>ClientQueryString

Una propiedad de solo lectura que devuelve la parte de la cadena de consulta de la dirección URL solicitada. Este valor es la dirección URL codificada. Puede usar el método UrlDecode de la clase HttpServerUtility descodificarlo.

## <a name="clientscript"></a>ClientScript

Esta propiedad devuelve un objeto ClientScriptManager que puede usarse para administrar la emisión ASP.NETs de script de cliente. (La clase ClientScriptManager se trata más adelante en este módulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Esta propiedad controla si está habilitada la validación de eventos para eventos de postback y devolución de llamada. Cuando se habilita, se comprueban los argumentos de devolución de datos o eventos de devolución de llamada para asegurarse de que se originaron desde el control de servidor que representaba originalmente.

## <a name="enabletheming"></a>EnableTheming

Esta propiedad obtiene o establece un valor booleano que especifica si se aplica un tema de ASP.NET 2.0 a la página.

## <a name="form"></a>Form

Esta propiedad devuelve el formulario HTML en la página ASPX como un objeto HtmlForm.

## <a name="header"></a>Header

Esta propiedad devuelve una referencia a un objeto HtmlHead que contiene el encabezado de página. Puede usar el objeto HtmlHead devuelto para obtener o establecer las hojas de estilos, etiquetas Meta, etcetera.

## <a name="idseparator"></a>IdSeparator

Esta propiedad de solo lectura Obtiene el carácter que se usa para separar los identificadores de control cuando ASP.NET es crear un identificador único para los controles en una página. No debe usarse directamente desde el código.

## <a name="isasync"></a>IsAsync

Esta propiedad permite que las páginas asincrónicas. Páginas asincrónicas se tratan más adelante en este módulo.

## <a name="iscallback"></a>IsCallback

Esta propiedad de solo lectura devuelve **true** si la página es el resultado de una devolución de llamada. Devolución de llamada se trata más adelante en este módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Esta propiedad de solo lectura devuelve **true** si la página forma parte de un postback entre páginas. Las devoluciones entre páginas se tratan más adelante en este módulo.

## <a name="items"></a>Elementos

Devuelve una referencia a una instancia de IDictionary que contiene todos los objetos almacenados en el contexto de las páginas. Puede agregar elementos a este objeto IDictionary y estarán disponibles durante la vigencia del contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Esta propiedad controla si ASP.NET emite JavaScript que mantiene que las páginas de la posición en el Explorador de desplazamiento después de que se produce un postback. (Los detalles de esta propiedad se trataron anteriormente en este módulo).

## <a name="master"></a>Master

Esta propiedad de solo lectura devuelve una referencia a la instancia de la página maestra para una página a la que se ha aplicado una página maestra.

## <a name="masterpagefile"></a>MasterPageFile

Obtiene o establece el nombre de archivo de página maestra para la página. Esta propiedad solo puede establecerse en el método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propiedad obtiene o establece la longitud máxima para el estado de las páginas en bytes. Si la propiedad se establece en un número positivo, el estado de vista de páginas se dividirse en varios campos ocultos de forma que no supere el número de bytes especificado. Si la propiedad es un número negativo, el estado de vista no se dividirá en fragmentos.

## <a name="pageadapter"></a>PageAdapter

Devuelve una referencia al objeto PageAdapter que modifica la página del explorador solicitante.

## <a name="previouspage"></a>PreviousPage

Devuelve una referencia a la página anterior en caso de un Server.Transfer o un postback entre páginas.

## <a name="skinid"></a>SkinID

Especifica la máscara de ASP.NET 2.0 para aplicar a la página.

## <a name="stylesheettheme"></a>StyleSheetTheme

Esta propiedad obtiene o establece la hoja de estilos que se aplica a una página.

## <a name="templatecontrol"></a>TemplateControl

Devuelve una referencia al control de contenedor para la página.

## <a name="theme"></a>Tema

Obtiene o establece el nombre del tema de ASP.NET 2.0 aplicado a la página. Este valor debe establecerse antes del método PreInit.

## <a name="title"></a>Title

Esta propiedad obtiene o establece el título de la página obtenida con el encabezado de páginas.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtiene o establece el ViewStateEncryptionMode de la página. Ver una explicación detallada de esta propiedad anteriormente en este módulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuevas propiedades protegidas de la clase de página

Los siguientes son las nuevas propiedades protegidas de la clase de página en ASP.NET 2.0.

## <a name="adapter"></a>Adaptador

Devuelve una referencia a la ControlAdapter que representa la página en el dispositivo que lo ha solicitado.

## <a name="asyncmode"></a>AsyncMode

Esta propiedad indica si se procesa la página de forma asincrónica. Está pensado para su uso por el tiempo de ejecución y no directamente en el código.

## <a name="clientidseparator"></a>ClientIDSeparator

Esta propiedad devuelve el carácter utilizado como separador al crear los identificadores de cliente único para los controles. Está pensado para su uso por el tiempo de ejecución y no directamente en el código.

## <a name="pagestatepersister"></a>PageStatePersister

Esta propiedad devuelve el objeto PageStatePersister para la página. Esta propiedad se utiliza principalmente los programadores de controles ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Esta propiedad devuelve un suffic único que se anexa a la ruta de acceso de archivo para almacenar en caché de los exploradores. El valor predeterminado es \_ \_ufps = y un número de 6 dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Nuevos métodos públicos de la clase de página

Los métodos públicos siguientes son nuevos en la clase Page en ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Este método registra a los delegados de controlador de eventos de ejecución de la página asincrónica. Páginas asincrónicas se tratan más adelante en este módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Se aplica a las propiedades en una hoja de estilos de las páginas a la página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Los seres este método una tarea asincrónica.

### <a name="getvalidators"></a>GetValidators

Devuelve una colección de validadores para el grupo de validaciones especificado o el grupo de validaciones predeterminado si no se especifica ninguno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Este método registra una nueva tarea asincrónica. Páginas asincrónicas se tratan más adelante en este módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Este método indica a ASP.NET que se debe conservar el estado de control de páginas.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Este método indica a ASP.NET que viewstate páginas requiere cifrado.

## <a name="resolveclienturl"></a>ResolveClientUrl

Devuelve una dirección URL relativa que se puede usar para las solicitudes de cliente para imágenes, etcetera.

## <a name="setfocus"></a>SetFocus

Este método establecerá el foco al control que se especifica cuando se carga la página por primera vez.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Este método anulará el control que se pasa a él que ya no requiere una persistencia de estado de control.

## <a name="changes-to-the-page-lifecycle"></a>Cambios realizados en el ciclo de vida de página

El ciclo de vida de página en ASP.NET 2.0 no ha cambiado considerablemente, pero hay algunos métodos nuevos que debe tener en cuenta. A continuación se describe el ciclo de vida de la página de ASP.NET 2.0.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novedad de ASP.NET 2.0)

El evento PreInit es la primera etapa del ciclo de vida que puede tener acceso un desarrollador. La adición de este evento permite cambiar los temas de ASP.NET 2.0, las páginas maestras mediante programación, obtener acceso a las propiedades de un perfil de ASP.NET 2.0, etcetera. Si se encuentra en un estado de devolución de datos, es importante comprender que Viewstate no se han aplicado a los controles en este punto del ciclo de vida. Por lo tanto, si un desarrollador cambia una propiedad de un control en esta fase, es probable que se sobrescribirá más adelante en el ciclo de vida de las páginas.

## <a name="init"></a>Init

El evento Init no ha cambiado desde ASP.NET 1.x. Esto es donde se desea leer o inicializar las propiedades de controles en la página. En este escenario, las páginas maestras, temas, etc. ya se aplican a la página.

## <a name="initcomplete-new-in-20"></a>InitComplete (nuevo en 2.0)

El evento InitComplete se llama al final de la fase de inicialización de las páginas. En este momento en el ciclo de vida, puede acceder a los controles en la página, pero aún no se ha rellenado su estado.

## <a name="preload-new-in-20"></a>PreLoad (nuevo en 2.0)

Se llama a este evento se haya aplicado todos los datos de postback y justo antes de la página\_carga.

## <a name="load"></a>Load

El evento de carga no ha cambiado desde ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nuevo en 2.0)

El evento LoadComplete es el último en la fase de carga de páginas. En esta fase, todos los datos de postback y viewstate se ha aplicado a la página.

## <a name="prerender"></a>PreRender

Si lo desea para que viewstate para mantenerse de forma adecuada para los controles que se agregan a la página de forma dinámica, el evento PreRender es la última oportunidad para agregarlos.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nuevo en 2.0)

En la fase de PreRenderComplete, todos los controles se han agregado a la página y la página está lista para su presentación. El evento de PreRenderComplete es el último evento que se genera antes de que se guarda el estado de vista de páginas.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nuevo en 2.0)

El evento SaveStateComplete se llama inmediatamente después de que todos los Estados de viewstate y control de página se ha guardado. Se trata del último evento antes de la página se representa realmente en el explorador.

## <a name="render"></a>Representar

El método Render no ha cambiado desde ASP.NET 1.x. Aquí es donde se ha inicializado el HtmlTextWriter y la página se representa en el explorador.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback entre páginas en ASP.NET 2.0

En ASP.NET 1.x, las devoluciones de datos eran necesarios para publicar en la misma página. No se permiten las devoluciones entre páginas. ASP.NET 2.0 agrega la capacidad para registrar una página distinta a través de la interfaz IButtonControl. Cualquier control que implementa la nueva interfaz IButtonControl (Button, LinkButton e ImageButton además de los controles personalizados de terceros) puede aprovechar esta nueva funcionalidad a través del uso del atributo PostBackUrl. El código siguiente muestra un control de botón que se envía a una segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Cuando se devuelve la página, la página que inicia la devolución de datos es accesible a través de la propiedad PreviousPage en la segunda página. Esta funcionalidad se implementa a través de la nueva WebForm\_DoPostBackWithOptions función de cliente que ASP.NET 2.0 presenta a la página cuando se envía un control a una página diferente. Esta función de JavaScript se proporciona con el nuevo controlador WebResource.axd que emite la secuencia de comandos al cliente.

El vídeo siguiente es un tutorial de un postback entre páginas.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Obtener más detalles sobre las devoluciones entre páginas

### <a name="viewstate"></a>ViewState

Es posible que ha solicitado por sí mismo ya sobre lo que ocurre en el estado de vista de la primera página en un escenario de devolución de datos entre páginas. Después de todo, cualquier control que no implementa IPostBackDataHandler conservará su estado a través de viewstate, por lo que para tener acceso a las propiedades de ese control en la segunda página de un postback entre páginas, debe tener acceso a la propiedad viewstate de la página. ASP.NET 2.0 se encarga de este escenario con un nuevo campo oculto en la segunda página denominada \_ \_PREVIOUSPAGE. El \_ \_campo de formulario PREVIOUSPAGE contiene el estado de vista para la primera página para que puedan tener acceso a las propiedades de todos los controles en la segunda página.

### <a name="circumventing-findcontrol"></a>Sortear FindControl

En el tutorial en vídeo de un postback entre páginas, usé el método FindControl para obtener una referencia al control de cuadro de texto en la primera página. Ese método funciona bien para ese propósito, pero FindControl es costoso y requiere escribir código adicional. Afortunadamente, ASP.NET 2.0 proporciona una alternativa a FindControl para este propósito que funcionará en muchos escenarios. PreviousPageType (directiva) le permite tener una referencia fuertemente tipada a la página anterior mediante el TypeName o VirtualPath (atributo). El atributo TypeName permite especificar el tipo de la página anterior, mientras que el atributo VirtualPath permite hacer referencia a la página anterior mediante una ruta de acceso virtual. Después de haber establecido PreviousPageType (directiva), a continuación, debe exponer los controles, etc. a la que desea permitir el acceso mediante las propiedades públicas.

## <a name="lab-1-cross-page-postback"></a>Postback entre páginas de laboratorio 1

En este laboratorio, creará una aplicación que usa la nueva funcionalidad de postback entre páginas de ASP.NET 2.0.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web de ASP.NET.
2. Agregue un nuevo Webform llamado page2.aspx.
3. Abra Default.aspx en la vista Diseño y agregar un control de botón y un control de cuadro de texto. 

    1. Dar el control de botón a un identificador de **SubmitButton** y el cuadro de texto Id. de control **UserName**.
    2. Establezca la propiedad PostBackUrl del botón a page2.aspx.
4. Abra page2.aspx en la vista del origen.
5. Agregue una @ PreviousPageType (directiva) como se muestra a continuación:
6. Agregue el código siguiente a la página\_carga de código subyacente del page2.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compile el proyecto haciendo clic en la compilación en el menú compilar.
8. Agregue el código siguiente al código subyacente para Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Cambiar la página\_carga en page2.aspx al siguiente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile el proyecto.
11. Ejecute el proyecto.
12. Escriba su nombre en el cuadro de texto y haga clic en el botón.
13. ¿Qué es el resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas asincrónicas en ASP.NET 2.0

Muchos problemas de contención en ASP.NET son causados por la latencia de llamadas externas (por ejemplo, llamadas de base de datos o servicio Web), la latencia de E/S de archivo, etcetera. Cuando se realiza una solicitud en una aplicación ASP.NET, ASP.NET usa uno de sus subprocesos de trabajo para atender la solicitud. Esa solicitud posee ese subproceso hasta que la solicitud se completa y se ha enviado la respuesta. ASP.NET 2.0 busca resolver problemas de latencia con estos tipos de problemas mediante la adición de la capacidad de ejecutar las páginas de forma asincrónica. Esto significa que un subproceso de trabajo puede iniciar la solicitud y, a continuación, entregar de ejecución adicionales a otro subproceso, con lo que se vuelven rápidamente al grupo de subprocesos disponibles. Cuando haya finalizado la E/S de archivos, la llamada de la base de datos, etc., se obtiene un nuevo subproceso del grupo de subprocesos para terminar la solicitud.

Es el primer paso en la creación de una página ejecutar de forma asincrónica establecer el **Async** como atributo de la directiva de página:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Este atributo indica a ASP.NET que implemente la IHttpAsyncHandler para la página.

El siguiente paso es llamar al método AddOnPreRenderCompleteAsync en un momento en el ciclo de vida de la página anterior PreRender. (Normalmente, se llama a este método en la página\_carga.) El método AddOnPreRenderCompleteAsync toma dos parámetros; un BeginEventHandler y un EndEventHandler. BeginEventHandler devuelve IAsyncResult que, a continuación, se pasa como parámetro a la EndEventHandler.

El siguiente vídeo es un tutorial de una solicitud de página asincrónica.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Una página asincrónica no se representará en el explorador hasta que se ha completado la EndEventHandler. Sin duda, pero surgirán algunos desarrolladores solicitudes asincrónicas es similar a las devoluciones de llamada asincrónica. Es importante tener en cuenta que no se encuentran. La ventaja para las solicitudes asincrónicas es que se puede devolver el primer subproceso de trabajo al grupo de subprocesos para atender nuevas solicitudes, lo que reduce la contención debido a que están enlazados de E/S, etcetera.


## <a name="script-callbacks-in-aspnet-20"></a>Devoluciones de llamada de secuencia de comandos en ASP.NET 2.0

Los desarrolladores Web siempre han buscado formas de evitar el parpadeo asociados con una devolución de llamada. En ASP.NET 1.x, SmartNavigation era el método más común para evitar el parpadeo, pero SmartNavigation causaba problemas para algunos desarrolladores debido a la complejidad de su implementación en el cliente. ASP.NET 2.0 soluciona este problema con las devoluciones de llamada de secuencia de comandos. Las devoluciones de llamada de secuencia de comandos usan XMLHttp para realizar solicitudes en el servidor Web a través de JavaScript. La solicitud con XMLHttp devuelve datos XML que, a continuación, se pueden manipular a través DOM. del explorador El usuario está oculta XMLHttp código con el nuevo controlador WebResource.axd.

Hay varios pasos que son necesarios para configurar una devolución de llamada de secuencia de comandos en ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Paso 1: Implementar la interfaz ICallbackEventHandler

En orden para que ASP.NET pueda reconocer la página como participar en una devolución de llamada de secuencia de comandos, debe implementar la interfaz ICallbackEventHandler. Puede hacerlo en el archivo de código subyacente de este modo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

También puede hacer esto utilizando similares de la directiva @ Implements así:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente, usaría la directiva @ Implements cuando se usa código ASP.NET en línea.

## <a name="step-2--call-getcallbackeventreference"></a>Paso 2: Llamada GetCallbackEventReference

Como se mencionó anteriormente, la llamada de XMLHttp se encapsula en el controlador WebResource.axd. Cuando se procesa la página, ASP.NET agregará una llamada a WebForm\_DoCallback, un script de cliente que se proporciona a través de WebResource.axd. Los formularios Web Forms\_DoCallback función reemplaza la \_ \_doPostBack función de devolución de llamada. Recuerde que \_ \_doPostBack mediante programación envía el formulario en la página. En un escenario de devolución de llamada, desea evitar que una devolución de datos, por lo que \_ \_doPostBack no será suficiente.

> [!NOTE]
> \_\_doPostBack aún se representa en la página en un escenario de devolución de llamada del script de cliente. Sin embargo, no se utiliza para la devolución de llamada.


Los argumentos para los formularios Web Forms\_DoCallback (función) del lado cliente se proporcionan a través de la función de servidor GetCallbackEventReference que normalmente se llama en la página\_carga. Una llamada a GetCallbackEventReference típica podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> En este caso, cm es una instancia de ClientScriptManager. La clase ClientScriptManager se tratarán más adelante en este módulo.


Hay varias versiones sobrecargadas de GetCallbackEventReference. En este caso, los argumentos son como sigue:

`this`

Una referencia al control donde se llama GetCallbackEventReference. En este caso, es la propia página.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argumento de cadena que se pasa desde el código del lado cliente para el evento de servidor. En este caso, Im, pasando el valor de una lista desplegable denominada ddlCompany.

`ShowCompanyName`

El nombre de la función del lado cliente que acepte el valor devuelto (como cadena) desde el evento de devolución de llamada del lado servidor. Esta función solo se llamará cuando la devolución de llamada del lado servidor se realiza correctamente. Por lo tanto, en aras de la solidez, por lo general se recomienda usar la versión sobrecargada de GetCallbackEventReference que toma un argumento de cadena adicional que especifica el nombre de una función del lado cliente para ejecutar si se produce un error.

`null`

Una cadena que representa una función del lado cliente que inicia antes de que la devolución de llamada al servidor. En este caso, no hay ninguna secuencia de este tipo, por lo que el argumento es null.

`true`

Valor booleano que especifica si se deben realizar de forma asincrónica la devolución de llamada.

La llamada a WebForm\_DoCallback en el cliente pasará estos argumentos. Por lo tanto, cuando esta página se representa en el cliente, ese código tendrá un aspecto de este modo:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Tenga en cuenta que la firma de la función en el cliente es un poco diferente. La función del lado cliente pasa 5 cadenas y un valor booleano. La cadena adicional (que es null en el ejemplo anterior) contiene la función del lado cliente que va a controlar los errores de la devolución de llamada del lado servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Paso 3: Enlazar el evento de Control del lado cliente

Tenga en cuenta que el valor devuelto de GetCallbackEventReference anterior se asignó a una variable de cadena. Esa cadena se usa para enlazar un evento del lado cliente para el control que inicia la devolución de llamada. En este ejemplo, la devolución de llamada se inicia mediante una lista desplegable en la página, así que voy a enlazar el *OnChange* eventos.

Para enlazar el evento del lado cliente, basta con agregar un controlador para el marcado del lado cliente como sigue:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Recuerde que *cbRef* es el valor devuelto de la llamada a GetCallbackEventReference. Contiene la llamada a WebForm\_DoCallback que se mostró anteriormente.

## <a name="step-4--register-the-client-side-script"></a>Paso 4: Registro de la secuencia de comandos del lado cliente

Recuerde que la llamada a GetCallbackEventReference especifica que un script de cliente denominado **ShowCompanyName** se ejecutaría si la devolución de llamada del lado servidor se realiza correctamente. Ese script debe agregarse a la página mediante una instancia de ClientScriptManager. (La clase ClientScriptManager será describen más adelante en este módulo). Hacerlo ese tipo:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Paso 5: Llamar a los métodos de la interfaz ICallbackEventHandler

El ICallbackEventHandler contiene dos métodos que se deben implementar en el código. Son **RaiseCallbackEvent** y **GetCallbackEvent**.

**RaiseCallbackEvent** toma una cadena como argumento y no devuelve nada. El argumento de cadena se pasa de la llamada del lado cliente a WebForm\_DoCallback. En este caso, ese valor es el *valor* atributo de la lista desplegable denominada ddlCompany. El código del lado servidor debe colocarse en el método RaiseCallbackEvent. Por ejemplo, si la devolución de llamada realiza una WebRequest contra un recurso externo, ese código debe colocarse en RaiseCallbackEvent.

**GetCallbackEvent** es responsable de procesar la devolución de la devolución de llamada al cliente. No toma ningún argumento y devuelve una cadena. La cadena que devuelve se pasará como un argumento a la función del lado cliente, en este caso *ShowCompanyName*.

Una vez completados los pasos anteriores, está listo para realizar una devolución de llamada de secuencia de comandos en ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/callback1.wmv)


Las devoluciones de llamada de secuencia de comandos en ASP.NET son compatibles con cualquier explorador que admita la realización de llamadas XMLHttp. Esto incluye todos los exploradores modernos en uso hoy en día. Internet Explorer utiliza el objeto XMLHttp ActiveX mientras que otros exploradores modernos (incluido el próximo Internet Explorer 7) usan un objeto XMLHttp intrínseco. Para determinar mediante programación si un explorador es compatible con las devoluciones de llamada, puede usar el **Request.Browser.SupportCallback** propiedad. Esta propiedad devolverá **true** si el cliente que admite las devoluciones de llamada de secuencia de comandos.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabajar con scripts de cliente en ASP.NET 2.0

Scripts de cliente en ASP.NET 2.0 se administran mediante el uso de la clase ClientScriptManager. Realiza un seguimiento de scripts de cliente utilizando un tipo y un nombre a la clase ClientScriptManager. Esto impide que el mismo script mediante programación que se va a insertar en una página más de una vez.

> [!NOTE]
> Después de una secuencia de comandos se ha registrado correctamente en una página, todos los intentos posteriores para registrar la misma secuencia de comandos simplemente dará como resultado de la secuencia de comandos no está registrada una segunda vez. No hay scripts duplicados se agregan y se produce ninguna excepción. Para evitar el cálculo innecesario, hay métodos que puede usar para determinar si una secuencia de comandos ya está registrado para que no intenta registrarlo más de una vez.


Los métodos de ClientScriptManager deben estar familiarizados para todos los desarrolladores ASP.NET actuales:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Este método agrega una secuencia de comandos a la parte superior de la página representada. Esto es útil para agregar funciones que se llama explícitamente en el cliente.

Hay dos versiones sobrecargadas de este método. Tres de cuatro argumentos son comunes entre ellos. Son estos:

`type (string)`

El ***tipo*** argumento identifica un tipo de la secuencia de comandos. Suele ser una buena idea usar el tipo de la página (esto. GetType()) para el tipo.

`key (string)`

El ***clave*** argumento es una clave definida por el usuario para la secuencia de comandos. Debe ser único para cada secuencia de comandos. Si intenta agregar un script con la misma clave y tipo de una secuencia de comandos ya se ha agregado, no se agregará.

`script (string)`

El ***script*** argumento es una cadena que contiene el script para agregar real. Se recomienda que use un objeto StringBuilder para crear la secuencia de comandos y, a continuación, utilice el método ToString() en StringBuilder para asignar el ***script*** argumento.

Si usas el RegisterClientScriptBlock sobrecargado que sólo toma tres argumentos, debe incluir los elementos de la secuencia de comandos (&lt;script&gt; y &lt;/script&gt;) en el script.

Puede utilizar la sobrecarga de RegisterClientScriptBlock que toma un cuarto argumento. El cuarto argumento es un valor booleano que especifica si ASP.NET debe agregar los elementos de la secuencia de comandos para usted. Si este argumento es **true**, el script no debe incluir explícitamente los elementos de la secuencia de comandos.

Utilice el método IsClientScriptBlockRegistered para determinar si se ha registrado una secuencia de comandos. Esto le permite evitar un intento para volver a registrar un script que ya se ha registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nuevo en 2.0)

La etiqueta RegisterClientScriptInclude crea un bloque de script que se vincula a un archivo de script externo. Tiene dos sobrecargas. Uno toma una clave y una dirección URL. El segundo, agrega un tercer argumento especifica el tipo.

Por ejemplo, el código siguiente genera un bloque de script que se vincula a jsfunctions.js en la raíz de la carpeta de scripts de la aplicación:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Este código genera el siguiente código en la página representada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> El bloque de script se representa en la parte inferior de la página.


Utilice el método IsClientScriptIncludeRegistered para determinar si se ha registrado una secuencia de comandos. Esto le permite evitar un intento para volver a registrar un script.

## <a name="registerstartupscript"></a>RegisterStartupScript

El método RegisterStartupScript toma los mismos argumentos que el método RegisterClientScriptBlock. Un script registrado con RegisterStartupScript ejecuta una vez cargada la página, pero antes que el evento OnLoad del lado cliente. En la versión 1.X, se colocaron los scripts registrados con RegisterStartupScript justo antes de cerrar &lt;/formar&gt; mientras se colocaron los scripts registrados con RegisterClientScriptBlock inmediatamente después de la apertura de etiquetas &lt;formulario&gt; etiqueta. En ASP.NET 2.0, ambos se colocan inmediatamente antes del cierre &lt;/formar&gt; etiqueta.

> [!NOTE]
> Si registra una función con RegisterStartupScript, esa función no se ejecutará hasta que se llama explícitamente en el código del lado cliente.


Utilice el método IsStartupScriptRegistered comprobó para determinar si se ha registrado una secuencia de comandos y evitar un intento para volver a registrar un script.

## <a name="other-clientscriptmanager-methods"></a>Otros métodos ClientScriptManager

Estos son algunos de los otros métodos útiles de la clase ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Vea las devoluciones de llamada del script anterior de este módulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtiene una referencia de JavaScript (javascript:&lt;llamar&gt;) que puede utilizarse para publicar desde un evento del lado cliente.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtiene una cadena que puede utilizarse para iniciar una publicación desde el cliente.                                    |
|      <strong>GetWebResourceUrl</strong>       | Devuelve una dirección URL a un recurso que está incrustado en un ensamblado. Debe usarse junto con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra un recurso Web con la página. Estos son los recursos incrustados en un ensamblado y controlados por el nuevo controlador WebResource.axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo de formulario oculto en la página.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra el código del lado cliente que se ejecuta cuando se envía el formulario HTML.                                   |

