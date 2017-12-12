---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: "Descripción de los desencadenadores ASP.NET AJAX UpdatePanel | Documentos de Microsoft"
author: scottcate
description: Cuando se trabaja en el editor de marcado en Visual Studio, puede observar (de IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de qu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1338ef0763d9bfab451bc30cafa39f715200153d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Descripción de los desencadenadores de UpdatePanel de AJAX de ASP.NET
====================
por [Scott ndicar](https://github.com/scottcate)

[Descarga de PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Cuando se trabaja en el editor de marcado en Visual Studio, puede observar (de IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de los cuales es el elemento de desencadenadores, que especifica los controles en la página (o el control de usuario, si está usando uno) que activará una representación parcial del control UpdatePanel en el que reside el elemento.


## <a name="introduction"></a>Introducción

Tecnología de Microsoft, ASP.NET proporciona un modelo de programación orientada a objetos y orientada a eventos y agrupa las ventajas del código compilado. Sin embargo, su modelo de procesamiento en el servidor tiene varias desventajas inherentes de la tecnología, muchas de las cuales se pueden solucionar si las nuevas características incluidas en las extensiones de AJAX de Microsoft ASP.NET 3.5. Estas extensiones permiten muchas características nuevas de cliente completas, incluida la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad para tener acceso a servicios Web a través de script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API de cliente diseñada para reflejar muchos de los esquemas de control que se muestra en el conjunto de control de servidor ASP.NET.

Estas notas del producto examina la funcionalidad de los desencadenadores de XML de ASP.NET AJAX `UpdatePanel` componente. Desencadenadores de XML proporcionan un control granular sobre los componentes que pueden causar la representación parcial para los controles UpdatePanel específicos.

Estas notas del producto se basan en la versión Beta 2 de .NET Framework 3.5 y Visual Studio 2008. Las extensiones de AJAX de ASP.NET, previamente un ensamblado de complemento destinado a ASP.NET 2.0, ahora están integradas en la biblioteca de clases Base de .NET Framework. Estas notas del producto también se supone que va a trabajar con Visual Studio 2008, no Visual Web Developer Express y le proporcionará los tutoriales de acuerdo con la interfaz de usuario de Visual Studio (aunque estarán completamente compatibles con independencia de los listados de código entorno de desarrollo).

## <a name="triggers"></a>*Desencadenadores*

Desencadenadores para una determinada UpdatePanel, de forma predeterminada, incluyen automáticamente todos los controles secundarios que se invocación una devolución de datos, incluidos los controles de cuadro de texto que tienen (por ejemplo) sus `AutoPostBack` propiedad establecida en **true**. Sin embargo, desencadenadores también se incluye de forma declarativa utilizando marcado; Esto se realiza dentro de la `<triggers>` sección de la declaración del control UpdatePanel. Aunque los desencadenadores son accesibles a través de la `Triggers` propiedad de colección, se recomienda registrar los desencadenadores de representación parcial en tiempo de ejecución (por ejemplo, si un control no está disponible en tiempo de diseño) mediante el uso de la `RegisterAsyncPostBackControl(Control)` método de la ScriptManager de objeto dentro de la página, el `Page_Load` eventos. Recuerde que las páginas son sin estado y, por lo que se deben volver a registrar estos controles cada vez que se crean.

Inclusión de desencadenador automática secundarios también se puede deshabilitar (de modo que los controles secundarios que crear postbacks desencadenará automáticamente crea la representación parcial) estableciendo el `ChildrenAsTriggers` propiedad **false**. Esto le permite la máxima flexibilidad para asignar los controles específicos puede invocar una representación de página y se recomienda, por lo que un programador se participar en para responder a un evento, en lugar de controlar los eventos que pueden surgir.

Tenga en cuenta que cuando están anidados controles UpdatePanel, cuando el UpdateMode está establecido en **condicional**, si el elemento secundario UpdatePanel se desencadene, pero el elemento primario no es así, es, a continuación, solo el elemento secundario UpdatePanel se actualizará. Sin embargo, si el elemento primario UpdatePanel se actualiza, a continuación, el elemento secundario UpdatePanel también se actualiza.

## <a name="the-lttriggersgt-element"></a>*El &lt;desencadenadores&gt; elemento*

Cuando se trabaja en el editor de marcado en Visual Studio, puede observar (de IntelliSense) que hay dos elementos secundarios de un `UpdatePanel` control. El elemento con mayor frecuencia encontrado es el `<ContentTemplate>` elemento, que básicamente encapsula el contenido que se pueden guardar en el panel de actualización (es decir, el contenido para el que nos estamos habilitar la representación parcial). El otro elemento es el `<Triggers>` elemento, que especifica los controles en la página (o el control de usuario, si está usando uno) que activará una representación parcial del control UpdatePanel en el que el &lt;desencadenadores&gt; elemento reside.

El `<Triggers>` elemento puede contener cualquier número cada de dos nodos secundarios: `<asp:AsyncPostBackTrigger>` y `<asp:PostBackTrigger>`. Aceptan dos atributos, `ControlID` y `EventName`y puede especificar cualquier Control dentro de la unidad actual de encapsulación (por ejemplo, si el control UpdatePanel reside dentro de un Control de usuario Web, no debe intentar hacer referencia a un Control en la página en el que residirá el Control de usuario).

El `<asp:AsyncPostBackTrigger>` elemento es especialmente útil en que puede tener como destino cualquier evento de un Control que existe como un elemento secundario de *cualquier* control UpdatePanel en la unidad de encapsulación, no solo el UpdatePanel en las que este desencadenador es un elemento secundario . Por lo tanto, se puede realizar cualquier control para desencadenar una actualización parcial de la página.

De forma similar, la `<asp:PostBackTrigger>` elemento puede utilizarse para representar una página parcial de desencadenador, pero que requiere un envío y recepción en el servidor. Este elemento de desencadenador también se puede usar para forzar una presentación de página completa cuando un control en caso contrario, normalmente activan una representación parcial de la página (por ejemplo, cuando un `Button` control existe en el `<ContentTemplate>` elemento de un control UpdatePanel). Una vez más, el elemento PostBackTrigger puede especificar cualquier control que es un elemento secundario de cualquier control UpdatePanel en la unidad actual de la encapsulación.

## <a name="lttriggersgt-element-reference"></a>*&lt;Desencadenadores&gt; referencia del elemento*

*Descendientes de marcado:*

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica un control y el evento que hará que una actualización parcial de página para el control UpdatePanel que contiene una referencia de desencadenador. |
| &lt;ASP: PostBackTrigger&gt; | Especifica un control y el evento que hará que una actualización de página completa (una actualización de página completa). Esta etiqueta se puede utilizar para forzar una actualización completa cuando un control en caso contrario, se activará la representación parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Tutorial: Entre-UpdatePanel desencadenadores*

1. Crear una nueva página ASP.NET con un objeto ScriptManager que se establecen para habilitar la representación parcial. Agregar dos UpdatePanel a esta página: en la primera, incluyen un control de etiqueta (Label1) y dos controles de botón (Button1 y Button2). Button1 debe indicar, haga clic para actualizarlas y Button2 debe indicar, haga clic para actualizar esta o algo a lo largo de estas líneas. En el segundo UpdatePanel, incluyen un control de etiqueta (Label2), pero establezca la propiedad de color de primer plano en algo distinto del predeterminado para diferenciarla.
2. Establezca la propiedad UpdateMode de las dos etiquetas UpdatePanel a **condicional**.

**Lista 1: Marcado para default.aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. En el controlador de eventos Click para Button1, establezca Label1.Text y Label2.Text a algo dependientes del tiempo (por ejemplo, DateTime.Now.ToLongTimeString()). Para el controlador de eventos Click de Button2, establezca Label1.Text solo en el valor de dependientes del tiempo.

**La lista 2: A código subyacente (recortada) en default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Presione F5 para compilar y ejecutar el proyecto. Tenga en cuenta que, al hacer clic en actualizar los paneles, ambas etiquetas cambian texto; Sin embargo, al hacer clic en el Panel de esta actualización, las actualizaciones solo Label1.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*En segundo plano*

Al utilizar el ejemplo que se acaba de crear, podemos realizar un vistazo a lo que está haciendo AJAX de ASP.NET y el funcionan de nuestro desencadenadores entre-panel de UpdatePanel. Para ello, vamos a trabajar con el origen de la página generada HTML, así como la extensión de Mozilla Firefox, denominada a FireBug - con él, podemos examinar fácilmente las devoluciones de AJAX. También utilizamos la herramienta .NET Reflector de Lutz Roeder. Ambas herramientas están disponibles en línea y pueden encontrarse con una búsqueda en internet.

Un examen del código fuente de página muestra casi nada fuera de lo normal; los controles UpdatePanel se representan como `<div>` contenedores y tenemos podemos ver que el recurso de script incluye proporcionados por el `<asp:ScriptManager>`. También hay algunas nuevas llamadas específico de AJAX a PageRequestManager internos de la biblioteca de script de cliente de AJAX. Por último, vemos que los dos contenedores de UpdatePanel: uno con el representado `<input>` botones con los dos `<asp:Label>` controles que se representan como `<span>` contenedores. (Si examina el árbol DOM en FireBug, observará que las etiquetas aparecen atenuadas para indicar que no se produce contenido visible).

Haga clic en el botón de actualización este Panel y observe que la UpdatePanel superior se actualizará con la hora actual del servidor. En FireBug, elija la pestaña de la consola para que se puede examinar la solicitud. Examine los parámetros de solicitud POST en primer lugar:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Tenga en cuenta que el UpdatePanel ha indicado en el código de AJAX del lado servidor con precisión qué árbol de control se activó a través del parámetro ScriptManager1: `Button1` de la `UpdatePanel1` control. Ahora, haga clic en el botón de actualización los paneles. A continuación, examinar la respuesta, se ve una serie de variables establecidas en una cadena; delimitado por canalización en concreto, se puede ver el UpdatePanel superior, `UpdatePanel1`, tiene la totalidad de su HTML que se envía al explorador. La biblioteca de script de cliente de AJAX sustituye HTML original de UpdatePanel contenido con el nuevo contenido a través de la `.innerHTML` propiedad, de modo que el servidor envía el contenido cambiado desde el servidor como HTML.

Ahora, haga clic en el botón de actualización los paneles y examinar los resultados del servidor. Los resultados son muy similares: ambos UpdatePanel recibe el nuevo código HTML desde el servidor. Al igual que con la devolución de llamada anterior, se envía el estado de página adicionales.

Como se puede observar, porque no se utiliza ningún código especial para llevar a cabo una devolución de AJAX, la biblioteca de script de cliente de AJAX es capaz de interceptar las devoluciones de formularios sin ningún código adicional. Controles de servidor utilizan automáticamente JavaScript para que no envíen automáticamente el formulario: ASP.NET inserta automáticamente código de validación del formulario y el estado ya, que principalmente se logra mediante la inclusión de recursos de secuencia de comandos automática, la clase PostBackOptions y la clase ClientScriptManager.

Por ejemplo, considere la posibilidad de un control CheckBox; Examine el desensamblado de la clase de .NET Reflector. Para ello, asegúrese de que el ensamblado System.Web está abierto y navegue hasta la `System.Web.UI.WebControls.CheckBox` (clase), abra el `RenderInputTag` método. Busque una instrucción condicional que comprueba el `AutoPostBack` propiedad:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Cuando se habilita la devolución de datos automática en un `CheckBox` control (a través de la propiedad AutoPostBack sea true), el resultante `<input>` etiqueta, por tanto, se representa con una secuencia de comandos de control de eventos ASP.NET su `onclick` atributo. La interceptación de envío del formulario, a continuación, permite AJAX de ASP.NET para que se insertará en la página nonintrusively, lo que ayuda a evitar cualquier posibilidad cambios importantes que pueden producirse al utilizar un reemplazo de cadena posiblemente imprecisa. Además, esto permite *cualquier* utilizan la eficacia de AJAX de ASP.NET sin ningún código adicional para admitir su uso dentro de un contenedor de UpdatePanel control personalizado de ASP.NET.

El `<triggers>` funcionalidad corresponde a los valores inicializados en la llamada PageRequestManager a \_updateControls (tenga en cuenta que la biblioteca de scripts de cliente de AJAX de ASP.NET usa la convención que métodos, eventos y nombres de campo que comiencen con un carácter de subrayado están marcados como internos y no están diseñadas para su uso fuera de la propia biblioteca). Con él, se puede observar qué controles están diseñados para hacer que las devoluciones de AJAX.

Por ejemplo, vamos a agregar dos controles adicionales a la página, dejando un control fuera el UpdatePanel por completo y dejando uno dentro de un UpdatePanel. Se agregará un control CheckBox en UpdatePanel superior y colocar un DropDownList con un número de colores definidos en la lista. Este es el marcado nuevo:

**Listado 3: Nueva marca**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Y este es el nuevo código subyacente:

**Listado 4: código subyacente**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

La idea que subyace en esta página es que la lista desplegable selecciona uno de tres colores para mostrar la segunda etiqueta, que la casilla de verificación determina si está en negrita, tanto si las etiquetas muestran la fecha, así como el tiempo. La casilla de verificación no debería causar una actualización de AJAX, pero debe la lista desplegable, aunque no se encuentran dentro de un UpdatePanel.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Como es evidente en la captura de pantalla anterior, se encontraba el botón más reciente que se hizo clic en el botón derecho el Panel de esta actualización, que actualiza el superior independientemente de la hora de la parte inferior. La fecha también se desactivarse entre los clics, como la fecha está visible en la etiqueta de la parte inferior. Por último, es el color de la etiqueta de la parte inferior: se actualizó más recientemente que el texto de la etiqueta, que muestra que el estado de control es importante, y los usuarios esperan que se conserve a través de devoluciones de AJAX. *Sin embargo*, no se actualizó el tiempo. La hora se vuelve a llenar automáticamente a través de la persistencia de la \_ \_campo de estado de vista de la página que se interpreta en tiempo de ejecución de ASP.NET cuando el control se vuelven a representar en el servidor. El código de servidor de AJAX de ASP.NET no reconoce en el que los controles de métodos están cambiando el estado; basta con volver a llenar del estado de vista y, a continuación, ejecuta los eventos que son adecuados.

Se debe señalarse, sin embargo, que había ha inicializado la hora en la página\_evento Load, la hora habría ha incrementado correctamente. Por lo tanto, los desarrolladores deben tener cuidado de que se está ejecutando el código adecuado durante los controladores de eventos apropiado y evite el uso de la página\_se carguen cuando un controlador de eventos de control sería adecuado.

## <a name="summary"></a>Resumen

El control de ASP.NET AJAX Extensions UpdatePanel es versátil y puede utilizar varios métodos para identificar los eventos de control que pueda provocar que se actualice. Se admite que se actualizan de forma automática sus controles secundarios, pero también puede responder a eventos de control en otro lugar en la página.

Para reducir la posibilidad de carga de procesamiento del servidor, se recomienda que el `ChildrenAsTriggers` propiedad de un UpdatePanel se establece en `false`, y que los eventos se participa en lugar de incluido de forma predeterminada. Esto también evita que los eventos innecesarios de causar efectos potencialmente no deseado, incluida la validación y cambios en campos de entrada. Estos tipos de errores pueden ser difíciles aislar, porque la página actualizaciones de forma transparente para el usuario y la causa, por tanto, puede no ser obvia.

Mediante el examen de los mecanismos internos de la forma de AJAX de ASP.NET después de modelo de interceptación, hemos podidos determinar que utiliza el marco de trabajo ya proporcionada por ASP.NET. Si lo hace, se conserva la máxima compatibilidad con controles diseñado con el mismo marco de trabajo y mínimamente entra en todo el código JavaScript adicional escrito para la página.

## <a name="bio"></a>Biografía del

Rob Paveza es un desarrollador de aplicaciones .NET senior en Terralever ([www.terralever.com](http://www.terralever.com)), una empresa de marketing interactiva inicial en Tempe, AZ. Puede ponerse en [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), y se encuentra en su blog [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott categoría ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el director general de myKB.com ([www.myKB.com](http://www.myKB.com)) donde está especializado en la escritura de ASP.NET en función de las aplicaciones que se centra en las soluciones de Software de Base de conocimiento. Scott se puede contactar a través de correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
[Siguiente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
