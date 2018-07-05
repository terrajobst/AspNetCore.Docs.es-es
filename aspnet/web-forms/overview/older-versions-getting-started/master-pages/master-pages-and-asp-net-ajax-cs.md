---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Páginas maestras y ASP.NET AJAX (C#) | Microsoft Docs
author: rick-anderson
description: Describe las opciones para el uso de AJAX de ASP.NET y páginas maestras. Examina el uso de la clase ScriptManagerProxy; Describe cómo los distintos archivos JS se cargan dependi...
ms.author: aspnetcontent
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: e8a4f9780b41c5ff77b996894d9f91a532877245
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842489"
---
<a name="master-pages-and-aspnet-ajax-c"></a>Páginas maestras y ASP.NET AJAX (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) o [descargar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Describe las opciones para el uso de AJAX de ASP.NET y páginas maestras. Examina el uso de la clase ScriptManagerProxy; Describe cómo se cargan los distintos archivos JS dependiendo de si se utiliza el control ScriptManager en el maestro de página o página de contenido.


## <a name="introduction"></a>Introducción

Durante los últimos años, han estado compilando más desarrolladores [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming))-aplicaciones web compatibles. Un sitio Web con AJAX habilitado, usa una serie de tecnologías web relacionadas para ofrecer una experiencia de usuario con más capacidad de respuesta. Creación de aplicaciones habilitadas para AJAX de ASP.NET es sorprendentemente sencilla gracias a Microsoft [marco de ASP.NET AJAX](../../../../ajax/index.md). AJAX de ASP.NET está integrada en ASP.NET 3.5 y Visual Studio 2008; También está disponible como descarga independiente para las aplicaciones de ASP.NET 2.0.

Al crear páginas web habilitadas para AJAX con el marco de AJAX de ASP.NET, debe agregar precisamente uno [control ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) a cada página que usa el marco de trabajo. Como su nombre implica, el control ScriptManager administra el script de cliente utilizado en páginas web habilitadas para AJAX. Como mínimo, el control ScriptManager emite código HTML que indica al explorador para descargar los archivos de JavaScript que utilizan la biblioteca de cliente AJAX de ASP.NET. También puede usarse para registrar los archivos personalizados de JavaScript, los servicios web habilitados para escritura y funcionalidad del servicio de aplicación personalizada.

Si el servidor maestro de sitio usa páginas (como debería), no necesariamente deberá agregar un control ScriptManager a cada página de contenido única; en su lugar, puede agregar un control ScriptManager en la página maestra. Este tutorial muestra cómo agregar el control ScriptManager a la página maestra. También estudia cómo usar el control ScriptManagerProxy para registrar scripts personalizados y servicios de script en una página de contenido específico.

> [!NOTE]
> En este tutorial no explorar diseñar o creación de aplicaciones web habilitadas para AJAX con el marco de AJAX de ASP.NET. Para obtener más información sobre el uso de AJAX, consulte el [vídeos de ASP.NET AJAX](../../../videos/aspnet-ajax/index.md) y [tutoriales](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md), así como los recursos enumerados en la sección Lecturas adicionales al final de este tutorial.


## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examinar el marcado emitido por el Control ScriptManager

El control ScriptManager emite el marcado que indica al explorador para descargar los archivos de JavaScript que utilizan la biblioteca de cliente AJAX de ASP.NET. También se agrega un poco de JavaScript alineado a la página que se inicializa esta biblioteca. El marcado siguiente muestra el contenido que se agrega a la salida representada de una página que incluye un control ScriptManager:


[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

El `<script src="url"></script>` etiquetas indiquen al explorador que descargue y ejecute el archivo de JavaScript en *url*. El control ScriptManager emite tres etiquetas de este tipo; uno hace referencia al archivo `WebResource.axd`, mientras que los otros dos hacer referencia al archivo `ScriptResource.axd`. Estos archivos no existen realmente como archivos en su sitio Web. En su lugar, cuando llega una solicitud para cualquiera de estos archivos en el servidor web, el motor ASP.NET examina la cadena de consulta y devuelve el contenido de JavaScript adecuado. La secuencia de comandos proporcionada por estos tres archivos JavaScript externos constituyen la biblioteca de cliente del marco de ASP.NET AJAX. El otro `<script>` etiquetas emitidas por el control ScriptManager incluyen secuencias de comandos de línea que inicializa esta biblioteca.

Las referencias de script externo y emitidos por el control ScriptManager del script en línea son esenciales para una página que usa el marco de AJAX de ASP.NET, pero no es necesaria para las páginas que no utilizan el marco de trabajo. Por lo tanto, podría analizar que es ideal para agregar sólo un ScriptManager a esas páginas que usan el marco de AJAX de ASP.NET. Y esto es suficiente, pero si tiene muchas páginas que usan el marco de trabajo terminará agregar el control ScriptManager a todas las páginas: una tarea repetitiva, citar. Como alternativa, puede agregar un ScriptManager en la página principal, que, a continuación, inserta esta secuencia de comandos necesario en todas las páginas de contenido. Con este enfoque, no es necesario recordar agregar un ScriptManager a una nueva página que usa el marco de AJAX de ASP.NET porque ya está incluido en la página maestra. Paso 1 le guía a través de agregar un ScriptManager en la página maestra.

> [!NOTE]
> Si tiene previsto incluir una funcionalidad de AJAX dentro de la interfaz de usuario de la página maestra, no tiene ninguna opción en la materia: debe incluir el control ScriptManager en la página maestra.


Una desventaja de agregar el control ScriptManager en la página principal es que el script anterior se genera en *cada* página, independientemente de si sea necesario. Claramente, esto conduce al ancho de banda desaprovechado para las páginas que no use las características del marco de AJAX de ASP.NET aún tiene el control ScriptManager incluido (a través de la página maestra). Pero, ¿cuánto se desperdicia el ancho de banda?

- El contenido real emitido por el control ScriptManager (mostrado arriba) totales de un poco más de 1KB.
- Los tres archivos de script externo al que hace referencia el `<script>` elemento, sin embargo, componen aproximadamente 450 KB de datos sin comprimir; en un sitio Web que usa la compresión gzip, se puede reducir este ancho de banda total cerca de 100 KB. Sin embargo, estos archivos de script se almacenan en caché mediante el explorador durante un año, lo que significa que sólo debe descargarse una vez y, a continuación, se pueden reutilizar en otras páginas en el sitio.

En el mejor caso, a continuación, cuando se almacenan en caché los archivos de script, el costo total es de 1KB, que es insignificante. En el peor de los casos, sin embargo - que es cuando los archivos de script aún no se han descargado y el servidor web no usa ningún tipo de compresión, la posición del ancho de banda está alrededor de 450KB, lo que puede agregar en cualquier parte de un segundo o dos a través de una conexión de banda ancha a hasta un minuto para  usuarios a través de módems de acceso telefónico. La buena noticia es que ya se almacenan en caché los archivos de script externo mediante el explorador, este escenario del peor caso se produce con poca frecuencia.

> [!NOTE]
> Si todavía se siente cómodo colocar el control ScriptManager en la página maestra, tenga en cuenta el formulario Web Forms (el `<form runat="server">` marcado en la página maestra). Todas las páginas ASP.NET que usa el modelo de postback deben incluir exactamente un formulario Web Forms. Agregar un formulario Web Forms, agrega contenido adicional: un número de campos de formulario oculto, el `<form>` etiqueta, y, si es necesario, una función JavaScript para iniciar un postback de secuencia de comandos. Este marcado no es necesario para las páginas que no devuelve datos. Quitando el formulario Web Forms de la página maestra y agregarlo manualmente a cada página de contenido que necesita uno, se podría eliminar este marcado extraño. Sin embargo, las ventajas de tener el formulario Web Forms en la página maestra superan los inconvenientes de tener que agregan innecesariamente a determinadas páginas de contenido.


## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Paso 1: Agregar un Control ScriptManager en la página maestra

Todas las páginas web que usa el marco de AJAX de ASP.NET debe contener exactamente un control ScriptManager. Debido a este requisito, normalmente tiene sentido colocar un solo control de ScriptManager en la página maestra para que todas las páginas de contenido tienen el control ScriptManager que se incluyen automáticamente. Además, el control ScriptManager debe preceder a cualquiera de los controles de servidor ASP.NET AJAX, como los controles UpdatePanel y UpdateProgress. Por lo tanto, es mejor colocar el control ScriptManager antes que los controles ContentPlaceHolder dentro del formulario Web.

Abra el `Site.master` página principal y agregue un control ScriptManager en la página en el formulario Web Forms, pero antes la `<div id="topContent">` elemento (consulte la figura 1). Si usas Visual Web Developer 2008 o Visual Studio 2008, el control ScriptManager se encuentra en el cuadro de herramientas en la pestaña Extensiones de AJAX. Si utiliza Visual Studio 2005, deberá instalar el marco de ASP.NET AJAX en primer lugar y agregue los controles al cuadro de herramientas. Visite el [Wiki de AJAX de ASP.NET](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obtener el marco de trabajo para ASP.NET 2.0.

Después de agregar el control ScriptManager en la página, cambiar su `ID` desde `ScriptManager1` a `MyManager`.


[![Agregar el control ScriptManager en la página maestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: agregar el control ScriptManager en la página maestra ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image3.png))


## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Paso 2: Usar el marco de AJAX de ASP.NET desde una página de contenido

Con el control ScriptManager agregado a la página maestra ahora podemos agregar funcionalidad de AJAX de ASP.NET framework en cualquier página de contenido. Vamos a crear una nueva página ASP.NET que muestra un producto seleccionado al azar de la base de datos Northwind. Vamos a usar el control de temporizador del marco de ASP.NET AJAX para actualizar esta pantalla cada 15 segundos, que muestra un nuevo producto.

Empiece por crear una nueva página en el directorio raíz denominado `ShowRandomProduct.aspx`. No se olvide de enlazar esta nueva página a la `Site.master` página maestra.


[![Agregue una nueva página de ASP.NET al sitio Web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: agregue una nueva página de ASP.NET al sitio Web ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image6.png))


Recuerde que en el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial hemos creado una clase de página base personalizada denominada `BasePage` que generó el título de la página si fuese no se establece explícitamente. Vaya a la `ShowRandomProduct.aspx` código subyacente de la página de clase y hacer que se derivan de `BasePage` (en lugar de desde `System.Web.UI.Page`).

Por último, actualice el `Web.sitemap` archivo para incluir una entrada en esta lección. Agregue el siguiente marcado debajo de la `<siteMapNode>` del patrón de la lección de interacción de página de contenido:


[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

La adición de este `<siteMapNode>` elemento se refleja en las lecciones de lista (consulte la figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Visualización de un producto seleccionado al azar

Vuelva a `ShowRandomProduct.aspx`. En el diseñador, arrastre un control UpdatePanel en el cuadro de herramientas en el `MainContent` control de contenido y establezca su `ID` propiedad `ProductPanel`. UpdatePanel representa una región en la pantalla que se puede actualizar de forma asincrónica a través de un postback de página parcial.

Nuestra primera tarea consiste en Mostrar información acerca de un producto seleccionado aleatoriamente dentro de UpdatePanel. Iniciar, arrastre un control DetailsView en UpdatePanel. Establecer el control DetailsView `ID` propiedad `ProductInfo` y borre su `Height` y `Width` propiedades. Expanda la etiqueta inteligente de DetailsView y, en la lista desplegable Elegir origen de datos, decide enlazar DetailsView a un nuevo control SqlDataSource denominado `RandomProductDataSource`.


[![Enlazar DetailsView a un nuevo Control SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: enlazar DetailsView a un nuevo SqlDataSource Control ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image9.png))


Configurar el control SqlDataSource para conectarse a la base de datos Northwind mediante el `NorthwindConnectionString` (que hemos creado en el [ *interactuar con la página maestra desde la página de contenido* ](interacting-with-the-content-page-from-the-master-page-cs.md) tutorial). Cuando elige la configuración de la instrucción select especificar una instrucción SQL personalizada y, a continuación, escriba la siguiente consulta:


[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

El `TOP 1` palabra clave en el `SELECT` cláusula devuelve solo el primer registro devuelto por la consulta. El [ `NEWID()` función](https://msdn.microsoft.com/library/ms190348.aspx) genera una nueva [el valor de identificador único global (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) y puede usarse en un `ORDER BY` cláusula para devolver registros de la tabla en orden aleatorio.


[![Configurar SqlDataSource para devolver un único registro seleccionado al azar](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configurar SqlDataSource para devolver un único registro seleccionado aleatoriamente ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image12.png))


Después de completar al asistente, Visual Studio crea un BoundField para las dos columnas devuelto por la consulta anterior. En este punto marcado declarativo de la página debe ser similar al siguiente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

La figura 5 muestra el `ShowRandomProduct.aspx` página cuando se ve mediante un explorador. Haga clic en el botón de actualización de su explorador para volver a cargar la página. debería ver el `ProductName` y `UnitPrice` valores para un nuevo registro seleccionado aleatoriamente.


[![Se muestra el nombre y el precio de un producto aleatorio](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: se muestra el nombre y el precio del producto aleatoria A ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image15.png))


### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Mostrar automáticamente un nuevo producto cada 15 segundos

El marco de ASP.NET AJAX incluye un control Timer que realiza una devolución de datos en un momento determinado; en el temporizador de devolución de datos `Tick` provoca el evento. Si el control de temporizador se coloca dentro de un UpdatePanel desencadena un postback de página parcial, durante el cual se pueden enlazar los datos a DetailsView para mostrar un nuevo producto seleccionado aleatoriamente.

Para ello, arrastre un control Timer desde el cuadro de herramientas y colóquelo en el control UpdatePanel. Cambiar el temporizador `ID` desde `Timer1` a `ProductTimer` y su `Interval` propiedad de 60000 a 15000. El `Interval` propiedad indica el número de milisegundos entre las devoluciones de datos; si se establece en 15000 hace que el temporizador desencadenar un postback de página parcial cada 15 segundos. En este punto marcado declarativo del temporizador debe ser similar al siguiente:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Crear un controlador de eventos del temporizador `Tick` eventos. En este controlador de eventos es necesario volver a enlazar los datos a DetailsView mediante una llamada a la DetailsView `DataBind` método. Si lo hace, indica a DetailsView para volver a recuperar los datos de su control de origen de datos, que se seleccione y mostrará un nuevo aleatoriamente seleccionado registro (al igual que al volver a cargar la página, haga clic en el botón Actualizar del explorador).


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Eso es todo esto! Volver a visitar la página a través de un explorador. Inicialmente, se muestra información de un producto aleatorio. Si observa la pantalla pacientemente observará que, después de 15 segundos, información acerca de un nuevo producto por arte de magia, reemplaza la presentación existente.

Para ver mejor lo que sucede aquí, vamos a agregar un control de etiqueta para el control UpdatePanel que muestra la hora a la que se actualizó por última vez la presentación. Agregue un control Web Label dentro del UpdatePanel, establezca su `ID` a `LastUpdateTime`y desactive su `Text` propiedad. A continuación, cree un controlador de eventos para el UpdatePanel `Load` eventos y mostrar la hora actual en la etiqueta. (El UpdatePanel `Load` evento se desencadena en cada postback de página completa o parcial.)


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Con este cambio completo, la página incluye el tiempo que se cargó el producto que se muestra actualmente. Figura 6 muestra la página cuando se visita por primera vez. Figura 7 muestra la página de 15 segundos más tarde después de que el control Timer ha "activada" y el control UpdatePanel se actualizó para mostrar información sobre un producto nuevo.


[![Se muestra un producto seleccionado aleatoriamente al cargar la página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: un aleatoriamente Selected Product se muestra al cargar la página ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image18.png))


[![Cada 15 segundos, se muestra un nuevo aleatoriamente Selected Product](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: segundos cada 15 se muestra un nuevo aleatoriamente Selected Product ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image21.png))


## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Paso 3: Usar el Control ScriptManagerProxy

Además de incluir el script necesario para la biblioteca de cliente del marco de ASP.NET AJAX, ScriptManager también puede registrar archivos personalizados de JavaScript, las referencias a los servicios Web habilitados para escritura y autenticación personalizada, autorización y servicios de perfiles. Normalmente tales personalizaciones son específicas de una página determinada. Sin embargo, si la autenticación, las referencias de servicio Web o archivos de script personalizado, autorización o servicios de perfiles se hace referencia en el control ScriptManager en la página principal, a continuación, se incluyen en *todas* páginas en el sitio Web.

Para agregar las personalizaciones de ScriptManager en la página por página usa el control ScriptManagerProxy. Puede agregar un objeto ScriptManagerProxy de a una página de contenido y, a continuación, registrar el archivo JavaScript personalizado, referencia de servicio Web, o autenticación, autorización o servicio de perfil de la ScriptManagerProxy; Esto tiene el efecto del registro de estos servicios para la página de contenido determinado.

> [!NOTE]
> Una página ASP.NET sólo puede tener no más de un control ScriptManager presente. Por lo tanto, no se puede agregar un control ScriptManager a una página de contenido si el control ScriptManager ya está definido en la página maestra. El único propósito de la ScriptManagerProxy es proporcionar una manera para que los desarrolladores definir el control ScriptManager en la página maestra, pero todavía tiene la capacidad de agregar personalizaciones de ScriptManager en la página por página.


Para ver el control ScriptManagerProxy en acción, vamos a aumentar el UpdatePanel en `ShowRandomProduct.aspx` para incluir un botón que usa el script de cliente para pausar o reanudar el control Timer. El control Timer tiene tres métodos del lado cliente que podemos usar para lograr esta funcionalidad deseada:

- `_startTimer()` -se inicia el control Timer
- `_raiseTick()` -hace que el control Timer "tick", con lo que se devolución y generar su `Tick` eventos en el servidor
- `_stopTimer()` -se detiene el control Timer

Vamos a crear un archivo JavaScript con una variable denominada `timerEnabled` y una función denominada `ToggleTimer`. El `timerEnabled` variable indica si el control de temporizador está habilitado o deshabilitado; el valor predeterminado es true. El `ToggleTimer` función acepta dos parámetros de entrada: una referencia para el cliente y el botón pausar y reanudar `id` valor del control Timer. Esta función cambia el valor de `timerEnabled`, obtiene una referencia al control de temporizador, se inicia o detiene el temporizador (dependiendo del valor de `timerEnabled`) y actualiza el texto del botón Mostrar a la "Pausa" o "Reanudar". Esta función se llamará cada vez que se hace clic en el botón pausar y reanudar.

Empiece por crear una nueva carpeta en el sitio Web denominado `Scripts`. A continuación, agregue un nuevo archivo en la carpeta de secuencias de comandos denominada `TimerScript.js` del tipo de archivo de JScript.


[![Agregue un nuevo archivo JavaScript a la carpeta de Scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: agregar un nuevo archivo de JavaScript a la `Scripts` carpeta ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image24.png))


[![Se agregó un nuevo archivo JavaScript en el sitio Web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: se agregó un nuevo archivo de JavaScript en el sitio Web ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image27.png))


A continuación, agregue el script siguiente en el archivo TimerScript.js:


[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Ahora debemos registrar este archivo JavaScript personalizado en `ShowRandomProduct.aspx`. Vuelva a `ShowRandomProduct.aspx` y agregue un control ScriptManagerProxy a la página; establecer su `ID` a `MyManagerProxy`. Para registrar un código de JavaScript personalizado archivo seleccionar el control ScriptManagerProxy en el diseñador y, a continuación, vaya a la ventana Propiedades. Una de las propiedades se titula secuencias de comandos. Selección de esta propiedad muestra el Editor de la colección ScriptReference que se muestra en la figura 10. Haga clic en el botón Agregar para incluir una referencia de script nuevo y, a continuación, escriba la ruta de acceso al archivo de script en la propiedad de ruta de acceso: `~/Scripts/TimerScript.js`.


[![Agregue una referencia de Script al Control ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: agregar una referencia de Script al ScriptManagerProxy Control ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image30.png))


Después de agregar la referencia de script, el control ScriptManagerProxy's declarativo marcado se actualiza para incluir un `<Scripts>` colección con una sola `ScriptReference` entrada, como el siguiente fragmento de marcado se muestra:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

El `ScriptReference` entrada indica la ScriptManagerProxy debe incluir una referencia al archivo JavaScript en su marcado representado. Es decir, mediante el registro personalizado de script en el objeto ScriptManagerProxy el `ShowRandomProduct.aspx` salida representada de la página ahora incluye otro `<script src="url"></script>` etiqueta: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Ahora podemos llamar a la `ToggleTimer` función definida en `TimerScript.js` desde el script de cliente en el `ShowRandomProduct.aspx` página. Agregue el siguiente código HTML en el control UpdatePanel:


[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Esto muestra un botón con el texto "Pausar". Cada vez que se hace clic, la función de JavaScript `ToggleTimer` se llama pasando una referencia para el botón y el valor de identificador del control Timer (`ProductTimer`). Tenga en cuenta la sintaxis para obtener el `id` valor del control Timer. `<%=ProductTimer.ClientID%>` emite el valor de la `ProductTimer` del control Timer `ClientID` propiedad. En el [ *nombres de identificador de Control en las páginas de contenido* ](control-id-naming-in-content-pages-cs.md) tutorial analizamos las diferencias entre el lado del servidor `ID` valor y el lado de cliente resultante `id` valor y cómo `ClientID` devuelve el cliente `id`.

Figura 11 muestra esta página cuando visita primero a través de un explorador. El temporizador se está ejecutando actualmente y actualiza la información de producto mostrado cada 15 segundos. Figura 12 se muestra la pantalla después de que se ha hecho clic el botón de pausa. Al hacer clic en el botón Pausar detiene el temporizador y actualiza el texto del botón para "Reanudar". La información del producto se actualice (y continuar actualizar cada 15 segundos) una vez que el usuario hace clic en Reanudar.


[![Haga clic en el botón de pausa para detener el Control Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: haga clic en el botón de pausa para detener el Control Timer ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image33.png))


[![Haga clic en el botón de reanudación para reiniciar el temporizador](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: haga clic en el botón de reanudación para reiniciar el temporizador ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image36.png))


## <a name="summary"></a>Resumen

Al compilar aplicaciones web habilitadas para AJAX mediante el marco de ASP.NET AJAX es imperativo que todas las páginas web habilitadas para AJAX incluyen un control ScriptManager. Para facilitar este proceso, podemos agregar un ScriptManager en la página principal, en lugar de tener que recordar agregar un ScriptManager a cada página de contenido. Paso 1 se ha explicado cómo agregar el control ScriptManager en la página maestra al paso 2, examinamos la implementación de la funcionalidad de AJAX en una página de contenido.

Si necesita agregar scripts personalizados, las referencias a los servicios Web habilitados para escritura, o personalizada autenticación, autorización o servicios de perfiles a una página de contenido determinado, agregue un control ScriptManagerProxy a la página contenida y, a continuación, configuración el personalizaciones no existe. Paso 3 se examina cómo usar el objeto ScriptManagerProxy para registrar un archivo JavaScript personalizado en una página de contenido específico.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Marco de AJAX de ASP.NET](../../../../ajax/index.md)
- [Tutoriales ASP.NET AJAX](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Creación de interfaz de usuario interactiva con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Utilizar NEWID para ordenar aleatoriamente los registros](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usar el Control Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Siguiente](specifying-the-master-page-programmatically-cs.md)
