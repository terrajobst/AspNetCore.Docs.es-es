---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Descripción de los desencadenadores ASP.NET AJAX UpdatePanel | Microsoft Docs
author: scottcate
description: Cuando se trabaja en el editor de marcado en Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de qu...
ms.author: aspnetcontent
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1b2e70a2b074d3c0a2fa4b669b3645ae5f7f790e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811936"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Descripción de los desencadenadores UpdatePanel de ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Cuando se trabaja en el editor de marcado en Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de los cuales es el elemento de los desencadenadores, que especifica los controles en la página (o el control de usuario, si está usando uno) que desencadenará una representación parcial del control UpdatePanel en el que reside el elemento.


## <a name="introduction"></a>Introducción

La tecnología de Microsoft ASP.NET ofrece un modelo de programación orientada a objetos y controlado por eventos y une con las ventajas del código compilado. Sin embargo, su modelo de procesamiento en el servidor tiene varias desventajas inherentes de la tecnología, muchas de las cuales se pueden solucionar si las nuevas características incluidas en las extensiones de AJAX de Microsoft ASP.NET 3.5. Estas extensiones permiten muchas nuevas características de cliente enriquecidas, como la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad de acceder a servicios Web a través de script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente diseñado para crear el reflejo de muchos de los esquemas de control que se muestra en el conjunto de controles de servidor ASP.NET.

Este artículo examina la funcionalidad de los desencadenadores de XML de ASP.NET AJAX `UpdatePanel` componente. Los desencadenadores de XML proporcionan un control granular sobre los componentes que puede producir la representación parcial para determinados controles de UpdatePanel.

Estas notas del producto se basan en la versión Beta 2 de .NET Framework 3.5 y Visual Studio 2008. Las extensiones de AJAX de ASP.NET, previamente un ensamblado de complemento destinado a ASP.NET 2.0, ahora están integradas en la biblioteca de clases Base de .NET Framework. Estas notas del producto también se da por supuesto que trabajará con Visual Studio 2008, no Visual Web Developer Express y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio (aunque serán totalmente compatibles con independencia de los listados de código entorno de desarrollo).

## <a name="triggers"></a>*Desencadenadores*

Los desencadenadores para una determinada UpdatePanel, de forma predeterminada, incluyen automáticamente todos los controles secundarios que invocan una devolución de datos, incluidos, (por ejemplo), los controles de cuadro de texto que tienen sus `AutoPostBack` propiedad establecida en **true**. Sin embargo, desencadenadores también se incluye de manera declarativa mediante marcado; Esto se realiza en el `<triggers>` sección de la declaración del control UpdatePanel. Aunque los desencadenadores se pueden acceder mediante el `Triggers` propiedad de colección, se recomienda registrar todos los desencadenadores de representación parcial en tiempo de ejecución (por ejemplo, si un control no está disponible en tiempo de diseño) mediante el uso de la `RegisterAsyncPostBackControl(Control)` método de la ScriptManager de objeto dentro de la página, el `Page_Load` eventos. Recuerde que las páginas no tienen estadas y, por lo que se deben volver a registrar estos controles cada vez que se crearon.

Inclusión de desencadenador automático secundarios también se puede deshabilitar (de modo que los controles secundarios que creación las devoluciones de datos desencadena automáticamente crea la representación parcial) estableciendo el `ChildrenAsTriggers` propiedad **false**. Esto le permite la máxima flexibilidad en la asignación de qué controles específicos puede invocar una representación de página y se recomienda, por lo que un desarrollador se participar en para responder a un evento, en lugar de controlar los eventos que puedan surgir.

Tenga en cuenta que cuando se anidan controles UpdatePanel, cuando el UpdateMode está establecido en **condicional**, si el elemento secundario UpdatePanel se desencadene, pero el elemento primario no es así, es, a continuación, solo el elemento secundario UpdatePanel se actualizará. Sin embargo, si el elemento primario UpdatePanel se actualiza, a continuación, el elemento secundario de UpdatePanel también se actualizan.

## <a name="the-lttriggersgt-element"></a>*El &lt;desencadenadores&gt; elemento*

Cuando se trabaja en el editor de marcado en Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un `UpdatePanel` control. El elemento que se ve con mayor frecuencia es el `<ContentTemplate>` elemento, que básicamente encapsula el contenido que se retiene el panel de actualización (es decir, el contenido para el que se va a habilitar la representación parcial). El otro elemento es el `<Triggers>` elemento, que especifica los controles en la página (o el control de usuario, si está usando uno) que desencadenará una representación parcial del control UpdatePanel en el que el &lt;desencadenadores&gt; elemento reside.

El `<Triggers>` elemento puede contener cualquier número de cada de dos nodos secundarios: `<asp:AsyncPostBackTrigger>` y `<asp:PostBackTrigger>`. Aceptan dos atributos, `ControlID` y `EventName`y puede especificar cualquier Control dentro de la unidad actual de la encapsulación (por ejemplo, si el control UpdatePanel reside dentro de un Control de usuario Web, no debe intentar hacer referencia a un Control en la página en el que residirá el Control de usuario).

El `<asp:AsyncPostBackTrigger>` elemento es especialmente útil en que puede tener como destino cualquier evento de un Control que existe como un elemento secundario de *cualquier* control UpdatePanel en la unidad de encapsulación, no solo el UpdatePanel en la que este desencadenador es un elemento secundario . Por lo tanto, se puede realizar cualquier control para desencadenar una actualización parcial de la página.

De forma similar, la `<asp:PostBackTrigger>` elemento se puede usar para representar una página parcial de desencadenador, pero que requiere una vuelta completa al servidor. Este elemento trigger también puede usarse para forzar una presentación de página completa, cuando un control en caso contrario, normalmente en cuestión desencadenaría una representación de página parcial (por ejemplo, cuando un `Button` control existe en el `<ContentTemplate>` elemento de un control UpdatePanel). De nuevo, el elemento PostBackTrigger puede especificar cualquier control que es un elemento secundario de cualquier control UpdatePanel en la unidad actual de la encapsulación.

## <a name="lttriggersgt-element-reference"></a>*&lt;Desencadenadores&gt; referencia del elemento*

*Descendientes de marcado:*

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Especifica un control y el evento que hará que una actualización parcial de páginas para el control UpdatePanel que contiene esta referencia del desencadenador. |
| &lt;asp:PostBackTrigger&gt; | Especifica un control y el evento que hará que una actualización de página completa (una actualización de página completa). Esta etiqueta se puede usar para forzar una actualización completa cuando un control en caso contrario, podría desencadenar la representación parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Tutorial: Entre UpdatePanel desencadenadores*

1. Crear una nueva página ASP.NET con un objeto de ScriptManager para habilitar la representación parcial. Agregue dos UpdatePanel en esta página, en la primera, incluyen un control de etiqueta (Label1) y dos controles Button (Button1 y Button2). Button1 debe indicar, haga clic para actualizar y Button2 debe indicar, haga clic para actualizar este o algo similar. En el segundo control UpdatePanel, incluir un control de etiqueta (Label2), pero establezca su propiedad de color de primer plano en algo distinto del predeterminado para diferenciarla.
2. Establezca la propiedad de UpdateMode de las dos etiquetas de UpdatePanel a **condicional**.

**Listado 1: Marcado para default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. En el controlador de eventos Click de Button1, establezca Label1.Text y Label2.Text en algo dependientes del tiempo (por ejemplo, DateTime.Now.ToLongTimeString()). Para el controlador de eventos Click para Button2 Label1.Text solo en el valor dependiente del tiempo.

**Listado 2: Codebehind (recortada) en default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Presione F5 para compilar y ejecutar el proyecto. Tenga en cuenta que, al hacer clic en actualizar los paneles, ambas etiquetas cambian texto; Sin embargo, al hacer clic en Panel de esta actualización, se actualiza solo Label1.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Aspectos técnicos*

Al utilizar el ejemplo que acabamos de construir, podemos echamos un vistazo a lo que hace ASP.NET AJAX y cómo funcionan nuestros los desencadenadores UpdatePanel de entre-panel. Para ello, vamos a trabajar con el origen de la página generada HTML, así como la extensión de Mozilla Firefox denominada a FireBug - con él, podemos examinar fácilmente las devoluciones de AJAX. También usamos la herramienta .NET Reflector de Lutz Roeder. Ambas herramientas están disponibles de forma gratuita en línea y pueden encontrarse con una búsqueda en internet.

Un examen del código fuente de página muestra casi nada extraordinario; los controles UpdatePanel se representan como `<div>` contenedores y podemos ver que el recurso de script incluye proporcionados por el `<asp:ScriptManager>`. También hay algunas nuevas llamadas de específica de AJAX en PageRequestManager internas de la biblioteca de scripts de cliente de AJAX. Por último, veremos los dos contenedores de UpdatePanel - uno con el texto representado `<input>` botones con los dos `<asp:Label>` los controles se representan como `<span>` contenedores. (Si inspecciona el árbol DOM en FireBug, observará que las etiquetas aparecen atenuadas para indicar que no producen contenido visible).

Haga clic en el botón de actualización este Panel y observe que el UpdatePanel superior se actualizará con la hora actual del servidor. En FireBug, elija la pestaña de la consola para que se puede examinar la solicitud. Examine los parámetros de solicitud POST primero:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Tenga en cuenta que el control UpdatePanel ha indicado al código del lado servidor AJAX precisamente se activó el árbol de control a través del parámetro ScriptManager1: `Button1` de la `UpdatePanel1` control. Ahora, haga clic en el botón de actualización ambos paneles. Examinar la respuesta, a continuación, vemos una serie delimitada por barras verticales de las variables establecidas en una cadena; en concreto, vemos el UpdatePanel superior, `UpdatePanel1`, tiene la totalidad de su HTML enviado al explorador. Sustituye de la biblioteca de scripts de cliente AJAX HTML original de UpdatePanel contenido con el nuevo contenido a través de la `.innerHTML` propiedad, por lo que el servidor envía el contenido cambiado desde el servidor como HTML.

Ahora, haga clic en el botón de actualización ambos paneles y examinar los resultados desde el servidor. Los resultados son muy similares: ambos UpdatePanels recibir nuevo código HTML del servidor. Al igual que con la devolución de llamada anterior, se envía el estado de página adicionales.

Como podemos ver, porque no se utiliza ningún código especial para llevar a cabo una devolución de AJAX, la biblioteca de scripts de cliente de AJAX es capaz de interceptar devoluciones de datos de formulario sin ningún código adicional. Los controles de servidor automáticamente usan JavaScript para que automáticamente no enviar el formulario: ASP.NET inserta automáticamente código para la validación de formulario y estado ya, que se consigue principalmente mediante la inclusión de recursos de script automático, la clase PostBackOptions y la clase ClientScriptManager.

Por ejemplo, considere la posibilidad de un control de casilla de verificación; Examine el desensamblado de clase en .NET Reflector. Para ello, asegúrese de que esté abierto el ensamblado System.Web y navegue hasta la `System.Web.UI.WebControls.CheckBox` (clase), abra el `RenderInputTag` método. Busque una instrucción condicional que comprueba el `AutoPostBack` propiedad:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Cuando se habilita el postback automático en un `CheckBox` control (a través de la propiedad AutoPostBack sea true), el resultante `<input>` etiqueta, por tanto, se representa con una secuencia de comandos de control de eventos ASP.NET su `onclick` atributo. La intercepción de envío del formulario, a continuación, permite a ASP.NET AJAX para insertarse en la página nonintrusively, lo que ayuda a evitar cualquier posible cambios importantes que pueden producirse al utilizar un reemplazo de cadena posiblemente imprecisa. Además, esto permite *cualquier* control ASP.NET personalizado para utilizar la eficacia de AJAX de ASP.NET sin ningún código adicional para admitir su uso dentro de un contenedor de UpdatePanel.

El `<triggers>` funcionalidad corresponde a los valores inicializados en la llamada PageRequestManager a \_updateControls (tenga en cuenta que la biblioteca de scripts de cliente AJAX de ASP.NET utiliza la convención de que los métodos, eventos y los nombres de campo que empiezan con un carácter de subrayado están marcadas como internas y no están diseñados para su uso fuera de la propia biblioteca). Con él, nos podemos observar qué controles están diseñados para hacer que las devoluciones de AJAX.

Por ejemplo, vamos a agregar dos controles adicionales a la página, dejando un control fuera de los UpdatePanels por completo y dejando uno dentro de un UpdatePanel. Se agregará un control de casilla en el UpdatePanel superior y colocar DropDownList con un número de colores definidos dentro de la lista. Este es el nuevo marcado:

**Listado 3: Nuevo marcado**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Y este es el nuevo código subyacente:

**Listado 4: código subyacente**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

La idea detrás de esta página es que la lista desplegable, selecciona uno de tres colores para mostrar la segunda etiqueta, que la casilla de verificación determina si está en negrita, tanto si las etiquetas muestran la fecha, así como el tiempo. La casilla de verificación no debería causar una actualización de AJAX, pero debe la lista desplegable, aunque no lo está alojado dentro de un UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Como es evidente en la captura de pantalla anterior, se encontraba el botón más reciente que se hizo clic en el Panel de esta actualización, que actualiza el superior independientemente de la hora de la parte inferior del botón secundario. La fecha también estaba desactivada entre los clics, como la fecha está visible en la etiqueta inferior. Por último de interés es el color de la etiqueta inferior: se ha actualizado más recientemente que el texto de la etiqueta, que muestra que el estado de control es importante, y los usuarios esperan que se conserva a través de las devoluciones de AJAX. *Sin embargo*, no se actualizó el tiempo. El tiempo se vuelve a llenar automáticamente a través de la persistencia de la \_ \_campo VIEWSTATE de la página que se va a interpretar el tiempo de ejecución de ASP.NET cuando el control se vuelven a representar en el servidor. El código de servidor ASP.NET AJAX no reconoce en el que los métodos de los controles cambian de estado; simplemente vuelve a llenar del estado de vista y, a continuación, ejecuta los eventos que son adecuados.

Se debe señalar, sin embargo, que había inicializa el tiempo dentro de la página\_eventos de carga, el tiempo que habría ha incrementado correctamente. Por lo tanto, los desarrolladores deben tener cuidado con la que se está ejecutando el código adecuado durante los controladores de eventos adecuado y evite el uso de la página\_cargar cuando un controlador de eventos de control sería adecuado.

## <a name="summary"></a>Resumen

El control UpdatePanel de ASP.NET AJAX Extensions es versátil y puede usar varios métodos para identificar los eventos de control que deben hacer que se puede actualizar. Es compatible con sus controles secundarios que se actualiza automáticamente, pero también puede responder a eventos de control en otro lugar en la página.

Para reducir la posibilidad de carga de procesamiento del servidor, se recomienda que el `ChildrenAsTriggers` propiedad de un UpdatePanel se establece en `false`, y que los eventos se elegido-en lugar de incluyen de forma predeterminada. Esto también evita que los eventos innecesarios de causar efectos potencialmente no deseado, como la validación y los cambios realizados en los campos de entrada. Estos tipos de errores pueden ser difíciles aislar, porque la página actualizaciones de forma transparente para el usuario y la causa, por tanto, puede no ser evidente de inmediato.

Examinando el funcionamiento interno de la forma de AJAX de ASP.NET después de modelo de interceptación, pudimos determinar que utiliza el marco de trabajo ya proporcionada por ASP.NET. De esta manera, se conserva la máxima compatibilidad con controles diseñados con el mismo marco de trabajo y mínimamente entra en cualquier código JavaScript escrito para la página.

## <a name="bio"></a>Biografía

Rob Paveza es un desarrollador jefe de aplicaciones de .NET en Terralever ([www.terralever.com](http://www.terralever.com)), una empresa líder en marketing interactiva en Tempe, AZ. Puede ponerse en [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), y se encuentra en su blog [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Siguiente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
