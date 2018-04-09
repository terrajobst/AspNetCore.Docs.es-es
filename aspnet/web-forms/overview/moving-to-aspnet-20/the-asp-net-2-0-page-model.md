---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: El modelo ASP.NET 2.0 página | Documentos de Microsoft
author: microsoft
description: En ASP.NET 1.x, los programadores tenían una opción entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente se pueden implementar mediante el attr de Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a>El modelo de páginas 2.0 de ASP.NET
====================
por [Microsoft](https://github.com/microsoft)

> En ASP.NET 1.x, los programadores tenían una opción entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente se pueden implementar mediante el atributo Src o el atributo de código subyacente de la @Page directiva. En ASP.NET 2.0, los desarrolladores tienen todavía elegir entre código en línea y código subyacente, pero ha habido mejoras considerables en el modelo de código subyacente.


En ASP.NET 1.x, los programadores tenían una opción entre un modelo de código en línea y un modelo de código de código subyacente. Código subyacente se pueden implementar mediante el atributo Src o el atributo de código subyacente de la @Page directiva. En ASP.NET 2.0, los desarrolladores tienen todavía elegir entre código en línea y código subyacente, pero ha habido mejoras considerables en el modelo de código subyacente.

## <a name="improvements-in-the-code-behind-model"></a>Mejoras en el modelo de código subyacente

Para comprender los cambios en el modelo de código subyacente en ASP.NET 2.0, lo mejor posible para ver rápidamente el modelo tal como existía en ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>El modelo de código subyacente en ASP.NET 1.x

En ASP.NET 1.x, consistía en el modelo de código subyacente de un archivo ASPX (el Webform) y un archivo de código subyacente que contiene el código de programación. Los dos archivos estaban conectados mediante la @Page la directiva en el archivo ASPX. Cada control en la página ASPX tenía una declaración correspondiente en el archivo de código subyacente como una variable de instancia. El archivo de código subyacente también contiene código para el enlace de eventos y genera el código necesario para el Diseñador de Visual Studio. Este modelo funcionaba bastante bien, pero dado que cada elemento ASP.NET en la página ASPX requiere código correspondiente en el archivo de código subyacente, no se produjo ningún verdadera separación de código y contenido. Por ejemplo, si un diseñador, agrega un nuevo control de servidor a un archivo ASPX fuera del IDE de Visual Studio, la aplicación se interrumpe debido a la ausencia de una declaración de ese control en el archivo de código subyacente.

## <a name="the-code-behind-model-in-aspnet-20"></a>El modelo de código subyacente en ASP.NET 2.0

ASP.NET 2.0 mejora en gran medida este modelo. En ASP.NET 2.0, código subyacente se implementa mediante la nueva *clases parciales* proporcionados en ASP.NET 2.0. La clase de código subyacente de ASP.NET 2.0 es definen como una clase parcial, lo que significa que contiene solo una parte de la definición de clase. La parte restante de la definición de clase se genera dinámicamente mediante la página ASPX en tiempo de ejecución o al precompilar el sitio Web de ASP.NET 2.0. El vínculo entre el archivo de código subyacente y la página ASPX sigue establecido mediante la directiva @ Page. Sin embargo, en lugar de un atributo de código subyacente o Src ASP.NET 2.0 ahora usa el atributo CodeFile. El atributo Inherits también se utiliza para especificar el nombre de clase para la página.

Una directiva @ Page típica podría tener este aspecto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definición de clase típica en un archivo de código subyacente de ASP.NET 2.0 podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# y Visual Basic son los únicos lenguajes administrados que admiten las clases parciales. Por lo tanto, los desarrolladores que utilizan J# no podrá utilizar el modelo de código subyacente en ASP.NET 2.0.


El nuevo modelo mejora el modelo de código subyacente porque los desarrolladores ahora disponen de los archivos de código que contienen únicamente el código que han creado. También proporciona una separación de código y contenido true porque no hay ninguna declaración de variable de instancia en el archivo de código subyacente.

> [!NOTE]
> Dado que la clase parcial para la página ASPX es donde realiza el enlace de eventos, los desarrolladores de Visual Basic pueden aportar un aumento ligeramente el rendimiento mediante la palabra clave Handles en el código subyacente para enlazar eventos. C# no tiene ninguna palabra clave equivalente.


## <a name="new--page-directive-attributes"></a>Nuevos atributos de la directiva @ Page

ASP.NET 2.0 agrega muchos atributos nuevos a la directiva @ Page. Los atributos siguientes son nuevos en ASP.NET 2.0.

## <a name="async"></a>Async

El atributo Async permite configurar página ejecución asincrónica. También se tratan las páginas asincrónicas más adelante en este módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Especifica el tiempo de espera para las páginas asincrónicas. El valor predeterminado es 45 segundos.

## <a name="codefile"></a>CodeFile

El atributo CodeFile es el reemplazo para el atributo de código subyacente en Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

El atributo CodeFileBaseClass se utiliza en casos donde probablemente prefiera varias páginas para derivar de una sola clase base. Debido a la implementación de clases parciales en ASP.NET, sin este atributo, una clase base que utiliza campos comunes compartidos para hacer referencia a los controles declarados en una página ASPX no funcionará correctamente porque ASP. El motor de compilación de redes crea automáticamente nuevos miembros en función de los controles en la página. Por lo tanto, si desea que una clase base común para dos o más páginas de ASP.NET, debe definir especificar la clase base en el atributo CodeFileBaseClass y, a continuación, derivar cada clase de páginas de esa clase base. El atributo CodeFile también es necesario cuando se utiliza este atributo.

## <a name="compilationmode"></a>compilationMode

Este atributo permite establecer la propiedad CompilationMode de la página ASPX. La propiedad CompilationMode es una enumeración que contiene los valores **siempre**, **automática**, y **nunca**. El valor predeterminado es **siempre**. El **automática** configuración impedirá que ASP.NET compilar dinámicamente la página si es posible. Exclusión de páginas de compilación dinámica aumenta el rendimiento. Sin embargo, si una página que se excluye contiene que el código que se debe compilar, se producirá un error cuando se explora la página.

## <a name="enableeventvalidation"></a>EnableEventValidation

Este atributo especifica si no se validan los eventos de postback y devolución de llamada. Cuando esta opción está habilitada, los argumentos de devolución de datos o eventos de devolución de llamada se comprueban para asegurarse de que se originaron desde el control de servidor que inicialmente los procesó.

## <a name="enabletheming"></a>EnableTheming

Este atributo especifica si no se usan los temas de ASP.NET en una página. El valor predeterminado es **false**. Se tratan los temas de ASP.NET en [módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Este atributo especifica si se deben agregar las directivas pragma de línea durante la compilación. Pragmas de línea son opciones que se usa en depuradores para marcar secciones específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Este atributo especifica si no se inyecta JavaScript en la página con el fin de mantener la posición de desplazamiento entre las devoluciones de datos. Este atributo es **false** de forma predeterminada.

Cuando este atributo es **true**, ASP.NET agregará un &lt;script&gt; bloque en el postback que el siguiente aspecto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Tenga en cuenta que la src para este bloque de script es WebResource.axd. Este recurso no es una ruta de acceso física. Cuando se solicita esta secuencia de comandos, ASP.NET crea dinámicamente la secuencia de comandos.

### <a name="masterpagefile"></a>MasterPageFile

Este atributo especifica el archivo de página maestra para la página actual. La ruta de acceso puede ser absoluta o relativa. Páginas maestras se tratan en [módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Este atributo permite invalidar propiedades de apariencia de interfaz de usuario definidas por un tema de ASP.NET 2.0. Los temas se tratan en [módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Especifica el tema de la página. Si no se especifica un valor para el atributo StyleSheetTheme, el atributo de tema invalida todos los estilos aplicados a los controles de la página.

## <a name="title"></a>Title

Establece el título de la página. El valor especificado aquí aparecerán en la &lt;título&gt; elemento de la página presentada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Establece el valor de la enumeración ViewStateEncryptionMode. Los valores disponibles son **siempre**, **automática**, y **nunca**. El valor predeterminado es **automática**. Cuando este atributo se establece en un valor de **automática**, se cifra el estado de vista es un control lo solicita mediante una llamada a la **RegisterRequiresViewStateEncryption** método.

## <a name="setting-public-property-values-via-the--page-directive"></a>Establecer valores de propiedad pública a través de la directiva @ Page

Otra nueva capacidad de la directiva @ Page en ASP.NET 2.0 es la capacidad para establecer el valor inicial de las propiedades públicas de una clase base. Suponga, por ejemplo, que tiene una propiedad pública denominada **SomeText** en su clase base y d similares para inicializarse a **Hello** cuando se carga una página. Para ello, simplemente establezca el valor en la directiva @ Page de este modo:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

El **SomeText** atributo de la directiva @ Page establece el valor inicial de la propiedad SomeText en la clase base para *Hello!*. El vídeo siguiente es un tutorial de establecer el valor inicial de una propiedad pública en una clase base mediante la directiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nuevas propiedades públicas de la clase de página

Las propiedades públicas siguientes son nuevas en ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Devuelve la ruta de acceso relativa a la aplicación a la página o el control. Por ejemplo, para una página que se encuentra en http://app/folder/page.aspx, la propiedad devuelve ~ / carpeta /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Devuelve la ruta de acceso virtual relativa a la página o el control. Por ejemplo, para una página que se encuentra en http://app/folder/page.aspx, la propiedad devuelve ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtiene o establece el tiempo de espera que se utiliza para el control de página asincrónica. (Las páginas asincrónicas se explicará más adelante en este módulo.)

## <a name="clientquerystring"></a>ClientQueryString

Una propiedad de solo lectura que devuelve la parte de la cadena de consulta de la dirección URL solicitada. Este valor es la dirección URL codificada. Puede utilizar el método UrlDecode de la clase HttpServerUtility para descodificarlo.

## <a name="clientscript"></a>ClientScript

Esta propiedad devuelve un objeto ClientScriptManager que puede usarse para administrar la emisión de ASP.NETs de script de cliente. (La clase ClientScriptManager se trata más adelante en este módulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Esta propiedad controla si no está habilitada la validación de eventos para eventos de postback y devolución de llamada. Cuando se habilita, se comprueban los argumentos de devolución de datos o eventos de devolución de llamada para asegurarse de que se originaron desde el control de servidor que inicialmente los procesó.

## <a name="enabletheming"></a>EnableTheming

Esta propiedad obtiene o establece un valor booleano que especifica si se permite o no un tema de ASP.NET 2.0 se aplica a la página.

## <a name="form"></a>Form

Esta propiedad devuelve el formato HTML en la página ASPX como un objeto HtmlForm.

## <a name="header"></a>Header

Esta propiedad devuelve una referencia a un objeto HtmlHead que contiene el encabezado de página. Puede usar el objeto HtmlHead devuelto para obtener o establecer las hojas de estilos, etiquetas Meta, etcetera.

## <a name="idseparator"></a>IdSeparator

Esta propiedad de solo lectura Obtiene el carácter que se usa para separar los identificadores de control cuando ASP.NET está generando un identificador único para los controles en una página. No debe usarse directamente desde el código.

## <a name="isasync"></a>IsAsync

Esta propiedad permite que las páginas asincrónicas. Las páginas asincrónicas se describen más adelante en este módulo.

## <a name="iscallback"></a>IsCallback

Esta propiedad de solo lectura devuelve **true** si la página es el resultado de una devolución de llamada. Devoluciones de llamadas se tratan más adelante en este módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Esta propiedad de solo lectura devuelve **true** si la página forma parte de una devolución de datos entre páginas. Las devoluciones de datos entre páginas se tratan más adelante en este módulo.

## <a name="items"></a>Elementos

Devuelve una referencia a una instancia de IDictionary que contiene todos los objetos almacenados en el contexto de páginas. Puede agregar elementos a este objeto IDictionary y estarán disponibles durante el ciclo de vida del contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Esta propiedad controla si ASP.NET emite JavaScript que mantiene que las páginas de la posición en el Explorador de desplazamiento después de que se produce un postback. (Los detalles de esta propiedad se han analizado anteriormente en este módulo).

## <a name="master"></a>Master

Esta propiedad de solo lectura devuelve una referencia a la instancia de la página maestra de una página a la que se ha aplicado una página maestra.

## <a name="masterpagefile"></a>MasterPageFile

Obtiene o establece el nombre de archivo de página maestra para la página. Esta propiedad solo puede establecerse en el método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propiedad obtiene o establece la longitud máxima para el estado de páginas en bytes. Si la propiedad se establece en un número positivo, el estado de vista de páginas se dividirse en varios campos ocultos de forma que no supere el número de bytes especificado. Si la propiedad es un número negativo, el estado de vista no se dividirá en fragmentos.

## <a name="pageadapter"></a>PageAdapter

Devuelve una referencia al objeto PageAdapter que modifica la página del explorador que lo solicita.

## <a name="previouspage"></a>PreviousPage

Devuelve una referencia a la página anterior en caso de un Server.Transfer o una devolución de datos entre páginas.

## <a name="skinid"></a>SkinID

Especifica la máscara de ASP.NET 2.0 que se aplica a la página.

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

## <a name="new-protected-properties-of-the-page-class"></a>Nuevas propiedades protegidos de la clase de página

Éstas son las nuevas propiedades protegidas de la clase de página en ASP.NET 2.0.

## <a name="adapter"></a>Adaptador

Devuelve una referencia a la ControlAdapter que presenta la página en el dispositivo que lo ha solicitado.

## <a name="asyncmode"></a>AsyncMode

Esta propiedad indica si la página se procesa de forma asíncrona. Está diseñado para su uso en tiempo de ejecución y no directamente en el código.

## <a name="clientidseparator"></a>ClientIDSeparator

Esta propiedad devuelve el carácter utilizado como separador al crear identificadores de cliente único para los controles. Está diseñado para su uso en tiempo de ejecución y no directamente en el código.

## <a name="pagestatepersister"></a>PageStatePersister

Esta propiedad devuelve el objeto PageStatePersister para la página. Esta propiedad se utiliza principalmente los programadores de controles ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Esta propiedad devuelve un suffic único que se anexa a la ruta de acceso de archivo para almacenar en caché de los exploradores. El valor predeterminado es \_ \_ufps = y un número de 6 dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Nuevos métodos públicos de la clase de página

Los métodos públicos siguientes son nuevos para la clase de página en ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Este método registra a los delegados de controladores de eventos para la ejecución de la página asincrónica. Las páginas asincrónicas se describen más adelante en este módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Se aplica a las propiedades de una hoja de estilos de páginas a la página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Los seres este método una tarea asincrónica.

### <a name="getvalidators"></a>GetValidators

Devuelve una colección de validadores para el grupo de validación especificado o el grupo de validación de forma predeterminada si no se especifica ninguno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Este método registra una nueva tarea asincrónica. Las páginas asincrónicas se tratan más adelante en este módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Este método indica a ASP.NET que se debe conservar el estado de control de páginas.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Este método indica a ASP.NET que el estado de vista de páginas requiere cifrado.

## <a name="resolveclienturl"></a>ResolveClientUrl

Devuelve una dirección URL relativa que puede usarse para las solicitudes de cliente de imágenes, etcetera.

## <a name="setfocus"></a>SetFocus

Este método establecerá el foco al control que se especifica cuando se carga la página por primera vez.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Este método anulará el control que se pasa como ya no requieren persistencia de estado de control.

## <a name="changes-to-the-page-lifecycle"></a>Cambios en el ciclo de vida de la página

El ciclo de vida de la página de ASP.NET 2.0 no ha cambiado considerablemente, pero hay algunos métodos nuevos que debe tener en cuenta. A continuación se indica el ciclo de vida de la página ASP.NET 2.0.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (novedad en ASP.NET 2.0)

El evento PreInit es la primera etapa del ciclo de vida que un programador puede tener acceso. La adición de este evento permite cambiar los temas de ASP.NET 2.0, páginas maestras mediante programación, obtener acceso a propiedades de un perfil de ASP.NET 2.0, etcetera. Si está en un estado de devolución de datos, su importante tener en cuenta que Viewstate no ha se ha aplicado a los controles en este punto del ciclo de vida. Por lo tanto, si un desarrollador cambia una propiedad de un control en esta fase, se sobrescribirá es probable que más adelante en el ciclo de vida de las páginas.

## <a name="init"></a>Init

El evento Init no ha cambiado desde ASP.NET 1.x. Esto es donde se desea leer o inicializar las propiedades de controles en la página. En esta fase, páginas maestras, temas, etc. ya se han aplicado a la página.

## <a name="initcomplete-new-in-20"></a>InitComplete (novedad en 2.0)

El evento InitComplete se llama al final de la fase de inicialización de páginas. En este punto del ciclo de vida, se pueden tener acceso a controles en la página, pero aún no se ha recibido su estado.

## <a name="preload-new-in-20"></a>Cargar previamente (novedad en 2.0)

Este evento se llama después de haber aplicado todos los datos de postback y justo antes de la página\_carga.

## <a name="load"></a>Load

El evento de carga no ha cambiado desde ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (novedad en 2.0)

El evento LoadComplete es el último en la fase de carga de páginas. En esta fase, todos los datos de postback y estado de vista se ha aplicado a la página.

## <a name="prerender"></a>PreRender

Si le gustaría tener en estado de vista se mantenga correctamente para los controles que se agregan a la página de forma dinámica, el evento PreRender es la última oportunidad para agregarlos.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (novedad en 2.0)

En la fase de PreRenderComplete, todos los controles que se agregaron a la página y está lista para representar la página. El evento PreRenderComplete es el último evento que se desencadena antes de que se guarda el estado de vista de páginas.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (novedad en 2.0)

El evento SaveStateComplete se llama inmediatamente después de que todos los Estados de control y el estado de vista de página se ha guardado. Éste es el último evento antes de que la página se representa realmente en el explorador.

## <a name="render"></a>Representar

El método de representación no ha cambiado desde ASP.NET 1.x. Aquí es donde se inicializa el HtmlTextWriter y la página se representa en el explorador.

## <a name="cross-page-postback-in-aspnet-20"></a>Devolución de datos entre páginas de ASP.NET 2.0

En ASP.NET 1.x, las devoluciones de datos fueron necesarios para enviar a la misma página. No se permiten las devoluciones de datos entre páginas. ASP.NET 2.0 agrega la capacidad para enviar a una página diferente mediante la interfaz IButtonControl. Cualquier control que implementa la nueva interfaz IButtonControl (Button, LinkButton e ImageButton además de los controles personalizados de terceros) puede aprovechar esta nueva funcionalidad a través del uso del atributo PostBackUrl. El código siguiente muestra un control de botón que envía a una segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Cuando se devuelve la página, la página que inicia la devolución de datos es accesible a través de la propiedad PreviousPage en la segunda página. Esta funcionalidad se implementa a través de la nueva WebForm\_DoPostBackWithOptions función de cliente que ASP.NET 2.0 se representa en la página cuando un control se envía a una página diferente. Esta función de JavaScript se proporciona con el nuevo controlador WebResource.axd que emite las secuencias de comandos al cliente.

El vídeo siguiente es un tutorial de una devolución de datos entre páginas.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Obtener más detalles sobre las devoluciones de datos entre páginas

### <a name="viewstate"></a>Estado de vista

Se le puede solicitado usted mismo ya sobre lo que ocurre en el estado de vista de la primera página en un escenario de devolución de datos entre páginas. Después de todo, cualquier control que implementan IPostBackDataHandler conservará su estado a través de estado de vista, por lo que para tener acceso a las propiedades de ese control en la segunda página de una devolución de datos entre páginas, debe tener acceso para el estado de vista de la página. ASP.NET 2.0 se encarga de este escenario mediante un nuevo campo oculto en la segunda página llama \_ \_PREVIOUSPAGE. El \_ \_campo de formulario PREVIOUSPAGE contiene el estado de vista para la primera página para que pueda tener acceso a las propiedades de todos los controles en la segunda página.

### <a name="circumventing-findcontrol"></a>Sortear FindControl

En el tutorial en vídeo de una devolución de datos entre páginas, utiliza el método FindControl para obtener una referencia al control de cuadro de texto en la primera página. Ese método es apropiado para ese fin, pero FindControl es costosa y es preciso escribir código adicional. Afortunadamente, ASP.NET 2.0 proporciona una alternativa a FindControl para este propósito que funcionará en muchos escenarios. La directiva PreviousPageType permite disponer de una referencia fuertemente tipados a la página anterior mediante el nombre de tipo o el atributo VirtualPath. El atributo TypeName permite especificar el tipo de la página anterior, mientras que el atributo VirtualPath permite hacer referencia a la página anterior mediante una ruta de acceso virtual. Una vez haya configurado la directiva PreviousPageType, a continuación, debe exponer los controles, etc. a la que desea permitir el acceso mediante las propiedades públicas.

## <a name="lab-1-cross-page-postback"></a>Práctica 1 Postback de página entre

En este laboratorio, creará una aplicación que utiliza la nueva funcionalidad de devolución de datos entre páginas de ASP.NET 2.0.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web de ASP.NET.
2. Agregar un nuevo Webform denominado page2.aspx.
3. Abra Default.aspx en la vista Diseño y agregue un control de botón y un control de cuadro de texto. 

    1. Proporcionar el control de botón en un Id. de **SubmitButton** y el cuadro de texto Id. de control **nombre de usuario**.
    2. Establezca la propiedad PostBackUrl del botón a page2.aspx.
4. Abrir page2.aspx en la vista de origen.
5. Agregue una directiva @ PreviousPageType tal y como se muestra a continuación:
6. Agregue el código siguiente a la página\_carga de código subyacente del page2.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Compile el proyecto haciendo clic en la compilación en el menú Generar.
8. Agregue el código siguiente en el código subyacente para Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Cambiar la página\_carga en page2.aspx al siguiente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile el proyecto.
11. Ejecute el proyecto.
12. Escriba su nombre en el cuadro de texto y haga clic en el botón.
13. ¿Cuál es el resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas asincrónicas en ASP.NET 2.0

Muchos problemas de contención en ASP.NET son causados por la latencia de las llamadas externas (por ejemplo, llamadas de base de datos o servicio Web), latencia de E/S de archivo, etcetera. Cuando se realiza una solicitud en una aplicación de ASP.NET, ASP.NET utiliza uno de sus subprocesos de trabajo para que la solicitud de servicio. Esa solicitud posee ese subproceso hasta que se completa la solicitud y la respuesta se ha enviado. Las búsquedas de ASP.NET 2.0 resolver los problemas de latencia con estos tipos de problemas mediante la adición de la capacidad para ejecutar las páginas de forma asincrónica. Esto significa que un subproceso de trabajo puede iniciar la solicitud y, a continuación, pasar ejecución adicional a otro subproceso, con lo que se devuelve al grupo de subprocesos disponibles rápidamente. Cuando haya finalizado la E/S de archivo, llamada base de datos, etc., se obtiene un nuevo subproceso del grupo de subprocesos para finalizar la solicitud.

El primer paso en la creación de una página ejecutar de forma asincrónica es establecer el **Async** como atributo de la directiva de página:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Este atributo indica a ASP.NET que implemente la IHttpAsyncHandler para la página.

El siguiente paso es llamar al método AddOnPreRenderCompleteAsync en un momento en el ciclo de vida de la página anterior PreRender. (Normalmente se llama a este método en la página\_carga.) El método AddOnPreRenderCompleteAsync toma dos parámetros; un BeginEventHandler y un EndEventHandler. BeginEventHandler devuelve un resultado de IAsyncResult que, a continuación, se pasa como un parámetro a la EndEventHandler.

El vídeo siguiente es un tutorial de una solicitud de página asincrónica.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Una página de async no se representará en el explorador hasta que haya completado la EndEventHandler. Sin duda, pero algunos desarrolladores surgirán solicitudes asincrónicas es similar a las devoluciones de llamada asincrónica. Es importante tener en cuenta que no se encuentran. La ventaja de solicitudes asincrónicas es que se puede devolver el primer subproceso de trabajo para el grupo de subprocesos para atender nuevas solicitudes, lo que reduce la contención debido a que están enlazados de E/S, etcetera.


## <a name="script-callbacks-in-aspnet-20"></a>Devoluciones de llamada de secuencia de comandos en ASP.NET 2.0

Los desarrolladores Web siempre han buscado formas de evitar el parpadeo asociados a una devolución de llamada. En ASP.NET 1.x, SmartNavigation era el método más común para evitar el parpadeo, pero SmartNavigation causa problemas para algunos desarrolladores debido a la complejidad de su implementación en el cliente. ASP.NET 2.0 resuelve este problema con las devoluciones de llamada de la secuencia de comandos. Las devoluciones de llamada de la secuencia de comandos utilizan XMLHttp para realizar solicitudes en el servidor Web a través de JavaScript. El XMLHttpRequest devuelve datos XML que, a continuación, se pueden manipular a través DOM. del explorador Código de XMLHttp se oculta al usuario con el nuevo controlador WebResource.axd.

Hay varios pasos que son necesarios para configurar una devolución de llamada de secuencia de comandos en ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Paso 1: Implementar la interfaz ICallbackEventHandler

En orden para que ASP.NET reconoce la página que participa en una devolución de llamada de secuencia de comandos, debe implementar la interfaz ICallbackEventHandler. Puede hacerlo en el archivo de código subyacente de este modo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

También puede hacer esto mediante similares de la directiva @ Implements así:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente, utilizaría la directiva @ Implements cuando se usa código ASP.NET en línea.

## <a name="step-2--call-getcallbackeventreference"></a>Paso 2: Llamada GetCallbackEventReference

Como se mencionó anteriormente, la llamada de XMLHttp se encapsula en el controlador WebResource.axd. Cuando se representa la página, ASP.NET agregará una llamada a WebForm\_DoCallback, un script de cliente que se proporciona por WebResource.axd. El WebForm\_DoCallback función reemplaza la \_ \_doPostBack función de devolución de llamada. Recuerde que \_ \_doPostBack mediante programación envía el formulario en la página. En un escenario de devolución de llamada, puede evitar una devolución de datos, por lo que \_ \_doPostBack no será suficiente.

> [!NOTE]
> \_\_doPostBack todavía se representa en la página en un escenario de devolución de llamada de script de cliente. Sin embargo, no se utiliza para la devolución de llamada.


Los argumentos para el WebForm\_DoCallback (función) de cliente se proporcionan a través de la función de servidor GetCallbackEventReference que normalmente se llama en la página\_carga. Una llamada a GetCallbackEventReference típica podría tener este aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> En este caso, cm es una instancia de ClientScriptManager. La clase ClientScriptManager se explicará más adelante en este módulo.


Hay varias versiones sobrecargadas de GetCallbackEventReference. En este caso, los argumentos son como sigue:

`this`

Una referencia al control donde se llama a GetCallbackEventReference. En este caso, es la propia página.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argumento de cadena que se pasará en el código de cliente para el evento de servidor. En este caso, se pasa el valor de una lista desplegable de Im llama ddlCompany.

`ShowCompanyName`

El nombre de la función de cliente que va a aceptar el valor devuelto (como cadena) desde el evento de devolución de llamada del lado servidor. Esta función solo se llamará cuando la devolución de llamada del lado servidor se realiza correctamente. Por lo tanto, por motivos de solidez, generalmente se recomienda usar la versión sobrecargada de GetCallbackEventReference que toma un argumento de cadena adicional que especifica el nombre de una función de cliente que se ejecuta si se produce un error.

`null`

Una cadena que representa una función de cliente que inició antes de la devolución de llamada al servidor. En este caso, no hay ninguna secuencia de este tipo, por lo que el argumento es null.

`true`

Valor booleano que especifica si se debe o no realizar la devolución de llamada de forma asincrónica.

La llamada a WebForm\_DoCallback en el cliente pasará estos argumentos. Por lo tanto, cuando esta página se representa en el cliente, que el código tendrá un aspecto de este modo:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Tenga en cuenta que la firma de la función en el cliente es un poco diferente. La función de cliente pasa 5 cadenas y un valor booleano. La cadena adicional (que es null en el ejemplo anterior) contiene la función de cliente que controlará los errores de la devolución de llamada del lado servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Paso 3: Enlazar el evento de Control del lado cliente

Tenga en cuenta que el valor devuelto de GetCallbackEventReference anterior se asigna a una variable de cadena. Esa cadena se usa para enlazar un evento de cliente para el control que inicia la devolución de llamada. En este ejemplo, la devolución de llamada se inicia mediante una lista desplegable en la página, por lo que desea enlazar el *OnChange* eventos.

Para enlazar el evento de cliente, basta con agregar un controlador para el marcado del lado cliente como sigue:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Recuerde que *cbRef* es el valor devuelto de la llamada a GetCallbackEventReference. Contiene la llamada a WebForm\_DoCallback que se mostró anteriormente.

## <a name="step-4--register-the-client-side-script"></a>Paso 4: Registrar la secuencia de comandos de cliente

Recuerde que la llamada a GetCallbackEventReference especifica que un script de cliente llama **ShowCompanyName** se ejecutaría cuando la devolución de llamada del lado servidor se realiza correctamente. Esa secuencia de comandos debe agregarse a la página con una instancia de ClientScriptManager. (La clase ClientScriptManager será describen más adelante en este módulo.) Por lo que hace ese tipo:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Paso 5: Llamar a los métodos de la interfaz ICallbackEventHandler

El ICallbackEventHandler contiene dos métodos que se deben implementar en el código. Únicamente son **RaiseCallbackEvent** y **GetCallbackEvent**.

**RaiseCallbackEvent** toma una cadena como argumento y no devuelve nada. El argumento de cadena se pasa en la llamada de cliente al WebForm\_DoCallback. En este caso, ese valor es el *valor* atributo de la lista desplegable denominada ddlCompany. El código de servidor debe colocarse en el método RaiseCallbackEvent. Por ejemplo, si la devolución de llamada realiza una WebRequest frente a un recurso externo, que el código debe colocarse en RaiseCallbackEvent.

**GetCallbackEvent** es responsable de procesar la devolución de la devolución de llamada al cliente. No toma ningún argumento y devuelve una cadena. La cadena que devuelve se pasará como un argumento a la función de cliente, en este caso *ShowCompanyName*.

Una vez completados los pasos anteriores, está listo para realizar una devolución de llamada de secuencia de comandos en ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Abra vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/callback1.wmv)


Las devoluciones de llamada de secuencia de comandos en ASP.NET son compatibles con cualquier explorador que admita la realización de llamadas de XMLHttp. Incluye todos los exploradores modernos en uso actualmente. Internet Explorer utiliza el objeto de XMLHttp ActiveX mientras que otros exploradores modernos (incluido el próximo Internet Explorer 7) usan un objeto XMLHttp intrínseco. Para determinar mediante programación si un explorador admite las devoluciones de llamada, puede usar el **Request.Browser.SupportCallback** propiedad. Esta propiedad devolverá **true** si el cliente que realiza la solicitud admite devoluciones de llamada de secuencia de comandos.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabajar con scripts de cliente en ASP.NET 2.0

Scripts de cliente en ASP.NET 2.0 se administran a través del uso de la clase ClientScriptManager. La clase ClientScriptManager realiza un seguimiento de scripts de cliente utilizando un tipo y un nombre. Esto impide que la misma secuencia de comandos mediante programación que se va a insertar en una página de más de una vez.

> [!NOTE]
> Después de una secuencia de comandos se ha registrado correctamente en una página, cualquier intento de posterior para registrar la misma secuencia de comandos simplemente se producirá en el script no está registrando una segunda vez. No hay secuencias de comandos duplicadas se agregan y se produce ninguna excepción. Para evitar cálculos innecesarios, existen métodos que puede usar para determinar si ya está registrada una secuencia de comandos para que no intente registrar de más de una vez.


Los métodos de ClientScriptManager deben estar familiarizados para todos los desarrolladores ASP.NET actuales:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Este método agrega una secuencia de comandos a la parte superior de la página representada. Esto es útil para agregar funciones que se llamará de forma explícita en el cliente.

Existen dos versiones sobrecargadas de este método. Tres de cuatro argumentos son comunes entre ellos. Son estos:

`type (string)`

El ***tipo*** argumento identifica un tipo para la secuencia de comandos. Suele ser una buena idea usar tipo de la página (esto. GetType()) para el tipo.

`key (string)`

El ***clave*** argumento es una clave definida por el usuario para la secuencia de comandos. Debe ser único para cada secuencia de comandos. Si intenta agregar un script con la misma clave y tipo de una secuencia de comandos ya ha agregado, no se agregará.

`script (string)`

El ***script*** argumento es una cadena que contiene el script para agregar real. Se recomienda que utilice un objeto StringBuilder para crear la secuencia de comandos y, a continuación, utilice el método ToString() en StringBuilder para asignar el ***script*** argumento.

Si usas el RegisterClientScriptBlock sobrecargado que toma solo tres argumentos, debe incluir los elementos de la secuencia de comandos (&lt;script&gt; y &lt;/script&gt;) en la secuencia de comandos.

Puede utilizar la sobrecarga de RegisterClientScriptBlock que toma un cuarto argumento. El cuarto argumento es un valor booleano que especifica si ASP.NET debe agregar elementos de script para usted. Si este argumento es **true**, la secuencia de comandos no debe incluir explícitamente los elementos de la secuencia de comandos.

Utilice el método IsClientScriptBlockRegistered para determinar si ya se ha registrado una secuencia de comandos. Esto le permite evitar intentos de volver a registrar un script que ya se ha registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (novedad en 2.0)

La etiqueta RegisterClientScriptInclude crea un bloque de script que se vincula a un archivo de script externo. Tiene dos sobrecargas. Una toma una clave y una dirección URL. El segundo, agrega un tercer argumento especifica el tipo.

Por ejemplo, el código siguiente genera un bloque de script que se vincula a jsfunctions.js en la raíz de la carpeta de scripts de la aplicación:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Este código genera el siguiente código en la página presentada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> El bloque de script se representa en la parte inferior de la página.


Utilice el método IsClientScriptIncludeRegistered para determinar si ya se ha registrado una secuencia de comandos. Esto le permite evitar el intento de volver a registrar un script.

## <a name="registerstartupscript"></a>RegisterStartupScript

El método RegisterStartupScript tiene los mismos argumentos que el método RegisterClientScriptBlock. Un script registrado con RegisterStartupScript ejecuta una vez cargada la página, pero antes del evento del lado cliente OnLoad. En 1.X, los scripts registrados con RegisterStartupScript se colocan inmediatamente antes de cerrar &lt;/formar&gt; mientras se colocaron scripts registrados con RegisterClientScriptBlock inmediatamente después de la apertura de etiquetas &lt;formulario&gt; etiqueta. En ASP.NET 2.0, ambos se colocan inmediatamente antes del cierre &lt;/formar&gt; etiqueta.

> [!NOTE]
> Si registra una función con RegisterStartupScript, esa función no se ejecutará hasta que se llama explícitamente en el código de cliente.


Utilice el método IsStartupScriptRegistered comprobó para determinar si ya se ha registrado una secuencia de comandos y evitar un intento de volver a registrar un script.

## <a name="other-clientscriptmanager-methods"></a>Otros métodos ClientScriptManager

Éstos son algunos de los otros métodos de la clase ClientScriptManager útiles.


|  <strong>GetCallbackEventReference</strong>   |                                                 Vea las devoluciones de llamada de secuencia de comandos incluida anteriormente en este módulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtiene una referencia de JavaScript (javascript:&lt;llamar&gt;) que se puede utilizar para registrar desde un evento de cliente.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtiene una cadena que puede utilizarse para iniciar una operación post desde el cliente.                                    |
|      <strong>GetWebResourceUrl</strong>       | Devuelve una dirección URL a un recurso que está incrustado en un ensamblado. Debe utilizarse junto con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra un recurso Web con la página. Estos son los recursos incrustados en ensamblados y administradas por el nuevo controlador WebResource.axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo oculto del formulario en la página.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra el código de cliente que se ejecuta cuando se envía el formulario HTML.                                   |

