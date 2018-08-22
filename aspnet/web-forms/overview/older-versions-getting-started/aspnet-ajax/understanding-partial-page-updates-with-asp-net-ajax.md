---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Descripción de las actualizaciones parciales de página con ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Quizás la característica más visible de ASP.NET AJAX Extensions es la capacidad para realizar las actualizaciones de una página parcial o incremental sin tener que realizar una devolución completa a t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 2e0b1e1d4cbb282e7fd4b27e0a93ba1b9702edea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839208"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Descripción de página parcial se actualiza con ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Quizás la característica más visible de ASP.NET AJAX Extensions es la capacidad para realizar las actualizaciones de una página parcial o incremental sin tener que realizar una devolución completa al servidor, sin cambios en el código y los cambios de marcado mínimo. Las ventajas son inmensas: no se ha modificado el estado de los archivos multimedia (por ejemplo, Adobe Flash o de Windows Media), se reducen los costos de ancho de banda y el cliente no experimenta el parpadeo normalmente asociado a un postback.


## <a name="introduction"></a>Introducción

La tecnología de Microsoft ASP.NET ofrece un modelo de programación orientada a objetos y controlado por eventos y une con las ventajas del código compilado. Sin embargo, su modelo de procesamiento en el servidor tiene varias desventajas inherentes a la tecnología:

- Las actualizaciones de página requieren una vuelta al servidor, lo que requiere una actualización de la página.
- Ida y vuelta no conserva los efectos generados por Javascript u otra tecnología de cliente (por ejemplo, Adobe Flash)
- Durante la devolución de datos, los exploradores que no sean de Microsoft Internet Explorer no admiten restaurar automáticamente la posición de desplazamiento. Y, incluso en Internet Explorer, todavía hay un parpadeo mientras se actualiza la página.
- Las devoluciones de datos pueden implicar una gran cantidad de ancho de banda como la \_ \_puede alcanzar el campo de formulario VIEWSTATE, especialmente cuando se trabaja con controles, como el control GridView o Repeater.
- No hay ningún modelo unificado para tener acceso a servicios Web a través de JavaScript o de otra tecnología del lado cliente.

Escriba las extensiones de AJAX de ASP.NET de Microsoft. AJAX, que es el acrónimo **A** sincrónica **J** avaScript **A** nd **X** ML, es un marco integrado para proporcionar una página incremental las actualizaciones a través de JavaScript de multiplataforma, formado por código de servidor que incluye el marco de AJAX de Microsoft y un componente de script denominado la secuencia de comandos de Microsoft AJAX Library. Las extensiones de ASP.NET AJAX también proporcionan compatibilidad multiplataforma para tener acceso a servicios Web de ASP.NET a través de JavaScript.

Este artículo examina la funcionalidad de las actualizaciones parciales de página de ASP.NET AJAX Extensions, que incluye el componente de ScriptManager, el control UpdatePanel y el control UpdateProgress y tiene en cuenta los escenarios en que se debe o no debe ser la designación.

Estas notas del producto se basan en la versión Beta 2 de Visual Studio 2008 y .NET Framework 3.5, que integra las extensiones de AJAX de ASP.NET en la biblioteca de clases Base (que era anteriormente un componente complementario disponible para ASP.NET 2.0). Estas notas del producto también se da por supuesto que usa Visual Studio 2008 y no Visual Web Developer Express Edition; Algunas plantillas de proyecto que se hace referencia no pueden estar disponibles para los usuarios de Visual Web Developer Express.

## <a name="partial-page-updates"></a>Actualizaciones parciales de página

Quizás la característica más visible de ASP.NET AJAX Extensions es la capacidad para realizar las actualizaciones de una página parcial o incremental sin tener que realizar una devolución completa al servidor, sin cambios en el código y los cambios de marcado mínimo. Las ventajas son inmensas: no se ha modificado el estado de los archivos multimedia (por ejemplo, Adobe Flash o de Windows Media), se reducen los costos de ancho de banda y el cliente no experimenta el parpadeo normalmente asociado a un postback.

La capacidad de integrar la representación de página parcial está integrada en ASP.NET con cambios mínimos en el proyecto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Tutorial: Integración de representación parcial en un proyecto existente


1. En Microsoft Visual Studio 2008, cree un nuevo proyecto de sitio Web de ASP.NET, vaya a <em>archivo</em>  <em>- &gt; New</em>  <em>- &gt; el sitio Web</em> y seleccione el sitio Web de ASP.NET en el cuadro de diálogo. Puede asignarle el nombre que prefiera y puede instalar en el sistema de archivos o en Internet Information Services (IIS).
2. Aparecerá la página en blanco de forma predeterminada con marcado básicos de ASP.NET (un formulario de servidor y un `@Page` directiva). Quitar una etiqueta denominada `Label1` y un botón denominado `Button1` hasta la página dentro del elemento de formulario. Puede establecer sus propiedades de texto que prefiera.
3. En la vista Diseño, haga doble clic en `Button1` para generar un controlador de eventos de código subyacente. Dentro de este controlador de eventos, establecer `Label1.Text` al hacer clic en el botón! .

**Listado 1: Marcado para default.aspx antes de habilita la representación parcial**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Listado 2: Codebehind (recortada) en default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Presione F5 para iniciar el sitio web. Visual Studio le solicitará que agregue un archivo web.config para habilitar la depuración; hacerlo. Al hacer clic en el botón, tenga en cuenta que actualice la página para cambiar el texto en la etiqueta, y hay un breve parpadeo como la página se vuelve a dibujar.
2. Después de cerrar la ventana del explorador, vuelva a Visual Studio y, a la página de marcado. Desplácese hacia abajo en el cuadro de herramientas de Visual Studio y busque la pestaña Extensiones de AJAX. (Si no tiene esta pestaña porque está usando una versión anterior de las extensiones AJAX o Atlas, consulte el tutorial para registrar los elementos de cuadro de herramientas de extensiones de AJAX más adelante en estas notas del producto o instale la versión actual con el instalador de Windows que se puede descargar desde el sitio de Web).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Problema conocido:</em>si instala Visual Studio 2008 en un equipo que ya tiene Visual Studio 2005 instalado con ASP.NET 2.0 AJAX Extensions, Visual Studio 2008 se importarán los elementos de cuadro de herramientas de AJAX Extensions. Puede determinar si este es el caso examinando la información sobre herramientas de los componentes; dicen versión 3.5.0.0. Si dicen que la versión 2.0.0.0, a continuación, ha importado los elementos del cuadro de herramientas anterior y deberá importarlos manualmente mediante el cuadro de diálogo Elegir elementos del cuadro de herramientas en Visual Studio. Podrá agregar controles de la versión 2 a través del diseñador.

2. Antes de la `<asp:Label>` etiqueta comienza, crear una línea de espacio en blanco y haga doble clic en el control UpdatePanel en el cuadro de herramientas. Tenga en cuenta que un nuevo `@Register` directiva se incluye en la parte superior de la página, que indica que los controles dentro del espacio de nombres System.Web.UI deben importarse con el `asp:` prefijo.
3. Arrastre el cierre `</asp:UpdatePanel>` etiqueta más allá del final del elemento Button, por lo que el elemento está bien formado con los controles de etiqueta y botón ajustados.
4. Después de la apertura `<asp:UpdatePanel>` de etiquetas, comience a una nueva etiqueta de apertura. Tenga en cuenta que IntelliSense le indicará con dos opciones. En este caso, cree un `<ContentTemplate>` etiqueta. Asegúrese de ajustar esta etiqueta en torno a la etiqueta y un botón para que el marcado es correcto.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. En cualquier lugar dentro de la `<form>` elemento, incluir un control ScriptManager haciendo doble clic en el `ScriptManager` elemento en el cuadro de herramientas.
2. Editar el `<asp:ScriptManager>` etiquetar para que incluya el atributo `EnablePartialRendering= true`.

**Listado 3: Marcado para default.aspx con la representación parcial habilitada**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Abra el archivo web.config. Tenga en cuenta que Visual Studio ha agregado automáticamente una referencia de compilación a System.Web.Extensions.dll.

1. Novedades de Visual Studio 2008: el archivo web.config que se incluye automáticamente con las plantillas de proyecto de sitio Web ASP.NET incluye todas las referencias necesarias a las extensiones de AJAX de ASP.NET e incluye secciones comentadas de información de configuración que puede ser sin comentada para habilitar la funcionalidad adicional. Visual Studio 2005 tenía plantillas similares al instalar ASP.NET 2.0 AJAX Extensions. Sin embargo, en Visual Studio 2008, las extensiones de AJAX son participar de forma predeterminada (es decir, se hace referencia de forma predeterminada, pero se pueden quitar como referencias).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Presione F5 para iniciar el sitio Web. Tenga en cuenta cómo eran necesarios para admitir la representación parcial no hay cambios de código fuente - solo de marcado se ha cambiado.

Al iniciar el sitio Web, debería ver que la representación parcial está ahora habilitada, porque al hacer clic en el botón existe no estará parpadeo ni se producirá ningún cambio en la posición de desplazamiento de página (en este ejemplo no muestra que). Si tuviera que ver en el origen de la página representado tras hacer clic en el botón, se confirmará que en realidad no se ha producido una devolución de datos: el texto de la etiqueta original sigue siendo parte del marcado de origen y la etiqueta ha cambiado a través de JavaScript.

Visual Studio 2008 no parecen venir con una plantilla predefinida para un sitio web de ASP.NET con AJAX habilitado. Sin embargo, dicha plantilla no estaba disponible en Visual Studio 2005 si se instalaron los Visual Studio 2005 y ASP.NET 2.0 AJAX Extensions. Por lo tanto, configurar un sitio web y a partir de la plantilla sitio de Web habilitado para AJAX probablemente será aún más fáciles, como la plantilla debe incluir un archivo web.config configura totalmente (compatibles con todas las extensiones de AJAX de ASP.NET, incluido el acceso de los servicios Web y la serialización de JSON - JavaScript Object Notation) e incluye un UpdatePanel y ContentTemplate dentro de la página principal de formularios Web Forms de forma predeterminada. Habilitación de la representación parcial con esta página predeterminada es tan sencillo como volver a visitar paso 10 de este tutorial y colocando controles en la página.

## <a name="the-scriptmanager-control"></a>El Control ScriptManager

## <a name="scriptmanager-control-reference"></a>Referencia de Control de ScriptManager

Propiedades de marcado habilitados:

| **Nombre de propiedad** | **Type** | **Descripción** |
| --- | --- | --- |
| AllowCustomErrors-redirección | Bool | Especifica si se debe usar la sección de errores personalizados del archivo web.config para controlar los errores. |
| Mensaje de AsyncPostBackError | String | Obtiene o establece el mensaje de error enviado al cliente si se produce un error. |
| AsyncPostBack-Timeout | Int32 | Obtiene o establece la cantidad predeterminada de una hora en que un cliente debe esperar que finalice la solicitud asincrónica. |
| EnableScript globalización | Bool | Obtiene o establece si se habilita la globalización de la secuencia de comandos. |
| Localización de EnableScript | Bool | Obtiene o establece si se habilita la localización del script. |
| ScriptLoadTimeout | Int32 | Determina el número de segundos que se permiten para cargar los scripts en el cliente |
| ScriptMode | Enum (Auto, Debug, Release, heredar) | Obtiene o establece si se debe representar las versiones de las secuencias de comandos |
| ScriptPath | String | Obtiene o establece la ruta de acceso a la ubicación de archivos de script que se envía al cliente. |

Propiedades de sólo código:

| **Nombre de propiedad** | **Type** | **Descripción** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Obtiene información detallada sobre el proxy de servicio de autenticación de ASP.NET que se enviará al cliente. |
| IsDebuggingEnabled | Bool | Obtiene si scripts y está habilitada la depuración de código. |
| IsInAsyncPostback | Bool | Obtiene si la página está actualmente en una solicitud de devolución asincrónica. |
| ProfileService | ProfileService-Manager | Obtiene información detallada sobre el proxy de servicio de generación de perfiles de ASP.NET que se enviará al cliente. |
| Scripts | Colección&lt;referencia de Script&gt; | Obtiene una colección de referencias de script que se enviará al cliente. |
| Servicios | Colección&lt;referencia de servicio&gt; | Obtiene una colección de referencias de proxy de servicio Web que se enviará al cliente. |
| SupportsPartialRendering | Bool | Obtiene si el cliente actual admite la representación parcial. Si esta propiedad devuelve **false**, entonces todas las solicitudes de página, se producirá devoluciones de datos estándares. |

Métodos de código público:

| **Nombre del método** | **Type** | **Descripción** |
| --- | --- | --- |
| SetFocus(string) | Void | Establece el foco del cliente a un control determinado cuando se haya completado la solicitud. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;AuthenticationService&gt; | Proporciona detalles sobre el proxy al servicio de autenticación de ASP.NET. |
| &lt;ProfileService&gt; | Proporciona detalles sobre el proxy para el servicio de generación de perfiles de ASP.NET. |
| &lt;Scripts&gt; | Proporciona las referencias de script adicionales. |
| &lt;ASP: ScriptReference&gt; | Denota una referencia de script específicos. |
| &lt;Servicio&gt; | Proporciona las referencias de servicio Web adicionales que tendrán las clases de proxy generadas. |
| &lt;ASP: ServiceReference&gt; | Denota una referencia de servicio Web específica. |

El control ScriptManager es el núcleo esencial para las extensiones de AJAX de ASP.NET. Proporciona acceso a la biblioteca de scripts (incluido el sistema de tipos extenso del script de cliente), admite la representación parcial y proporciona una amplia compatibilidad para servicios adicionales de ASP.NET (por ejemplo, autenticación y generación de perfiles, sino también otros servicios Web). El control ScriptManager también proporciona compatibilidad con la globalización y localización los scripts de cliente.

## <a name="providing-alterative-and-supplemental-scripts"></a>Proporciona secuencias de comandos adicionales y alternativo

Aunque Microsoft ASP.NET 2.0 AJAX Extensions incluyen el código de secuencia de comandos completa en ambas versiones de depuración y versión ediciones como recursos incrustados en los ensamblados de referencia, los desarrolladores son gratuitos redirigir el ScriptManager a archivos de script personalizado, así como para registrar scripts necesarios adicionales.

Para reemplazar el enlace predeterminado para las secuencias de comandos incluyen normalmente (por ejemplo, aquellos que son compatibles con el espacio de nombres Sys.WebForms y el sistema de escritura personalizado), puede registrarse para obtener el `ResolveScriptReference` eventos de la clase ScriptManager. Cuando se llama a este método, el controlador de eventos tiene la oportunidad de cambiar la ruta de acceso al archivo de script en cuestión. el director de scripts, a continuación, enviará una copia diferente o personalizada de las secuencias de comandos al cliente.

Además, las referencias de script (representado por la `ScriptReference` clase) se pueden incluir, mediante programación o a través de marcado. Para ello, modifique mediante programación el `ScriptManager.Scripts` colección, o incluir `<asp:ScriptReference>` etiquetas en la `<Scripts>` etiqueta, que es un elemento secundario de primer nivel del control ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Control UpdatePanel de errores personalizado

Aunque las actualizaciones se controlan mediante desencadenadores especificados por controles UpdatePanel, la compatibilidad con control de errores y mensajes de error personalizados se controla mediante la instancia del control de ScriptManager de la página. Esto se hace mediante la exposición de un evento, `AsyncPostBackError`, la página que puede, a continuación, proporcionar lógica personalizada de control de excepciones.

Al consumir el evento AsyncPostBackError, puede especificar el `AsyncPostBackErrorMessage` propiedad, que, a continuación, hace que un cuadro de alerta que se genera tras la finalización de la devolución de llamada.

Personalización del lado cliente también es posible en lugar de usar el cuadro de alerta de forma predeterminada; Por ejemplo, es posible que desea mostrar una personalizada `<div>` elemento en lugar de con el cuadro de diálogo modal del explorador predeterminado. En este caso, puede controlar el error en el script de cliente:

**Listado 5: Script de cliente para mostrar los errores personalizados**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Es bastante simple, el script anterior registra una devolución de llamada con el tiempo de ejecución de AJAX del lado cliente para cuando se haya completado la solicitud asincrónica. A continuación, se comprueba si se ha notificado un error y si es así, procesa los detalles del mismo, por último, que indica al tiempo de ejecución que se ha controlado el error en el script personalizado.

## <a name="globalization-and-localization-support"></a>La globalización y la compatibilidad de localización

El control ScriptManager ofrece una amplia compatibilidad para la localización de cadenas de script y componentes de la interfaz de usuario; Sin embargo, ese tema está fuera del ámbito de este artículo. Para obtener más información, consulte las notas del producto, la compatibilidad de globalización en ASP.NET AJAX Extensions.

## <a name="the-updatepanel-control"></a>El Control UpdatePanel

## <a name="updatepanel-control-reference"></a>Referencia de Control UpdatePanel

Propiedades de marcado habilitados:

| **Nombre de propiedad** | **Type** | **Descripción** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Especifica si los controles secundarios invocan automáticamente la actualización en el postback. |
| RenderMode | enum (bloque, en línea) | Especifica que la forma en que el contenido se presentarán visualmente. |
| UpdateMode | enum (siempre, condicional) | Especifica si el control UpdatePanel se actualiza siempre durante una presentación parcial o si sólo se actualiza cuando se alcanza un desencadenador. |

Propiedades de sólo código:

| **Nombre de propiedad** | **Type** | **Descripción** |
| --- | --- | --- |
| IsInPartialRendering | bool | Obtiene si el control UpdatePanel es compatible con la representación parcial de la solicitud actual. |
| ContentTemplate | ITemplate | Obtiene la plantilla de marcado para la solicitud de actualización. |
| ContentTemplateContainer | Control | Obtiene la plantilla de programación para la solicitud de actualización. |
| Desencadenadores | UpdatePanel - TriggerCollection | Obtiene la lista de los desencadenadores asociados con el control UpdatePanel actual. |

Métodos de código público:

| **Nombre del método** | **Type** | **Descripción** |
| --- | --- | --- |
| Update() | Void | Actualiza un UpdatePanel especificado mediante programación. Permite que una solicitud de servidor desencadenar una representación parcial de un UpdatePanel en caso contrario,-no desencadenado. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ContentTemplate&gt; | Especifica el marcado que se usará para representar el resultado de la representación parcial. Elemento secundario de &lt;asp: UpdatePanel&gt;. |
| &lt;Desencadenadores&gt; | Especifica una colección de *n* controles asociados con este UpdatePanel de la actualización. Elemento secundario de &lt;asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Especifica un desencadenador que invoca una representación de página parcial para el control UpdatePanel determinado. Esto puede o no puede ser un control como un descendiente de UpdatePanel en cuestión. Granulares para el nombre del evento. Elemento secundario de &lt;desencadenadores&gt;. |
| &lt;asp:PostBackTrigger&gt; | Especifica un control que hace que toda la página Actualizar. Esto puede o no puede ser un control como un descendiente de UpdatePanel en cuestión. Granulares para el objeto. Elemento secundario de &lt;desencadenadores&gt;. |

El `UpdatePanel` es el control que delimita el contenido del servidor que se formen parte de la funcionalidad de representación parcial de las extensiones de AJAX. No hay ningún límite al número de controles UpdatePanel que puede estar en una página y se pueden anidar. Cada UpdatePanel está aislado, por lo que cada uno puede trabajar de forma independiente (puede tener dos UpdatePanels ejecutando al mismo tiempo, representar diferentes partes de la página, independientemente de la devolución de datos de la página).

UpdatePanel controlan principalmente ofertas con desencadenadores de control: de forma predeterminada, cualquier control dentro de un UpdatePanel `ContentTemplate` que crea una devolución de datos se registra como un desencadenador para el control UpdatePanel. Esto significa que UpdatePanel puede trabajar con los controles enlazados a datos de forma predeterminada (como GridView), con controles de usuario, y se pueden programar en script.

De forma predeterminada, cuando se desencadena una representación de página parcial, se actualizarán todos los controles UpdatePanel en la página, si los controles UpdatePanel definen desencadenadores para dicha acción. Por ejemplo, si un UpdatePanel define un control de botón y se hace clic en ese control de botón, se actualizarán todos los controles UpdatePanel en la página de forma predeterminada. Esto es porque, de forma predeterminada, el `UpdateMode` propiedad de UpdatePanel se establece en `Always`. Como alternativa, puede establecer la propiedad UpdateMode en `Conditional`, lo que significa que el control UpdatePanel sólo se actualizará si se alcanza un desencadenador específico.

## <a name="custom-control-notes"></a>Notas de Control personalizado

Un UpdatePanel se puede agregar a cualquier control de usuario o un control personalizado; Sin embargo, la página en el que se incluyen estos controles también debe incluir un control ScriptManager con la propiedad EnablePartialRendering establecida en **true**.

Una manera en que podría tener esto en cuenta al usar controles Web personalizados consiste en invalidar protegido `CreateChildControls()` método de la `CompositeControl` clase. Al hacerlo, puede insertar un UpdatePanel entre los elementos secundarios del control y el mundo exterior, si determina que la página admite la representación parcial; en caso contrario, simplemente puede agregar los controles secundarios en un contenedor `Control` instancia.

## <a name="updatepanel-considerations"></a>Consideraciones de UpdatePanel

UpdatePanel funciona como un elemento de una caja negra, ajuste las devoluciones de ASP.NET dentro del contexto de un JavaScript XMLHttpRequest. Sin embargo, existen consideraciones de rendimiento significativas a tener en cuenta, tanto en términos de comportamiento y la velocidad. Para entender cómo funciona el control UpdatePanel, por lo que pueda decidir mejor cuando su uso es adecuado, debe examinar el intercambio de AJAX. El ejemplo siguiente usa un sitio existente y, Mozilla Firefox con la extensión Firebug (Firebug captura datos XMLHttpRequest).

Considere la posibilidad de un formulario que, entre otras cosas, tiene un cuadro de texto del código postal que se supone que debe para rellenar un campo de ciudad y estado en un formulario o control. Este formulario finalmente recopila información de suscripción, incluido el nombre, dirección e información de contacto de un usuario. Hay muchas consideraciones de diseño para tener en cuenta, según los requisitos de un proyecto específico.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


En la iteración original de esta aplicación, se creó un control que incorpora la totalidad de los datos de registro de usuario, incluidos el código postal, ciudad y estado. Todo el control se ajusta dentro de un UpdatePanel y coloca en un formulario Web. Cuando se escribe el código postal del usuario, el control UpdatePanel detecta el evento (el evento TextChanged correspondiente en el back-end, mediante la especificación de los desencadenadores o mediante el uso de la propiedad ChildrenAsTriggers establecida en true). AJAX registra todos los campos dentro del UpdatePanel, según lo capturado por FireBug (consulte el diagrama de la derecha).

Como indica la captura de pantalla, los valores de todos los controles UpdatePanel se entregan (en este caso, están vacías), así como el campo de ViewState. Todo este trabajo en 9kb de datos se envía, cuando en realidad sólo cinco bytes de datos eran necesarios para realizar esta solicitud particular. La respuesta es incluso más recargada: en total, 57kb se envía al cliente, simplemente para actualizar un campo de texto y un campo de lista desplegable.

También puede ser de interés para ver cómo se actualiza la presentación AJAX de ASP.NET. La parte de la respuesta de solicitud de actualización de UpdatePanel se muestra en la pantalla de la consola Firebug a la izquierda; es una formula especialmente delimitada por barras verticales cadena que está organizada por el script de cliente y, a continuación, vuelve a ensamblar en la página. En concreto, ASP.NET AJAX establece el *innerHTML* propiedad del elemento HTML en el cliente que representa el UpdatePanel. Como el explorador vuelve a genera el DOM, hay un ligero retraso, según la cantidad de información que debe procesarse.

La regeneración del DOM desencadena una serie de problemas adicionales:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Si el elemento HTML con está dentro del UpdatePanel, perderá el foco. Por lo tanto, para los usuarios que se presiona la tecla Tab para salir del cuadro de texto del código postal, su siguiente destino habría sido el cuadro de texto de la ciudad. Sin embargo, una vez que UpdatePanel actualiza la presentación, el formulario ya no habría tenía el foco y presiona Tab habría iniciado resaltar los elementos del enfoque (por ejemplo, vínculos).
- Si cualquier tipo de script de cliente personalizado está en uso que tiene acceso a elementos de DOM, las referencias se conservan por funciones puede quedar inactivo después de una devolución parcial.

UpdatePanel no pretenden ser soluciones catch-all. En su lugar, proporcionan una solución rápida para determinadas situaciones, incluidas la creación de prototipos, pequeño controlar las actualizaciones y proporcionar una interfaz conocida a los desarrolladores de ASP.NET que podrían estar familiarizado con el modelo de objetos de .NET pero menos, de manera con DOM. Hay una serie de alternativas que pueden provocar un rendimiento mejor, según el escenario de aplicación:

- Considere la posibilidad de usar los PageMethods y JSON (JavaScript Object Notation) permite al desarrollador invocar métodos estáticos en una página como si se invocó una llamada de servicio web. Dado que los métodos son estáticos, no es necesario; no hay ningún estado el llamador de la secuencia de comandos proporciona los parámetros y el resultado se devuelve de forma asincrónica.
- Considere el uso de un servicio Web y JSON si un control único debe usarse en varios lugares a través de una aplicación. Este nuevo requiere muy poco trabajo especial y funciona de forma asincrónica.

Incorporación de funcionalidad a través de servicios Web o los métodos de página tiene inconvenientes también. En primer lugar y ante todo, los desarrolladores de ASP.NET generalmente se tienden a crear componentes pequeños de funcionalidad en controles de usuario (archivos .ascx). Los métodos de página no se puede hospedar en estos archivos; deben hospedarse dentro de la clase de página .aspx real. De forma similar, los servicios Web, se deben hospedar dentro de la clase asmx. Dependiendo de la aplicación, esta arquitectura puede infringir el principio de responsabilidad única, en que la funcionalidad para un único componente se extiende ahora por dos o más componentes físicos que pueden tener valores equivalentes cohesivas poca o ninguna.

Por último, si una aplicación requiere que se usan los UpdatePanels, deben ayudar las directrices siguientes con la solución de problemas y mantenimiento.

- **Anidar UpdatePanels poco como sea posibles, no solo dentro de unidades, sino también a través de unidades de código.** Por ejemplo, tener un UpdatePanel en una página que contiene un Control, mientras que ese Control también contiene un UpdatePanel, que contiene otro Control que contiene un control UpdatePanel, es anidamiento entre unidades. Esto ayuda a aclarar qué elementos debe estar actualizando y evita que actualizaciones inesperadas al elemento secundario UpdatePanels.
- **Mantener la *ChildrenAsTriggers* propiedad establecida en false y establece explícitamente la activación de eventos.** Uso de la `<Triggers>` colección es una manera mucho más clara para controlar los eventos y puede evitar un comportamiento inesperado, ayudando a las tareas de mantenimiento y forzar que un desarrollador a participar en un evento.
- **Utilice la unidad más pequeña posible para lograr la funcionalidad.** Como se indicó en la descripción del servicio código postal, ajuste que solo el mínimo reduce el tiempo para el servidor de procesamiento total y la superficie del intercambio cliente / servidor, mejora del rendimiento.

## <a name="the-updateprogress-control"></a>El Control UpdateProgress

## <a name="updateprogress-control-reference"></a>Referencia de Control UpdateProgress

Propiedades de marcado habilitados:

| **Nombre de propiedad** | **Type** | **Descripción** |
| --- | --- | --- |
| AssociatedUpdate PanelID | String | Especifica el identificador de UpdatePanel que deben notificar este UpdateProgress en. |
| DisplayAfter | Valor int. | Especifica el tiempo de espera en milisegundos antes de este control se muestra después de que comience la solicitud asincrónica. |
| DynamicLayout | bool | Especifica si el progreso se representa de forma dinámica. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contiene la plantilla de control establecido para el contenido que se mostrará con este control. |

El control UpdateProgress ofrece una medida de comentarios para mantener el interés de los usuarios mientras se realiza el trabajo necesario para el transporte en el servidor. Esto puede ayudar a los usuarios saber que se esté haciendo algo, aunque puede que no sea evidente, sobre todo porque la mayoría de los usuarios se usa para la página actualización y ver la barra de estado resaltar.

Como una nota, controles UpdateProgress pueden aparecer en cualquier lugar en una jerarquía de página. Sin embargo, en los casos en que se inicia una devolución parcial desde un elemento secundario de UpdatePanel (donde un UpdatePanel se anida dentro de otro UpdatePanel), las devoluciones de datos que el elemento secundario UpdatePanel provocará que se muestren plantillas de UpdateProgress para el elemento secundario de desencadenador UpdatePanel, así como el elemento primario de UpdatePanel. Sin embargo, si el desencadenador es un elemento secundario directo del elemento primario UpdatePanel, a continuación, se mostrará solo las plantillas de UpdateProgress asociadas al elemento primario.

## <a name="summary"></a>Resumen

Las extensiones de Microsoft ASP.NET AJAX son sofisticados productos diseñados para ayudar a mejorar la accesibilidad de contenido web y para proporcionar una experiencia del usuario a sus aplicaciones web. Como parte de las extensiones de AJAX de ASP.NET, los controles de representación parcial de la página, incluidos los controles de ScriptManager, UpdatePanel y UpdateProgress son algunos de los componentes del Kit de herramientas más visibles.

El componente de ScriptManager integra el aprovisionamiento de JavaScript de cliente para las extensiones, así como permite a los distintos componentes de servidor y cliente para que funcione con inversión mínima en desarrollo.

El control UpdatePanel es el cuadro de magic aparente: marcado dentro del UpdatePanel puede tener código subyacente del lado servidor y no desencadenan una actualización de la página. Controles UpdatePanel se pueden anidar y pueden depender de los controles de otros UpdatePanel. De forma predeterminada, UpdatePanels controlar los postbacks invocados por sus controles descendientes, aunque esta funcionalidad se puede llevar a cabo una optimizar, ya sea de forma declarativa o mediante programación.

Cuando se usa el control UpdatePanel, los desarrolladores deben tener en cuenta el impacto de rendimiento que potencialmente podría surgir. Las alternativas posibles incluyen servicios web y los métodos de página, aunque se debe considerar el diseño de la aplicación.

El control permite al usuario saber que los que no se omite, y que está ocurriendo la solicitud en segundo plano mientras la página en caso contrario, no es de UpdateProgress hacer nada para responder a la entrada del usuario. También incluye la posibilidad de anular los resultados de representación parcial.

Juntas, estas herramientas ayudan a crear una experiencia de usuario enriquecida y perfecta realizando trabajo servidor menos evidente para el usuario e interrumpir el flujo de trabajo menos.

## <a name="bio"></a>Biografía

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Siguiente](understanding-asp-net-ajax-updatepanel-triggers.md)
